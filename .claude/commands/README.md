# 📚 Comandos Claude - plaud4All

Este diretório contém todos os comandos disponíveis para automação e manutenção do projeto plaud4All.

---

## 🎯 Comandos Disponíveis

### 1. organize-commits
**Descrição:** Organiza arquivos modificados em commits lógicos

**Uso:** `/organize-commits`

**Quando usar:**
- Acúmulo de múltiplos arquivos modificados
- Fim de milestone
- Antes de criar pull request

### 2. update-docs
**Descrição:** Atualiza documentação técnica

**Uso:**
- `/update-docs initialize`
- `/update-docs system [arquivo]`
- `/update-docs task [milestone-id]`
- `/update-docs sop [nome]`

### 3. audit-rules
**Descrição:** Audita regras e comandos do projeto

**Uso:**
- `/audit-rules quick` - Verificação rápida
- `/audit-rules full` - Análise completa

### 4. audit-roadmap-refs
**Descrição:** Audita referências a comandos no Roadmap/TODO

**Uso:** `/audit-roadmap-refs [--fix] [--output-file]`

### 5. validate-docs-links
**Descrição:** Valida e corrige links de documentação

**Uso:**
- `/validate-docs-links check`
- `/validate-docs-links fix`

### 6. generate-session-prompt
**Descrição:** Gera prompt para retomada de sessão

**Uso:** `/generate-session-prompt [milestone-id]`

---

## 📋 Regras Disponíveis

### Processo
- `/rules/dor [milestone-id]` - Definition of Ready
- `/rules/dod [milestone-id]` - Definition of Done
- `/rules/pre-commit` - Checklist pré-commit

### Qualidade
- `/rules/code-quality` - Padrões Python
- `/rules/testing` - Requisitos de testes
- `/rules/collaborative` - Desenvolvimento colaborativo

### Integrações
- `/rules/api-integration [gemini|notion]` - Regras de API
- `/rules/explain-code` - Explicar código

---

## 🔧 Contexto plaud4All

**Stack:**
- Python pipeline (não FastAPI backend)
- iPhone/iCloud para captura (não WhatsApp)
- Gemini para transcrição
- Notion para publicação

**Componentes:**
- `FolderWatcher` - Detecta arquivos Plaud via iCloud
- `TranscriptionService` - Gemini Audio API
- `NotionPublisher` - Publica no Notion

**Estrutura:**
```
src/
├── watcher/           # Folder monitoring
├── transcription/     # Gemini integration
└── notion/            # Notion publishing
```

---

**Versão:** 1.0.0
**Última atualização:** 2026-01-19
**Mantido por:** Claude Code + Fernando Bertholdo
