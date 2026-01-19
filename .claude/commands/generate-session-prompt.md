---
description: Gera prompt otimizado para retomada de desenvolvimento em nova sessão
argument-hint: [milestone-id]
---

# Gerar Prompt de Retomada de Sessão

Você deve gerar um **prompt inicial estruturado e eficiente** para prosseguir o desenvolvimento em uma nova sessão, especialmente quando a sessão atual ficou extensa e está consumindo muitos tokens.

**Argumentos:** $ARGUMENTS

---

## 🎯 Objetivo

Criar um prompt que:
1. Resume o contexto atual de forma concisa
2. Estabelece objetivos claros
3. Lista próximas tarefas
4. Inclui referências aos arquivos principais
5. Segue princípios de eficiência de tokens

---

## 📋 Processo

### 1. Análise do Estado Atual

**SEMPRE verificar:**
- [x] Roadmap.md - Milestone atual e seu DoR/DoD
- [x] TODO.md - Tarefas pendentes e progresso
- [x] Plans em `.claude/plans/` (se existir plano recente)
- [x] Último commit - O que foi feito recentemente
- [x] Milestones docs em `documents/milestones/` (se relevante)

### 2. Identificar Informações Essenciais

**Extrair:**
- Milestone/tarefa atual (ex: M1.3.5-OPT)
- Objetivo principal (ex: reduzir latência E2E)
- Metas quantitativas (ex: <15s meta stretch, <20s meta realista)
- Contexto mínimo (ambiente validado, docs alinhadas, etc.)
- Próxima fase/tarefa pendente

### 3. Estrutura do Prompt

**Formato obrigatório:**

```markdown
Vamos continuar o desenvolvimento do Annotr AI no milestone [MILESTONE-ID] ([NOME]).

**Referências principais:**
- @[arquivo1] (contexto sobre X)
- @[arquivo2] (tarefas e checklist)
- @[arquivo3] (plano de implementação detalhado)

**Objetivo:** [Descrição concisa do objetivo principal]

**Contexto atual:**
- [Ponto 1 - ambiente/docs/progresso]
- [Ponto 2 - validações concluídas]
- [Ponto 3 - próxima fase]

**Por favor:**
1. [Tarefa 1 - específica e acionável]
2. [Tarefa 2 - com princípio aplicável]
3. [Tarefa 3 - com critério de quando aplicável]
4. [Tarefa 4 - com marcação de conclusão]
```

---

## 🔧 Templates por Tipo de Retomada

### Template 1: Retomada de Planejamento

```markdown
Vamos continuar o desenvolvimento do Annotr AI no milestone [MILESTONE-ID] ([NOME]).

**Referências principais:**
- @.claude/plans/[plano].md (plano de implementação detalhado)
- @documents/core/TODO.md (seção [MILESTONE-ID] com checklist de tarefas)
- @documents/core/Roadmap.md (contexto de DoR/DoD e baseline)

**Objetivo:** [Objetivo principal - ex: Reduzir tempo E2E de 36s → <15s (meta stretch) ou <20s (meta realista)]

**Contexto atual:**
- Planejamento completo documentado em @.claude/plans/[plano].md
- DoR validado ✅
- Próxima fase: [Nome da próxima fase]

**Por favor:**
1. Leia o plano de implementação em @.claude/plans/[plano].md
2. Identifique a próxima tarefa pendente [ ] no TODO.md (seção [MILESTONE-ID])
3. Implemente a tarefa seguindo os princípios de:
   - `/rules/code-quality` (padrões Python)
   - `/rules/collaborative` (chunks ≤100 linhas, comentários explicativos)
   - `/rules/api-integration` (quando aplicável)
4. Atualize o TODO.md marcando [x] ao concluir
```

### Template 2: Retomada de Implementação

