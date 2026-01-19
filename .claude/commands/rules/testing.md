---
description: Requisitos e padrões para testes do projeto
argument-hint:
---

# Requisitos de Testes - Annotr AI

Aplique estas regras ao trabalhar com arquivos de teste (test_*.py, *_test.py, tests/**/*.py).

## Estrutura de Testes Obrigatória

```
tests/
├── unit/
│   ├── services/
│   │   ├── test_asr_service.py
│   │   ├── test_llm_service.py
│   │   ├── test_rag_service.py
│   │   └── test_notion_service.py
│   └── models/
│       └── test_schemas.py
├── integration/
│   ├── test_gemini_integration.py
│   ├── test_notion_integration.py
│   └── test_redis_integration.py
├── e2e/
│   ├── test_capture_flow.py
│   ├── test_query_flow.py
│   └── test_command_flow.py
├── fixtures/
│   ├── audio_samples/
│   └── mock_responses/
└── conftest.py
```

## Cobertura Mínima (DoD)

- **Services:** 70% mínimo
- **Routers:** 60% mínimo
- **Models:** 80% mínimo (validação Pydantic)

Verificar cobertura:
```bash
pytest --cov=src --cov-report=html
```

## Nomenclatura de Testes

```python
# ✅ CORRETO - Descritivo e específico
def test_transcribe_returns_valid_json_structure():
    """Testa se transcrição retorna JSON com campos esperados."""
    pass

def test_transcribe_raises_timeout_error_when_api_exceeds_30s():
    """Testa timeout após 30s de espera."""
    pass

def test_classify_intent_returns_capture_for_task_message():
    """Testa classificação de mensagem de tarefa."""
    pass

# ❌ ERRADO - Genérico
def test_transcribe():
    pass

def test_service():
    pass
```

## Estrutura AAA Pattern

```python
def test_structure_content_extracts_title_correctly():
    """Testa extração de título de transcrição."""

    # ARRANGE - Preparar dados
    transcription = "Preciso revisar o relatório amanhã às 10h"
    llm_service = LLMService(api_key="test")

    # ACT - Executar ação
    result = await llm_service.structure_content(transcription)

    # ASSERT - Verificar resultado
    assert result["title"] == "Revisar relatório"
    assert "amanhã" in result["summary"]
    assert result["type"] == "tarefa"
```

## Fixtures (conftest.py)

```python
import pytest
from unittest.mock import AsyncMock, MagicMock

@pytest.fixture
def mock_gemini_api():
    """Mock do Gemini API."""
    with patch("google.generativeai") as mock:
        mock.GenerativeModel.return_value.generate_content = AsyncMock(
            return_value=MagicMock(text='{"intent": "capture", "confidence": 0.95}')
        )
        yield mock

@pytest.fixture
def sample_audio_path(tmp_path):
    """Cria arquivo de áudio temporário para testes."""
    audio_file = tmp_path / "test_audio.mp3"
    audio_file.write_bytes(b"fake audio data")
    return str(audio_file)

@pytest.fixture
def mock_redis_client():
    """Mock do Redis."""
    return AsyncMock()

@pytest.fixture
def mock_notion_client():
    """Mock do Notion client."""
    client = AsyncMock()
    client.pages.create.return_value = {"id": "page-123"}
    return client
```

## Testes Unitários

```python
# tests/unit/services/test_asr_service.py

import pytest
from unittest.mock import patch, AsyncMock
from services.asr_service import ASRService

class TestASRService:
    """Testes para ASRService."""

    @pytest.mark.asyncio
    async def test_transcribe_success(self, mock_gemini_api, sample_audio_path):
        """Testa transcrição bem-sucedida."""
        # ARRANGE
        service = ASRService(api_key="test")

        # ACT
        result = await service.transcribe(sample_audio_path)

        # ASSERT
        assert "text" in result
        assert "confidence" in result
        assert result["language"] == "pt-BR"
        assert 0 <= result["confidence"] <= 1

    @pytest.mark.asyncio
    async def test_transcribe_timeout_raises_error(self, sample_audio_path):
        """Testa timeout em transcrição."""
        # ARRANGE
        service = ASRService(api_key="test")

        with patch("services.asr_service.genai") as mock:
            mock.GenerativeModel.return_value.generate_content = AsyncMock(
                side_effect=TimeoutError()
            )

            # ACT & ASSERT
            with pytest.raises(ASRTimeoutError):
                await service.transcribe(sample_audio_path)
```

