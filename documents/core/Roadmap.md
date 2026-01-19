# plaud4All - Roadmap MVP

## Visão Geral

**Objetivo:** Criar MVP de sistema de gravação e transcrição usando iPhone + iCloud + Gemini API + Notion

**Foco:** Simplicidade, funcionalidade core, BYOK (Bring Your Own Key)

---

## Fase 1: Foundation & Setup

### Milestone 1.1: Project Setup & Configuration
**Status:** 🟢 In Progress
**Prazo:** Semana 1

**Definition of Ready (DoR):**
- [x] Estrutura de diretórios criada
- [x] .claude/settings.json configurado
- [x] .claude/CLAUDE.md com regras ativas
- [x] Roadmap.md e TODO.md criados
- [ ] requirements.txt definido
- [ ] .env.example documentado

**Objetivos:**
1. Estrutura de projeto completa
2. Configuração de desenvolvimento
3. Documentação base

**Tarefas:**
- [x] Criar estrutura de diretórios (src/, config/, docs/, tests/)
- [ ] Definir dependencies (requirements.txt)
- [ ] Criar .env.example com todas as variáveis
- [ ] Criar .gitignore adequado
- [ ] Configurar config.yaml com categorias de gravação

**Definition of Done (DoD):**
- [ ] requirements.txt completo e testado
- [ ] .env.example documentado com exemplos
- [ ] .gitignore protege secrets
- [ ] config.yaml com todas as categorias (meeting, class, lecture, etc.)
- [ ] Documentação de setup em README.md
- [ ] Pre-commit hooks instalados (opcional para MVP)

---

### Milestone 1.2: iPhone Shortcut Setup
**Status:** ⏳ Não iniciado
**Prazo:** Semana 1

**Definition of Ready (DoR):**
- [ ] M1.1 completado (projeto configurado)
- [ ] Formato de filename definido: YYYY-MM-DD_[category]_[title].m4a
- [ ] Categorias de gravação documentadas em config.yaml
- [ ] iCloud folder path definido

**Objetivos:**
1. Documentar criação do Shortcut
2. Definir fluxo de categorização
3. Estabelecer padrão de nomeação

**Tarefas:**
- [ ] Documentar passo-a-passo criação do Shortcut
- [ ] Definir lista de categorias (meeting, class, lecture, chat, interview, other)
- [ ] Testar fluxo de gravação → nomeação → save to iCloud
- [ ] Criar screenshots/guia visual
- [ ] Documentar fallbacks e edge cases

**Definition of Done (DoD):**
- [ ] shortcuts/README.md completo com passo-a-passo
- [ ] Shortcut testado e funcional
- [ ] Arquivos salvos em iCloud com naming correto
- [ ] Documentação com screenshots
- [ ] Edge cases documentados (ex: título com caracteres especiais)

---

## Fase 2: Core Processing Pipeline

### Milestone 2.1: Folder Watcher Implementation
**Status:** ⏳ Não iniciado
**Prazo:** Semana 2

**Definition of Ready (DoR):**
- [ ] M1.2 completado (Shortcut funcionando)
- [ ] watchdog library estudada
- [ ] iCloud folder path configurado em .env
- [ ] Logging strategy definida

**Objetivos:**
1. Implementar monitoring de pasta iCloud
2. Detectar novos arquivos .m4a
3. Extrair metadata de filename

**Tarefas:**
- [ ] Implementar src/watcher/folder_watcher.py
- [ ] Parsear filename para extrair date, category, title
- [ ] Validar formato de filename
- [ ] Implementar error handling (permission errors, invalid filenames)
- [ ] Adicionar logging estruturado
- [ ] Escrever tests para filename parsing

**Definition of Done (DoD):**
- [ ] FolderWatcher detecta novos arquivos corretamente
- [ ] Metadata extraction funciona para todos os formatos válidos
- [ ] Error handling robusto (invalid filenames, permissions)
- [ ] Logs informativos (structured logging)
- [ ] Tests com cobertura ≥80%
- [ ] Documentado em .claude/docs/system/architecture.md

---

### Milestone 2.2: Gemini Transcription Service
**Status:** ⏳ Não iniciado
**Prazo:** Semana 2-3