```markdown
Vamos continuar a implementação do milestone [MILESTONE-ID] ([NOME]).

**Referências principais:**
- @documents/core/TODO.md (seção [MILESTONE-ID] - [N]% concluído)
- @documents/core/Roadmap.md (DoD e critérios de sucesso)
- @src/backend/[arquivo-modificado] (última implementação)

**Objetivo:** [Objetivo principal]

**Progresso atual:**
- [N]% concluído ([X]/[Y] tarefas)
- Última implementação: [descrição breve]
- Próxima tarefa: [nome da tarefa]

**Por favor:**
1. Revise a tarefa pendente [ ] em TODO.md seção [MILESTONE-ID]
2. Implemente seguindo `/rules/code-quality` e `/rules/collaborative`
3. Execute testes relevantes (unit/integration/e2e conforme `/rules/testing`)
4. Atualize TODO.md marcando [x] ao concluir
```

### Template 3: Retomada de Testes/Validação

```markdown
Vamos validar a implementação do milestone [MILESTONE-ID] ([NOME]).

**Referências principais:**
- @documents/core/Roadmap.md (seção DoD do [MILESTONE-ID])
- @documents/core/TODO.md (checklist de validação)
- @tests/[tipo]/[arquivo-teste].py (testes implementados)

**Objetivo:** Validar DoD antes de considerar milestone completo

**Status atual:**
- Implementação concluída ✅
- Testes pendentes: [lista]
- Métricas a validar: [lista]

**Por favor:**
1. Execute `/rules/dod [MILESTONE-ID]` para verificar critérios
2. Execute testes pendentes conforme `/rules/testing`
3. Valide métricas contra critérios de sucesso do Roadmap
4. Documente evidências em TODO.md
5. Se tudo passar, atualize Roadmap.md marcando milestone como ✅
```

### Template 4: Retomada de Troubleshooting

```markdown
Vamos resolver o issue [DESCRIÇÃO BREVE] no contexto do milestone [MILESTONE-ID].

**Referências principais:**
- @[arquivo-com-erro] (código com issue)
- @documents/core/TODO.md (contexto do milestone)
- @[log-ou-erro] (evidência do problema)

**Problema:** [Descrição concisa do problema]

**Contexto:**
- Sintoma: [o que está acontecendo]
- Esperado: [o que deveria acontecer]
- Tentativas: [o que já foi tentado]

**Por favor:**
1. Analise o erro em @[arquivo-com-erro]
2. Consulte `/rules/api-integration [api-name]` se for erro de API externa
3. Implemente correção seguindo `/rules/code-quality`
4. Teste para confirmar resolução
5. Considere criar SOP via `/update-docs sop [nome-do-erro]` se for erro não documentado
```

---

## 🎨 Exemplo Real (Baseado na Imagem)

### Prompt Gerado para M1.3.5-OPT

```markdown
Vamos continuar o desenvolvimento do Annotr AI no milestone M1.3.5-OPT (Otimização do Pipeline E2E).

**Referências principais:**
- @.claude/plans/mellow-crunching-sundae.md (plano de implementação detalhado)
- @documents/core/TODO.md (seção M1.3.5-OPT com checklist de tarefas)
- @documents/milestones/E2E_RETOMADA_DESENVOLVIMENTO.md (contexto e baseline)

**Objetivo:** Reduzir tempo E2E de 36s → <15s (meta stretch) ou <20s (meta realista)

**Por favor:**
1. Leia o plano de implementação em @.claude/plans/mellow-crunching-sundae.md
2. Identifique a próxima tarefa pendente [ ] no TODO.md (seção M1.3.5-OPT)
3. Implemente a tarefa seguindo os princípios de:
   - `/rules/code-quality` (padrões Python)
   - `/rules/collaborative` (chunks ≤100 linhas, comentários explicativos)
   - `/rules/api-integration` (quando aplicável)
4. Atualize o TODO.md marcando [x] ao concluir

**Contexto atual:**
- Ambiente validado ✅
- Documentação alinhada ✅
- Progresso: 13% (2/15 tarefas)
- Próxima fase: Criar script de benchmark automatizado
```

---

## ⚙️ Regras de Geração

### SEMPRE:

