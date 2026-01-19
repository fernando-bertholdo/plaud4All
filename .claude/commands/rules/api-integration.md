---
description: Regras específicas para integração com APIs externas (Gemini, Notion)
argument-hint: [api-name]
---

# Regras de Integração com APIs Externas

Consulte estas regras ao trabalhar com as APIs: **Gemini** ou **Notion**

API solicitada: **$ARGUMENTS**

---

## Gemini API

### Referências Oficiais
- [Audio API](https://ai.google.dev/gemini-api/docs/audio)
- [Long Context](https://ai.google.dev/gemini-api/docs/long-context)
- [Rate Limits](https://ai.google.dev/gemini-api/docs/rate-limits)

### Configuração Obrigatória

```python
import google.generativeai as genai
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    GEMINI_API_KEY: str

    # Rate limits (Free tier)
    GEMINI_MAX_RPM: int = 15  # Requests por minuto
    GEMINI_MAX_RPD: int = 1500  # Requests por dia
    GEMINI_MAX_TPM: int = 1_000_000  # Tokens por minuto

settings = Settings()
genai.configure(api_key=settings.GEMINI_API_KEY)
```

### Modelos Permitidos

```python
# APENAS estes modelos no MVP
GEMINI_MODELS = {
    "transcription": "gemini-2.0-flash",  # Transcrição com diarização
    "embedding": "models/embedding-001"  # Se necessário embeddings
}
```

### Transcrição de Áudio (Plaud Note)

```python
async def transcribe_audio(audio_path: str) -> dict[str, Any]:
    """
    Transcreve áudio do Plaud usando Gemini Audio API.

    IMPORTANTE:
    - Suporta .m4a (formato Plaud Note)
    - Limite: 20MB por arquivo
    - Usar Files API para todos uploads
    - Sempre deletar arquivo após transcrição
    - Incluir diarização via prompt
    """

    try:
        # 1. Upload via Files API
        audio_file = genai.upload_file(path=audio_path)

        # 2. Aguardar processamento
        while audio_file.state.name == "PROCESSING":
            await asyncio.sleep(1)
            audio_file = genai.get_file(audio_file.name)

        # 3. Transcrição com prompt estruturado (inclui diarização)
        model = genai.GenerativeModel("gemini-2.0-flash")

        prompt = """
        Transcreva este áudio em português brasileiro com diarização.

        Identifique cada falante e formate assim:
        [Falante 1]: <texto>
        [Falante 2]: <texto>

        Formato de saída (JSON):
        {
            "text": "transcrição completa com marcadores de falante",
            "language": "pt-BR",
            "confidence": 0.0-1.0,
            "speakers_detected": 2
        }
        """

        response = model.generate_content(
            [audio_file, prompt],
            generation_config={
                "temperature": 0.1,
                "response_mime_type": "application/json"
            }
        )

        # 4. Cleanup OBRIGATÓRIO
        genai.delete_file(audio_file.name)

        return json.loads(response.text)

    except Exception as e:
        logger.error(f"Erro na transcrição Gemini: {e}")
        if 'audio_file' in locals():
            genai.delete_file(audio_file.name)
        raise
```

### Rate Limiting

```python
class GeminiRateLimiter:
    """Rate limiter para Gemini Free Tier."""

    def __init__(self):
        self.minute_requests = deque()
        self.day_requests = deque()

    async def acquire(self):
        """Aguarda até poder fazer request."""
        now = datetime.now()

        # Limpar requests antigos
        minute_ago = now - timedelta(minutes=1)
        while self.minute_requests and self.minute_requests[0] < minute_ago:
            self.minute_requests.popleft()

        # Verificar limites
        if len(self.minute_requests) >= 15:
            wait_time = 60 - (now - self.minute_requests[0]).seconds
            logger.warning(f"Rate limit RPM atingido. Aguardando {wait_time}s")
            await asyncio.sleep(wait_time)

        if len(self.day_requests) >= 1500:
            raise Exception("Rate limit diário atingido (1500 RPD)")

        self.minute_requests.append(now)
        self.day_requests.append(now)
```

---

## Notion API

### Referências Oficiais
- [Getting Started](https://developers.notion.com/reference/intro)
- [Rate Limits](https://developers.notion.com/reference/request-limits)

### Configuração

```python
from notion_client import AsyncClient

class Settings(BaseSettings):
    NOTION_TOKEN: str
    NOTION_DATABASE_ID: str

settings = Settings()
notion = AsyncClient(auth=settings.NOTION_TOKEN)
```

### Rate Limiting

```python
class NotionRateLimiter:
    """Rate limiter para Notion (3 req/s)."""

    def __init__(self, max_per_second: int = 3):
        self.requests = deque()
        self.max_per_second = max_per_second

    async def acquire(self):
        now = datetime.now()

        # Limpar requests > 1s
        second_ago = now - timedelta(seconds=1)
        while self.requests and self.requests[0] < second_ago:
            self.requests.popleft()

        # Aguardar se no limite
        if len(self.requests) >= self.max_per_second:
            wait_time = 1 - (now - self.requests[0]).total_seconds()
            await asyncio.sleep(wait_time)

        self.requests.append(now)
```

### CRUD Operations - plaud4All

```python
async def create_notion_page(
    title: str,
    content: str,
    category: str,
    date: str,
    audio_file: str
) -> str:
    """
    Cria página no Notion com transcrição do Plaud.

    IMPORTANTE:
    - Validar schema antes de criar
    - Usar properties corretos do database
    - Tratar erro 400 (validation)
    - Retry em 429 (rate limit)
    - Incluir metadata do arquivo original
    """

    await notion_limiter.acquire()

    try:
        response = await notion.pages.create(
            parent={"database_id": settings.NOTION_DATABASE_ID},
            properties={
                "Nome": {
                    "title": [{"text": {"content": title}}]
                },
                "Categoria": {
                    "select": {"name": category}
                },
                "Data": {
                    "date": {"start": date}
                },
                "Arquivo Original": {
                    "rich_text": [{"text": {"content": audio_file}}]
                }
            },
            children=[
                {
                    "object": "block",
                    "type": "paragraph",
                    "paragraph": {
                        "rich_text": [{"text": {"content": content}}]
                    }
                }
            ]
        )

        return response["id"]

    except Exception as e:
        logger.error(f"Erro criando página Notion: {e}")
        raise
```

---

## Error Handling Geral

```python
from tenacity import retry, stop_after_attempt, wait_exponential

@retry(
    stop=stop_after_attempt(3),
    wait=wait_exponential(multiplier=1, min=2, max=10)
)
async def resilient_api_call(func, *args, **kwargs):
    """Wrapper para chamadas de API com retry."""

    try:
        return await func(*args, **kwargs)
    except httpx.TimeoutException as e:
        logger.warning(f"Timeout em {func.__name__}: {e}")
        raise
    except Exception as e:
        logger.error(f"Erro em {func.__name__}: {e}")
        raise
```

---

Consulte a documentação completa em [.claude/rules/api-integration-rules.md](../../.claude/rules/api-integration-rules.md)
