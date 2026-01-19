---
description: Padrões de qualidade de código Python e documentação
argument-hint:
---

# Padrões de Qualidade de Código - plaud4All

Aplique estas regras ao trabalhar com arquivos Python (*.py) e Markdown (*.md).

## Convenções Python

### Nomenclatura
- **Funções e variáveis:** `snake_case`
- **Classes:** `PascalCase`
- **Constantes:** `UPPER_CASE`
- **Privado:** prefixo `_` (ex: `_internal_method`)

### Type Hints (OBRIGATÓRIO)
```python
# ✅ CORRETO
def transcribe(audio_path: str) -> dict[str, Any]:
    """Transcreve áudio usando Gemini."""
    pass

async def create_page(
    database_id: str,
    properties: dict[str, str],
    content: str
) -> str:
    """Cria página no Notion."""
    pass

# ❌ ERRADO
def transcribe(audio_path):  # Sem type hints
    pass
```

### Docstrings (OBRIGATÓRIO)
```python
def extract_metadata(filename: str) -> dict[str, Any]:
    """Extrai metadata do nome do arquivo Plaud.

    Args:
        filename: Nome do arquivo (YYYY-MM-DD_[category]_[title].m4a)

    Returns:
        Dict com 'date', 'category', 'title'

    Raises:
        ValueError: Se filename não segue padrão esperado

    Example:
        >>> metadata = extract_metadata("2026-01-19_work_meeting.m4a")
        >>> metadata['category']
        'work'
    """
    pass
```

### Async/Await
```python
# ✅ CORRETO - Use async para I/O
async def transcribe_file(path: str) -> str:
    async with aiofiles.open(path, 'rb') as f:
        data = await f.read()
        return await gemini_service.transcribe(data)

# ❌ ERRADO - Não use sync para I/O quando async é melhor
def transcribe_file(path: str) -> str:
    with open(path, 'rb') as f:
        data = f.read()
        return gemini_service.transcribe_sync(data)
```

### Error Handling
```python
# ✅ CORRETO - Específico e informativo
try:
    result = await gemini_api.transcribe(audio)
except httpx.TimeoutException as e:
    logger.error(f"Timeout ao transcrever: {e}")
    raise TranscriptionTimeoutError("Gemini timeout") from e
except Exception as e:
    logger.error(f"Erro inesperado: {e}")
    raise

# ❌ ERRADO - Catch genérico sem re-raise
try:
    result = await gemini_api.transcribe(audio)
except:
    print("Erro")  # Nunca usar print, sempre logger
```

### Logging
```python
# ✅ CORRETO - Estruturado e contextual
logger.info(
    "Transcrição iniciada",
    extra={
        "audio_file": filename,
        "category": category,
        "model": "gemini-2.0-flash"
    }
)

# ❌ ERRADO - Print ou log sem contexto
print(f"Transcrevendo {audio_path}")
logger.info("Iniciando")
```

## Formatação e Linting

### Black (line-length=100)
```python
# ✅ CORRETO
long_variable_name = some_function_call(
    parameter_one="value",
    parameter_two="another value",
    parameter_three="yet another value"
)

# Use aspas duplas
message = "Olá, mundo!"
```

### Imports
```python
# Ordem: stdlib → third-party → local
import os
from datetime import datetime
from typing import Any

import httpx
from pydantic import BaseModel

from transcription.gemini_service import GeminiService
from notion.publisher import NotionPublisher
```

## Segurança

### NUNCA Commit
- API keys
- Senhas
- Tokens
- Dados de usuários
- Stack traces com info sensível

### Sempre Validar
- Input do usuário
- Nomes de arquivos
- Caminhos de diretórios
- Respostas de APIs externas
- Tamanho de arquivos
- Rate limits

## Performance

### Cache Quando Possível
```python
from functools import lru_cache

@lru_cache(maxsize=128)
def get_system_prompt(prompt_type: str) -> str:
    """Cache de prompts do sistema."""
    return load_prompt_from_file(prompt_type)
```

### Connection Pooling
```python
class GeminiTranscriptionService:
    def __init__(self):
        self._client = httpx.AsyncClient(timeout=30.0)

    async def close(self):
        await self._client.aclose()
```

## Documentação

### Inline Comments
```python
# Use comentários para explicar "POR QUÊ", não "O QUÊ"

# ✅ CORRETO
# Gemini requer Files API para áudios > 10MB
audio_file = genai.upload_file(audio_path)

# ❌ ERRADO
# Upload do arquivo
audio_file = genai.upload_file(audio_path)
```

## Conventional Commits

```bash
# feat: Nova funcionalidade
feat(watcher): adiciona suporte a detecção de .m4a

# fix: Correção de bug
fix(transcription): corrige timeout em áudios longos

# docs: Documentação
docs: atualiza README com setup de iCloud

# refactor: Refatoração
refactor(notion): extrai lógica de formatação para helper

# test: Testes
test: adiciona testes para folder watcher

# chore: Manutenção
chore: atualiza dependências do requirements.txt
```

## Checklist de Qualidade

Antes de commitar código Python:

- [ ] Type hints em todas funções públicas
- [ ] Docstrings completas (Google style)
- [ ] Error handling implementado
- [ ] Logging estruturado
- [ ] Sem prints ou debug code
- [ ] Imports organizados
- [ ] Black formatado (line-length=100)
- [ ] Sem secrets hardcoded
- [ ] Variáveis com nomes descritivos
- [ ] Testes escritos

---

Aplique estes padrões rigorosamente em TODOS os arquivos Python e Markdown do projeto.