1. **Ser conciso** - Máximo 250 tokens no prompt
2. **Usar @ references** - Facilita navegação do Claude
3. **Incluir métricas** - Quando relevantes (ex: progresso %, tempo)
4. **Referenciar comandos** - `/rules/X` quando aplicável
5. **Listar próximas tarefas** - Max 4 tarefas específicas
6. **Manter contexto mínimo** - Apenas o essencial para continuar
7. **Usar checkboxes** - Para tarefas acionáveis
8. **Incluir validações** - Quando em fase de testes/DoD

### NUNCA:

1. ❌ Incluir histórico completo da sessão anterior
2. ❌ Duplicar conteúdo já nos arquivos referenciados
3. ❌ Usar descrições genéricas (ex: "continue o trabalho")
4. ❌ Omitir referências aos arquivos principais
5. ❌ Criar prompts > 300 tokens sem justificativa
6. ❌ Esquecer de mencionar milestone/tarefa atual

---

## 🔄 Fluxo de Uso

### Cenário 1: Sessão de Planejamento Ficou Extensa

```bash
# No final da sessão de planejamento no Claude Code:
"A sessão ficou extensa. Gere um prompt inicial para eu continuar 
o desenvolvimento em uma nova sessão."

# Claude executa internamente:
/generate-session-prompt [milestone-id]

# Output: Prompt estruturado pronto para copiar e colar
```

### Cenário 2: Retomando Trabalho Após Pausa

```bash
# Usuário abre nova sessão e quer continuar de onde parou
/generate-session-prompt [milestone-id]

# Claude analisa estado atual e gera prompt apropriado
```

### Cenário 3: Mudança de Contexto (Ex: Cursor → Claude Code)

```bash
# Desenvolvendo no Cursor, quer mover para Claude Code
/generate-session-prompt [milestone-id]

# Gera prompt otimizado para nova ferramenta
```

---

## 📝 Exemplo de Execução Real

### Contexto

Usuário finalizou sessão de planejamento do M1.3.5-OPT e quer continuar em nova sessão.

### Entrada

```
/generate-session-prompt M1.3.5-OPT
```

### Análise Automática

O comando automaticamente:
1. Lê `documents/core/TODO.md` → Identifica progresso 20% (3/15 tarefas)
2. Lê `documents/core/Roadmap.md` → Identifica DoR/DoD do milestone
3. Lê `documents/milestones/E2E_RETOMADA_DESENVOLVIMENTO.md` → Contexto e baseline
4. Verifica última tarefa concluída [x] e próxima pendente [ ]

### Prompt Gerado

```markdown
Vamos continuar o desenvolvimento do Annotr AI no milestone M1.3.5-OPT (Otimização do Pipeline E2E).

**Referências principais:**
- @documents/milestones/E2E_RETOMADA_DESENVOLVIMENTO.md (contexto e baseline)
- @documents/core/TODO.md (seção M1.3.5-OPT - 20% concluído)
- @documents/technical/E2E_PERFORMANCE_MONITORING.md (métricas técnicas)

**Objetivo:** Reduzir tempo E2E de 36s → <15s (meta stretch) ou <20s (meta realista)

**Contexto atual:**
- Ambiente validado ✅ (Docker, Redis, WhatsApp, FastAPI)
- Documentação alinhada ✅ (Roadmap, TODO)
- Progresso: 20% (3/15 tarefas)
- Próxima fase: Criar script de benchmark automatizado

**Por favor:**
1. Leia o contexto completo em @documents/milestones/E2E_RETOMADA_DESENVOLVIMENTO.md
2. Identifique a próxima tarefa pendente [ ] no TODO.md seção "M1.3.5-OPT"
3. Implemente seguindo `/rules/code-quality` e `/rules/collaborative` (chunks ≤100 linhas)
4. Quando aplicável, consulte `/rules/api-integration gemini` para otimizações
5. Atualize TODO.md marcando [x] ao concluir cada tarefa
```

### Como Usar o Prompt Gerado

