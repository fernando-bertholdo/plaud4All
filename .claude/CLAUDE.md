# Claude Code - Regras do Projeto plaud4All

Este arquivo contém as **regras sempre ativas** para o projeto plaud4All.

---

## 📋 1. Acompanhamento de Roadmap e TODO

### Responsabilidade Contínua

Você DEVE sempre:

#### Antes de começar qualquer tarefa:
- Consultar documents/core/Roadmap.md para verificar o milestone atual
- Verificar o DoR (Definition of Ready) do milestone
- Se DoR não estiver completo, PARE e trabalhe nas dependências primeiro
- Consultar documents/core/TODO.md para tarefas granulares

#### Durante o desenvolvimento:
- Consultar periodicamente o DoD (Definition of Done) no Roadmap
- Usar o DoD como checklist de qualidade
- Marcar checkboxes no TODO.md conforme avança

#### Ao completar uma tarefa:
- Validar TODOS os critérios do DoD no Roadmap.md
- Atualizar checkboxes no TODO.md
- Documentar evidências (testes, logs, exemplos)

### Regra de Ouro

**Se DoR não está completo, NÃO comece. Se DoD não está 100% atendido, NÃO está done.**

---

## 🏗️ 2. Arquitetura plaud4All

### Stack Tecnológica (MVP)

- **Language:** Python 3.9+
- **Transcription:** Gemini API (with BYOK support)
- **Storage:** Notion API
- **File Monitoring:** watchdog
- **Configuration:** pydantic-settings, YAML

### Princípios Arquiteturais

1. **Gemini-First para MVP**
   - Gemini API com diarização via prompting
   - BYOK (Bring Your Own Key)
   - Abstração para futuros backends (Whisper, Parakeet)

2. **Simplicidade > Complexidade**
   - Começar local, pensar cloud depois
   - NÃO adicionar tecnologias sem justificativa

### Fluxos Principais

#### Fluxo 1: Recording (iPhone)
1. User triggers Shortcut
2. Choose category + title
3. Record audio
4. Save as: YYYY-MM-DD_[category]_[title].m4a to iCloud

#### Fluxo 2: Processing Pipeline
1. FolderWatcher detects new file
2. Extract metadata from filename
3. TranscriptionService.transcribe() → text
4. NotionPublisher.create_page() with metadata
5. Archive original file (configurable)

---

## 🔒 3. Segurança

### Regra de Ouro

**NUNCA commitar dados sensíveis**

### Secrets Management

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    GEMINI_API_KEY: str
    NOTION_TOKEN: str
    NOTION_DATABASE_ID: str

    class Config:
        env_file = ".env"
        case_sensitive = True
```

### Checklist de Segurança

- [ ] Nenhum secret hardcoded
- [ ] Todas API keys via .env
- [ ] .env no .gitignore
- [ ] Filename validation implementada
- [ ] Path sanitization implementada

---

## 🤝 4. Desenvolvimento Colaborativo

### Princípios

1. **Chunks ≤100 linhas** - Justificar se exceder
2. **Explicar decisões** - Documentar trade-offs
3. **Referenciar docs** - Links para API docs oficiais
4. **Menos suposições** - Perguntar quando incerto

### APIs Principais

- **Gemini:** https://ai.google.dev/gemini-api/docs/audio
- **Notion:** https://developers.notion.com/reference/intro

---

## 🔄 5. Commits

### Conventional Commits

Format: `<type>(<scope>): <subject>`

Types: feat, fix, docs, refactor, test, chore
Scopes: watcher, transcription, notion, queue, config

### Política de Atribuição

❌ NUNCA mencionar IAs
✅ SEMPRE apresentar como trabalho do desenvolvedor

---

## 📚 6. Comandos

- `/rules/pre-commit` - Checklist pré-commit
- `/rules/dor [milestone]` - Validar DoR
- `/rules/dod [milestone]` - Validar DoD
- `/rules/code-quality` - Padrões de código
- `/update-docs system` - Atualizar arquitetura

---

**Versão:** 0.1.0
**Última atualização:** 2026-01-19
**Autor:** Fernando Bertholdo