## Testes de Integração

```python
# tests/integration/test_gemini_integration.py

import pytest
import os

@pytest.mark.integration
@pytest.mark.asyncio
async def test_gemini_asr_real_api(sample_audio_path):
    """Testa ASR com API real do Gemini (skip se sem API key)."""

    if not os.getenv("GEMINI_API_KEY"):
        pytest.skip("GEMINI_API_KEY não configurada")

    service = ASRService(api_key=os.getenv("GEMINI_API_KEY"))
    result = await service.transcribe(sample_audio_path)

    assert "text" in result
    assert len(result["text"]) > 0
```

## Testes E2E

```python
# tests/e2e/test_capture_flow.py

import pytest
from httpx import AsyncClient
from main import app

@pytest.mark.e2e
@pytest.mark.asyncio
async def test_full_capture_flow():
    """Testa fluxo completo de captura (áudio → Notion)."""

    async with AsyncClient(app=app, base_url="http://test") as client:
        # 1. Simular webhook com áudio
        webhook_data = {
            "message": {
                "type": "audio",
                "url": "https://example.com/audio.mp3",
                "sender": "+5511999999999"
            }
        }

        response = await client.post(
            "/webhook/whatsapp",
            json=webhook_data
        )
        assert response.status_code == 200
```

## Markers Pytest

```python
# pytest.ini

[pytest]
markers =
    unit: Testes unitários rápidos
    integration: Testes de integração com APIs externas
    e2e: Testes end-to-end completos
    slow: Testes que levam > 5s
    benchmark: Testes de performance

addopts =
    -v
    --strict-markers
    --cov=src
    --cov-report=term-missing
    --cov-report=html
```

## Executar Testes

```bash
# Todos testes
pytest

# Apenas unit (rápidos)
pytest -m unit

# Apenas integration (requer APIs)
pytest -m integration

# Sem os lentos
pytest -m "not slow"

# Com cobertura
pytest --cov=src --cov-report=html

# Específico
pytest tests/unit/services/test_asr_service.py::TestASRService::test_transcribe_success
```

## Critérios de Aceitação (DoD)

Para marcar milestone como "done", DEVE ter:

### Testes Unitários
- [ ] Todos services testados (>70% cobertura)
- [ ] Todos models validados
- [ ] Casos de erro cobertos
- [ ] Mocks apropriados

### Testes de Integração
- [ ] APIs externas testadas (se aplicável)
- [ ] Fluxo de dados validado
- [ ] Rate limits respeitados

### Testes E2E
- [ ] Fluxo principal testado
- [ ] Casos de sucesso e falha
- [ ] Performance validada

### Documentação
- [ ] Docstrings em todos testes
- [ ] Fixtures documentadas
- [ ] README de testes atualizado

## Boas Práticas

### Isolamento
```python
# ✅ CORRETO - Cada teste independente
def test_a():
    service = create_service()
    assert service.method() == "expected"

def test_b():
    service = create_service()  # Nova instância
    assert service.other_method() == "expected"
```

### Dados de Teste
```python
# ✅ CORRETO - Fixtures parametrizadas
@pytest.mark.parametrize("text,expected_intent", [
    ("Criar tarefa", "capture"),
    ("Buscar notas", "query"),
    ("Me lembra", "command"),
])
def test_classify_intent(text, expected_intent):
    result = classify_intent(text)
    assert result["intent"] == expected_intent
```

### Async/Await
```python
# ✅ CORRETO - pytest-asyncio
@pytest.mark.asyncio
async def test_async_function():
    result = await async_function()
    assert result == "expected"
```

## Ferramentas

### Pytest Plugins
```txt
# requirements-dev.txt
pytest>=7.0.0
pytest-asyncio>=0.21.0
pytest-cov>=4.0.0
pytest-mock>=3.10.0
pytest-timeout>=2.1.0
httpx>=0.24.0  # Para testes de API
```

---

Aplique estes padrões rigorosamente em TODOS os testes do projeto.
