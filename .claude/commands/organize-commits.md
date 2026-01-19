# /organize-commits - Organizar Commits Automaticamente

**Propósito:** Analisar arquivos modificados e organizá-los em commits lógicos e estruturados automaticamente.

**Quando usar:**
- Acúmulo de múltiplos arquivos modificados não-commitados
- Fim de milestone ou fase de desenvolvimento
- Antes de criar pull request
- Quando explicitamente solicitado pelo usuário

---

## 🎯 Funcionamento

### Passo 1: Análise de Estado

```bash
# Verificar arquivos modificados
git status --porcelain

# Verificar diff statistics
git diff --stat

# Verificar histórico recente
git log --oneline -10
```

**Informações coletadas:**
- Arquivos modificados (M)
- Arquivos novos (??)
- Arquivos deletados (D)
- Contexto de mudanças (linha diff)

### Passo 2: Agrupamento Lógico

Agrupar arquivos por:

1. **Contexto funcional**
   - Features relacionadas
   - Componentes específicos
   - Tipos de mudança (pipeline, docs, tests)

2. **Milestone**
   - Verificar Roadmap.md para milestone atual
   - Agrupar por fase de desenvolvimento

3. **Tipo de mudança**
   - feat: Novas funcionalidades
   - fix: Correções
   - docs: Documentação
   - test: Testes
   - refactor: Refatoração
   - chore: Manutenção

4. **Dependências**
   - Mudanças que devem ser commitadas juntas
   - Ordem de dependência (config antes de código)

### Passo 3: Propor Commits

Para cada grupo, criar commit com:

**Estrutura da mensagem:**
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Elementos:**
- Type: feat, fix, docs, test, refactor, chore
- Scope: pipeline, transcription, notion, watcher, docs, infra
- Subject: Descrição concisa (≤50 chars)
- Body: Bullet points com detalhes
- Footer: Milestone, referências

**⚠️ IMPORTANTE:**
- NÃO incluir menções a assistentes de IA
- NÃO incluir co-autoria com ferramentas automatizadas
- Apresentar como trabalho do desenvolvedor

### Passo 4: Executar Commits

```bash
# Para cada grupo proposto
git add <arquivos_do_grupo>
git commit -m "$(cat <<'EOF'
<mensagem_completa>
EOF
)"
```

**Ordem de execução:**
1. Configurações/dependencies primeiro
2. Core services/models
3. Componentes principais
4. Testes
5. Documentação
6. Arquivos auxiliares

### Passo 5: Validação

Após todos os commits:

```bash
# Verificar working tree limpo
git status

# Mostrar commits criados
git log --oneline -N  # N = número de commits criados
```

---

## 📋 Critérios de Agrupamento

### Por Milestone

**Exemplo: M1.1 - Folder Watcher**

Agrupar:
- `src/watcher/folder_watcher.py` (implementa watcher)
- `requirements.txt` (adiciona watchdog)
- `.env.example` (documenta WATCH_FOLDER)
- `documents/technical/FOLDER_WATCHER_SETUP.md` (guia)

**Commit:**
```
feat(watcher): implementa folder watcher para arquivos Plaud

- Adiciona FolderWatcher usando watchdog
- Atualiza requirements.txt com watchdog==3.0.0
- Documenta WATCH_FOLDER em .env.example
- Cria guia completo de configuração

Decisão técnica: watchdog para cross-platform compatibility
vs inotify (Linux-only)

Milestone: M1.1 (Folder Watcher)
```

### Por Funcionalidade

**Exemplo: Gemini Transcription**

Agrupar:
- `src/transcription/gemini_service.py`
- `documents/technical/GEMINI_INTEGRATION.md`
- `tests/test_gemini_transcription.py`
- `.env.example` (GEMINI_API_KEY)

**Commit:**
```
feat(transcription): implementa transcrição com Gemini

TranscriptionService:
- Gemini Audio API integration
- Retry logic com exponential backoff
- Error handling e logging

Testes:
- Unit tests para GeminiService
- Mock de Gemini API responses

Milestone: M1.2 (Transcription Pipeline)
```

### Por Tipo

**Exemplo: Reestruturação de Testes**

Agrupar:
- `tests/README.md`
- `tests/.env.test`
- `tests/conftest.py`
- `tests/unit/`, `tests/integration/`
- `pytest.ini`

**Commit:**
```
test(pipeline): reestrutura testes em unit/integration

Nova estrutura:
- tests/unit/ - Testes isolados de components
- tests/integration/ - Testes de APIs externas (Gemini, Notion)

Configuração:
- pytest.ini: Markers, coverage, plugins
- conftest.py: Fixtures compartilhadas
- .env.test: Variáveis de teste

Milestone: M1.3 (Testing Infrastructure)
```

---

## 🔍 Heurísticas de Agrupamento

### 1. Mesma Feature/Component

**Se múltiplos arquivos modificam mesmo componente:**