**Definition of Ready (DoR):**
- [ ] Gemini API docs estudadas (https://ai.google.dev/gemini-api/docs/audio)
- [ ] GEMINI_API_KEY configurada em .env
- [ ] Rate limits entendidos
- [ ] Diarization strategy decidida (via prompting)

**Objetivos:**
1. Implementar TranscriptionService com Gemini API
2. Suporte a diarização via prompting
3. BYOK (Bring Your Own Key)
4. Abstração para futuros backends

**Tarefas:**
- [ ] Criar src/transcription/base.py (abstract class)
- [ ] Implementar src/transcription/gemini.py
- [ ] Testar com áudios de diferentes tamanhos
- [ ] Implementar retry logic com exponential backoff
- [ ] Adicionar rate limit handling
- [ ] Configurar diarization prompt
- [ ] Escrever tests com mock da Gemini API

**Definition of Done (DoD):**
- [ ] Transcription funcional com Gemini API
- [ ] Diarization via prompt funcionando
- [ ] Rate limit handling implementado
- [ ] Retry logic com exponential backoff
- [ ] Error handling robusto (API errors, network issues)
- [ ] Tests com coverage ≥70%
- [ ] SOP criado: .claude/docs/sops/integrating-gemini-audio.md

---

### Milestone 2.3: Notion Integration
**Status:** ⏳ Não iniciado
**Prazo:** Semana 3

**Definition of Ready (DoR):**
- [ ] Notion API docs estudadas
- [ ] NOTION_TOKEN e NOTION_DATABASE_ID configurados
- [ ] Database schema definido (Title, Date, Category, Transcription, etc.)
- [ ] Rate limits entendidos (3 req/s)

**Objetivos:**
1. Criar database no Notion (via script ou manual)
2. Implementar NotionPublisher
3. Criar páginas com metadata + transcription

**Tarefas:**
- [ ] Criar script scripts/setup_notion_db.py
- [ ] Implementar src/notion/client.py
- [ ] Implementar src/notion/publisher.py
- [ ] Testar criação de páginas
- [ ] Implementar retry logic
- [ ] Adicionar rich text formatting para transcription
- [ ] Escrever tests com mock da Notion API

**Definition of Done (DoD):**
- [ ] Notion database criada (manual ou via script)
- [ ] NotionPublisher cria páginas corretamente
- [ ] Metadata extraída de filename salva corretamente
- [ ] Transcription formatada em rich text
- [ ] Rate limit handling (3 req/s)
- [ ] Tests com coverage ≥70%
- [ ] SOP criado: .claude/docs/sops/creating-notion-pages.md

---

## Fase 3: Integration & Testing

### Milestone 3.1: End-to-End Pipeline
**Status:** ⏳ Não iniciado
**Prazo:** Semana 4

**Definition of Ready (DoR):**
- [ ] M2.1, M2.2, M2.3 completados
- [ ] Todos os serviços individuais testados
- [ ] Queue/processor strategy definida

**Objetivos:**
1. Integrar todos os componentes
2. Implementar queue de processamento
3. Entry point (src/main.py)

**Tarefas:**
- [ ] Implementar src/queue/processor.py
- [ ] Implementar src/main.py com orchestration
- [ ] Testar fluxo completo: iPhone → iCloud → Transcription → Notion
- [ ] Implementar cleanup de arquivos processados
- [ ] Adicionar metrics/logging
- [ ] Escrever integration tests

**Definition of Done (DoD):**
- [ ] Pipeline end-to-end funcional
- [ ] Arquivos processados corretamente arquivados/removidos
- [ ] Logs completos de todo o fluxo
- [ ] Error handling em todos os estágios
- [ ] Integration tests passando
- [ ] Performance aceitável (< 2min por áudio de 1h)

---

### Milestone 3.2: Documentation & Deployment
**Status:** ⏳ Não iniciado
**Prazo:** Semana 4

**Definition of Ready (DoR):**
- [ ] M3.1 completado (pipeline funcionando)
- [ ] Tests passando
- [ ] Todos os DoDs anteriores validados

**Objetivos:**
1. Documentação completa de setup
2. Guia de deployment (local + cloud)
3. README.md final

**Tarefas:**
- [ ] Atualizar README.md com quickstart
- [ ] Criar docs/setup.md detalhado
- [ ] Documentar deployment local (Windows/Mac/Linux)
- [ ] Documentar deployment em VPS (opcional)
- [ ] Criar troubleshooting guide
- [ ] Adicionar exemplos de uso
- [ ] Video demo (opcional)

**Definition of Done (DoD):**
- [ ] README.md completo e claro
- [ ] docs/setup.md passo-a-passo
- [ ] Troubleshooting guide com erros comuns
- [ ] Exemplos de config.yaml e .env
- [ ] Deployment testado em ambiente limpo
- [ ] MVP pronto para uso!

---

## Próximos Passos (Pós-MVP)

- Local Whisper support
- Parakeet TTS integration
- LLM-powered summaries
- Category-specific processing templates
- Web dashboard
- Android support
- Google Drive integration

---

## Comandos Úteis

### Antes de Iniciar Milestone
- `/rules/dor [milestone-id]` - Validar Definition of Ready

### Durante Desenvolvimento
- `/rules/code-quality` - Padrões de código
- `/rules/security` - Best practices de segurança

### Antes de Concluir Milestone
- `/rules/dod [milestone-id]` - Validar Definition of Done
- `/update-docs system` - Atualizar documentação

---

**Versão:** 0.1.0
**Última atualização:** 2026-01-19