1. **Copie** o prompt gerado
2. **Abra nova sessão** no Claude Code ou Cursor
3. **Cole o prompt** como primeira mensagem
4. **Continue o desenvolvimento** com contexto limpo e eficiente

### Resultado Esperado

- Sessão nova inicia com contexto essencial (≤250 tokens)
- IA sabe exatamente onde parou e o que fazer
- Referências @ permitem navegação rápida
- Comandos `/rules/X` aplicados conforme necessário

---

## 📊 Validação do Prompt Gerado

Após gerar o prompt, verificar:

- [ ] **Concisão:** Prompt ≤250 tokens (justificar se >300)
- [ ] **Referências:** Inclui @ para arquivos principais (3-5 refs)
- [ ] **Objetivo:** Declarado claramente
- [ ] **Contexto:** 3-5 bullets de status atual
- [ ] **Tarefas:** 3-4 ações específicas com checkboxes
- [ ] **Comandos:** Referencia `/rules/X` quando aplicável
- [ ] **Métricas:** Inclui progresso % ou metas quantitativas
- [ ] **Milestone:** Identifica claramente qual milestone/tarefa

---

## 🎯 Checklist de Eficiência (token.plan.md)

Este comando segue os princípios de eficiência de tokens:

- [x] **Tier A obrigatório:** Roadmap, TODO, planos relevantes
- [x] **Snippets limitados:** Prompt não inclui trechos de código
- [x] **Contexto comprimido:** Resume estado em 3-5 bullets
- [x] **Referências prioritárias:** Usa @ ao invés de duplicar conteúdo
- [x] **Budget respeitado:** Prompt final ≤250 tokens
- [x] **Delta-first approach:** Foca no que mudou e próximos passos

---

## 💡 Dicas de Uso

### Para Sessões Muito Extensas (>150k tokens)

Se a sessão atual está muito extensa:

1. Execute `/generate-session-prompt [milestone-id]`
2. Copie o prompt gerado
3. Abra nova sessão (Claude Code ou Cursor)
4. Cole o prompt
5. Continue o desenvolvimento com contexto limpo

### Para Pivotagem Entre Milestones

Se mudou de foco entre milestones:

1. Execute `/generate-session-prompt [novo-milestone-id]`
2. Use o prompt para reorientar a IA
3. Contexto anterior é descartado, foca no novo objetivo

### Para Handoff (Ex: Claude → Cursor)

Se mudando de ferramenta:

1. Execute `/generate-session-prompt [milestone-id]`
2. Adicione nota sobre ferramenta destino (se necessário)
3. Use o prompt na nova ferramenta

---

## 📚 Documentação Relacionada

- [token.plan.md](../../token.plan.md) - Princípios de eficiência de tokens
- [CLAUDE.md](../.claude/CLAUDE.md) - Regras gerais do projeto
- [Roadmap.md](../../documents/core/Roadmap.md) - Milestones e DoR/DoD
- [TODO.md](../../documents/core/TODO.md) - Tarefas granulares
- [/update-docs](./update-docs.md) - Atualização de documentação

---

## 🔍 Troubleshooting

### Problema: Prompt gerado muito genérico
**Causa:** Milestone não identificado ou TODO.md desatualizado
**Solução:** Atualizar TODO.md primeiro ou especificar milestone manualmente

### Problema: Prompt muito longo (>300 tokens)
**Causa:** Contexto excessivo ou múltiplos objetivos
**Solução:** Focar em um milestone/tarefa específico, resumir contexto

### Problema: Referências quebradas
**Causa:** Arquivos movidos ou renomeados
**Solução:** Executar `/validate-docs-links check` primeiro

### Problema: Falta de próximas tarefas
**Causa:** TODO.md não tem checkboxes ou está vazio
**Solução:** Atualizar TODO.md com tarefas granulares antes de gerar prompt

---

**Execute a geração de prompt conforme o milestone especificado em $ARGUMENTS**

Se nenhum argumento foi fornecido, pergunte ao usuário qual milestone está sendo trabalhado.
