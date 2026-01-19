# plaud4All - TODO List

## Current Milestone: 1.1 - Project Setup & Configuration

### In Progress
- [x] Criar estrutura básica de diretórios
- [x] Criar .claude/settings.json
- [x] Criar .claude/CLAUDE.md
- [x] Criar Roadmap.md
- [x] Criar TODO.md
- [ ] **NEXT:** Criar requirements.txt
- [ ] **NEXT:** Criar .env.example

### Pending (Milestone 1.1)
- [ ] Criar .gitignore adequado
- [ ] Criar config/config.yaml com categorias
- [ ] Atualizar README.md com quickstart
- [ ] Testar instalação de dependencies

---

## Upcoming Milestones

### Milestone 1.2: iPhone Shortcut Setup
- [ ] Documentar criação do Shortcut
- [ ] Testar fluxo iPhone → iCloud
- [ ] Criar screenshots/guia

### Milestone 2.1: Folder Watcher
- [ ] Implementar folder_watcher.py
- [ ] Parser de filename
- [ ] Tests

### Milestone 2.2: Gemini Transcription
- [ ] Implementar transcription/base.py
- [ ] Implementar transcription/gemini.py
- [ ] Tests + SOP

### Milestone 2.3: Notion Integration
- [ ] Setup Notion database
- [ ] Implementar NotionPublisher
- [ ] Tests + SOP

### Milestone 3.1: End-to-End Pipeline
- [ ] Implementar processor.py
- [ ] Implementar main.py
- [ ] Integration tests

### Milestone 3.2: Documentation & Deployment
- [ ] Finalizar README.md
- [ ] Criar docs/setup.md
- [ ] Troubleshooting guide

---

## Progress Tracking

| Milestone | Status | Progress | Completion Date |
|-----------|--------|----------|-----------------|
| 1.1 - Project Setup | 🟢 In Progress | 60% | - |
| 1.2 - iPhone Shortcut | ⏳ Not Started | 0% | - |
| 2.1 - Folder Watcher | ⏳ Not Started | 0% | - |
| 2.2 - Gemini Transcription | ⏳ Not Started | 0% | - |
| 2.3 - Notion Integration | ⏳ Not Started | 0% | - |
| 3.1 - End-to-End Pipeline | ⏳ Not Started | 0% | - |
| 3.2 - Documentation | ⏳ Not Started | 0% | - |

---

## Notes & Decisions

### 2026-01-19
- Decided on Gemini API for MVP (generous free tier + diarization)
- Simplified structure inspired by Annotr but MVP-focused
- BYOK approach for future flexibility
- Focus on iPhone first, Android later

---

## Quick Commands

- `/rules/dor 1.1` - Validate Definition of Ready for current milestone
- `/rules/dod 1.1` - Validate Definition of Done before marking complete
- `/update-docs system` - Update architecture documentation

---

**Last Updated:** 2026-01-19