Arquivos:
- `src/transcription/gemini_service.py`
- `src/notion/notion_publisher.py`
- `tests/test_transcription.py`
- `tests/test_notion.py`

**Decisão:**
- Se mudanças são relacionadas (mesma feature) → 1 commit
- Se mudanças são independentes → 2 commits separados

### 2. Dependencies

**Se arquivos têm dependências entre si:**

Ordem correta:
1. `requirements.txt` (adiciona dependência)
2. `src/config.py` (usa dependência)
3. `src/transcription/gemini_service.py` (implementa com dependência)

**Agrupar todos no mesmo commit.**

### 3. Documentação vs Código

**Regra geral:**
- Documentação de feature → mesmo commit da feature
- Documentação técnica standalone → commit separado

**Exemplo:**

Feature + docs:
```
feat(pipeline): implementa pipeline de transcrição

Pipeline:
- FolderWatcher para detectar novos arquivos
- GeminiService para transcrição
- NotionPublisher para publicar resultados

Documentação:
- Guia de uso do pipeline (PIPELINE_GUIDE.md)

Milestone: M1.3
```

Docs standalone:
```
docs(technical): adiciona guias de configuração

- Expande guia de setup do iPhone
- Documenta estratégia de sincronização iCloud
- Adiciona guias de deployment

Milestone: M1.2.5
```

### 4. Tamanho do Commit

**Limite sugerido:**
- ✅ Ideal: 50-200 linhas modificadas
- ⚠️ Aceitável: 200-500 linhas (se relacionadas)
- ❌ Evitar: >500 linhas (quebrar em múltiplos commits)

**Se um grupo excede 500 linhas:**
Quebrar por sub-contexto (ex: separar service de publisher).

---

## 🚨 Casos Especiais

### Arquivos Temporários

**Identificar e remover:**
- `test_*.py` (arquivos de teste ad-hoc)
- `*.pyc`, `__pycache__/`
- `git-history.txt`, `notes.txt`
- `.DS_Store`

**Ação:**
```bash
rm -f <arquivo_temporario>
```

**Não commitar arquivos temporários.**

### Arquivos WIP (Work in Progress)

**Se houver arquivos incompletos:**

Opções:
1. **Commitar como WIP** (se há valor)
   ```
   wip(transcription): progresso em implementação de cache

   Parcialmente implementado:
   - [ ] Cache de transcrições
   - [x] Rate limiting
   - [ ] Testes

   IMPORTANTE: Commit WIP, não está completo
   ```

2. **Não commitar** (deixar para próxima sessão)
   - Usar `git stash` se necessário

### Mudanças Conflitantes

**Se arquivo tem mudanças de múltiplos contextos:**

Exemplo: `main.py` tem:
- Mudança 1: Adiciona logging
- Mudança 2: Corrige bug de encoding

**Solução:**
- Usar `git add -p` (patch mode) para adicionar por seção
- OU: Fazer 2 commits sequenciais documentando ambas mudanças

---

## 📊 Output Esperado

### Formato de Resposta

Após executar `/organize-commits`, apresentar:

```markdown
# Organização de Commits Concluída

## Commits Criados: 6

### 1. feat(watcher): implementa folder watcher para Plaud
**Arquivos:** 4 modificados
- src/watcher/folder_watcher.py
- requirements.txt
- .env.example
- documents/technical/FOLDER_WATCHER_SETUP.md

### 2. feat(transcription): integração com Gemini Audio API
**Arquivos:** 5 (3 modificados, 2 novos)
- src/transcription/gemini_service.py
- src/transcription/__init__.py
- tests/test_gemini_transcription.py (novo)
- documents/technical/GEMINI_INTEGRATION.md (novo)
- .env.example

[... continuar para todos os commits ...]

## Status Final

```bash
git status
# On branch main
# nothing to commit, working tree clean
```

## Próximos Passos

- Revisar commits criados: `git log --oneline -6`
- Push para remote: `git push origin main`
- Criar pull request (se aplicável)
```

---

## ✅ Checklist de Execução

Ao executar `/organize-commits`, verificar:

- [ ] Analisou `git status --porcelain`
- [ ] Identificou arquivos temporários e removeu
- [ ] Agrupou arquivos por contexto lógico
- [ ] Criou mensagens seguindo conventional commits
- [ ] Mensagens NÃO mencionam assistentes de IA
- [ ] Ordem de commits respeita dependências
- [ ] Todos os arquivos relevantes foram commitados
- [ ] Working tree está clean ao final
- [ ] Apresentou resumo de commits criados

---

## 📚 Referências

- [.claude/rules/commit-frequency.md](../rules/commit-frequency.md) - Regras de commit
- [documents/core/Roadmap.md](../../documents/core/Roadmap.md) - Milestones
- [documents/core/TODO.md](../../documents/core/TODO.md) - Tasks
- [Conventional Commits](https://www.conventionalcommits.org/)

---

**Versão:** 1.0.0
**Última atualização:** 2026-01-19
**Autor:** Fernando Bertholdo
