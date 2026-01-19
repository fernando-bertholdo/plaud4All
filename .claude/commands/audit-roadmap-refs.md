---
description: Audita referências a comandos e regras no Roadmap.md e TODO.md
argument-hint: [--fix] [--output-file]
---

# Auditoria de Referências a Comandos no Roadmap/TODO

Você deve auditar se Roadmap.md e TODO.md referenciam comandos e regras apropriadamente: **$ARGUMENTS**

---

## 📋 Objetivo

Garantir que documentos de planejamento (Roadmap.md e TODO.md) incluam referências úteis a comandos e regras, facilitando o desenvolvimento e garantindo que desenvolvedores (humanos e IA) saibam quando acionar cada comando/regra.

---

## 🎯 O Que Verificar

### 1. Seção "Comandos Úteis de Suporte"

**Verificar:**
- [ ] Existe seção "💡 Comandos Úteis" ou similar no início dos documentos
- [ ] Seção lista comandos por categoria (validação, desenvolvimento, manutenção)
- [ ] Comandos listados incluem: `/rules/dor`, `/rules/dod`, `/rules/code-quality`, `/rules/testing`, `/rules/api-integration`, `/update-docs`, `/validate-docs-links`, `/audit-rules`
- [ ] Links para documentação completa estão presentes

**Se faltando:** Sugerir adição de seção formatada similar ao template abaixo

### 2. Referências em Templates DoR/DoD

**Verificar:**
- [ ] Template de DoR menciona comando `/rules/dor [milestone-id]`
- [ ] Template de DoD menciona comandos: `/rules/dod`, `/update-docs system`, `/validate-docs-links check`, `/rules/pre-commit`
- [ ] Templates estão no início dos documentos para fácil acesso

**Se faltando:** Sugerir adição de linha "💡 Validação:" com comando apropriado

### 3. Referências em Milestones Individuais

Para CADA milestone no Roadmap.md e TODO.md, verificar se existe seção "🔧 Comandos Aplicáveis" ou similar com:

**Antes de iniciar:**
- [ ] `/rules/dor [milestone-id]` mencionado

**Durante desenvolvimento:**
- [ ] `/rules/code-quality` mencionado (se envolve código Python)
- [ ] `/rules/testing` mencionado (se envolve testes)
- [ ] `/rules/api-integration [api-name]` mencionado (se integra com APIs externas)
- [ ] `/rules/collaborative` mencionado (para desenvolvimento colaborativo)

**Antes de concluir:**
- [ ] `/rules/dod [milestone-id]` mencionado
- [ ] `/update-docs system` mencionado
- [ ] `/update-docs task [milestone-id]` mencionado (para salvar implementation plan)
- [ ] `/validate-docs-links check` mencionado
- [ ] `/rules/pre-commit` mencionado

**Comandos especializados (quando aplicável):**
- [ ] `scripts/validate_dataset_quality.py` para milestones com ML
- [ ] `scripts/test_svm_generalization.py` para deploy de modelos
- [ ] `/update-docs sop [nome]` para criar SOPs de processos novos
- [ ] `/audit-rules [quick|full]` após mudanças arquiteturais

**Se faltando:** Sugerir adição de seção formatada

### 4. Referências Contextuais em Tarefas Específicas

Verificar tarefas que envolvem:

**Integrações com APIs:**
- [ ] Tarefas Gemini → mencionam `/rules/api-integration gemini`
- [ ] Tarefas Notion → mencionam `/rules/api-integration notion`
- [ ] Tarefas Evolution → mencionam `/rules/api-integration evolution`
- [ ] Tarefas Cerebras → mencionam `/rules/api-integration cerebras`

**Testes e Qualidade:**
- [ ] Seções de testing → mencionam `/rules/testing`
- [ ] Seções de code review → mencionam `/rules/code-quality`
- [ ] Seções de ML/dataset → mencionam scripts de validação

**Documentação:**
- [ ] Após completar milestone → menciona `/update-docs system`
- [ ] Após criar processo novo → menciona `/update-docs sop [nome]`
- [ ] Após reorganizar docs → menciona `/validate-docs-links fix`

---

## 📊 Formato do Relatório

### Estrutura do Relatório

```markdown
# Auditoria de Referências a Comandos - [DATA]

## 📈 Resumo Executivo

- **Roadmap.md:** X/Y milestones com referências (Z%)
- **TODO.md:** X/Y milestones com referências (Z%)
- **Seção "Comandos Úteis":** [Presente ✅ | Ausente ❌ | Desatualizada ⚠️]
- **Templates DoR/DoD:** [Completos ✅ | Incompletos ⚠️ | Ausentes ❌]

## 🔍 Análise Detalhada

### Roadmap.md

#### ✅ Milestones com Referências Completas
- Milestone 1.1: Infraestrutura ✅ (DoR, DoD, comandos durante)
- Milestone 1.2: Gemini ✅ (DoR, DoD, comandos API-specific)

#### ⚠️ Milestones com Referências Parciais
- Milestone 2.1: RAG ⚠️ (Falta `/update-docs sop rag-testing` no DoD)

#### ❌ Milestones sem Referências
- Milestone 3.1: Comandos ❌ (Sem seção "🔧 Comandos Aplicáveis")

### TODO.md

[Análise similar]

### Seção "Comandos Úteis"

**Roadmap.md:**
- Status: ✅ Presente e completa
- Localização: Linha 168
- Comandos listados: 15/16 esperados
- Faltando: `/audit-roadmap-refs` (este comando!)

**TODO.md:**
- Status: ✅ Presente e completa
- Localização: Linha 33
- Comandos listados: 15/16 esperados
- Faltando: `/audit-roadmap-refs`

### Templates DoR/DoD

**Template DoR:**
- Localização: Roadmap.md linha 50-61, TODO.md linha 38-39
- Status: ✅ Completo (menciona `/rules/dor [milestone-id]`)

**Template DoD:**
- Localização: Roadmap.md linha 63-82, TODO.md linha 48-53
- Status: ✅ Completo (menciona todos comandos esperados)

## 🎯 Recomendações

### Prioridade Alta
1. **Milestone 3.1:** Adicionar seção "🔧 Comandos Aplicáveis" com DoR/DoD
2. **Milestone 2.2:** Adicionar `/rules/testing` na seção de comandos

### Prioridade Média
3. **Seção Comandos Úteis:** Adicionar `/audit-roadmap-refs` à lista
4. **Milestone 2.1:** Completar lista de comandos DoD

### Prioridade Baixa
5. **Milestone 1.4:** Considerar adicionar checkpoint scripts em destaque

## 📝 Templates de Correção

### Template: Seção para Milestone Individual

```markdown
**🔧 Comandos Aplicáveis:**
- **Antes de iniciar:** `/rules/dor [milestone-id]` - Validar pré-requisitos
- **Durante implementação:**
  - `/rules/code-quality` - Padrões de código
  - `/rules/testing` - Requisitos de testes
  - `/rules/api-integration [api-name]` - Integração com APIs (se aplicável)
  - `/rules/collaborative` - Desenvolvimento colaborativo
- **Antes de concluir:**
  - `/rules/dod [milestone-id]` - Validar DoD
  - `/update-docs system` - Atualizar docs técnicos
  - `/update-docs task [milestone-id]` - Salvar implementation plan
  - `/validate-docs-links check` - Verificar integridade
  - `/rules/pre-commit` - Checklist antes de commit
```

### Template: Seção "Comandos Úteis" (se ausente)

```markdown
## 💡 Comandos Úteis de Suporte

Para facilitar o desenvolvimento seguindo as regras do projeto, use estes comandos conforme necessário:

### Validação de Milestone
- **Claude:** `/rules/dor [milestone-id]` - Validar Definition of Ready
- **Claude:** `/rules/dod [milestone-id]` - Validar Definition of Done
- **Cursor:** `dor-dod-validation.mdc` - Regra sempre ativa

### Desenvolvimento e Qualidade
- **Claude:** `/rules/code-quality` - Padrões de código Python
- **Claude:** `/rules/testing` - Requisitos de testes
- **Claude:** `/rules/collaborative` - Desenvolvimento colaborativo
- **Claude:** `/rules/api-integration [gemini|notion|evolution|cerebras]` - APIs
- **Cursor:** `code-quality-standards.mdc`, `testing-requirements.mdc` - Sempre ativas

### Documentação e Manutenção
- **Claude:** `/update-docs system` - Atualizar docs técnicos
- **Claude:** `/update-docs task [milestone-id]` - Implementation plan
- **Claude:** `/update-docs sop [nome]` - Criar SOP
- **Claude:** `/validate-docs-links [check|fix]` - Validar/corrigir links
- **Claude:** `/rules/pre-commit` - Checklist antes de commits

### Auditoria
- **Claude:** `/audit-rules [quick|full]` - Auditar regras
- **Claude:** `/audit-roadmap-refs` - Auditar referências a comandos

**📚 Documentação:** Ver [.claude/commands/README.md] e [.claude/rules/README.md]
```
```

---

## 🔧 Modo de Operação

### Modo Padrão (Report Only)
**Uso:** `/audit-roadmap-refs`

**Ação:**
- Analisar Roadmap.md e TODO.md
- Gerar relatório detalhado no formato acima
- Retornar no output do chat
- NÃO fazer mudanças nos arquivos

### Modo Fix (Com Correções)
**Uso:** `/audit-roadmap-refs --fix`

**Ação:**
- Analisar Roadmap.md e TODO.md
- Identificar milestones sem referências
- PERGUNTAR ao usuário quais correções aplicar
- Aplicar correções aprovadas
- Gerar relatório de mudanças aplicadas

**IMPORTANTE:** Modo --fix deve ser interativo e pedir confirmação antes de cada mudança significativa.

### Modo Output File
**Uso:** `/audit-roadmap-refs --output-file`

**Ação:**
- Salvar relatório em `documents/technical/AUDIT_ROADMAP_REFS_[DATA].md`
- Útil para revisão offline ou tracking histórico

---

## 📋 Checklist de Execução

Ao executar este comando, seguir:

1. [ ] Ler Roadmap.md completamente
2. [ ] Ler TODO.md completamente
3. [ ] Verificar seção "Comandos Úteis" em ambos
4. [ ] Verificar templates DoR/DoD em ambos
5. [ ] Para cada milestone identificado:
   - [ ] Verificar se há seção de comandos
   - [ ] Verificar se comandos apropriados estão listados
   - [ ] Identificar comandos faltantes baseado no contexto do milestone
6. [ ] Gerar relatório formatado
7. [ ] Se --fix: Propor correções interativamente
8. [ ] Se --output-file: Salvar em documents/technical/

---

## 🎯 Exemplos de Uso

### Exemplo 1: Auditoria Rápida
```
Usuario: /audit-roadmap-refs

Claude: [Analisa documentos e retorna relatório no chat]

# Auditoria de Referências a Comandos - 2025-10-27

## 📈 Resumo Executivo

- **Roadmap.md:** 8/10 milestones com referências (80%)
- **TODO.md:** 7/10 milestones com referências (70%)
- **Seção "Comandos Úteis":** ✅ Presente em ambos
- **Templates DoR/DoD:** ✅ Completos

## 🔍 Análise Detalhada

[Detalhes...]

## 🎯 Recomendações

[Sugestões...]
```

### Exemplo 2: Auditoria com Correções
```
Usuario: /audit-roadmap-refs --fix

Claude: [Analisa e identifica milestones sem referências]

Encontrei 2 milestones sem referências a comandos:

1. **Milestone 2.1: RAG Long Context** (Roadmap.md, linha 450)
   - Falta seção "🔧 Comandos Aplicáveis"
   - Comandos sugeridos: /rules/dor 2.1, /rules/dod 2.1, /rules/testing, /update-docs sop rag-query

2. **Milestone 3.1: Comandos WhatsApp** (TODO.md, linha 890)
   - Seção existe mas incompleta
   - Faltando: /rules/api-integration evolution

Deseja que eu adicione essas referências? [Sim/Não/Personalizar]

Usuario: Sim

Claude: [Aplica mudanças e confirma]

✅ Adicionado seção de comandos ao Milestone 2.1
✅ Completado seção de comandos do Milestone 3.1

Arquivos modificados:
- Roadmap.md (1 adição)
- TODO.md (1 atualização)

Use `/validate-docs-links check` para verificar integridade dos links.
```

### Exemplo 3: Relatório Salvo
```
Usuario: /audit-roadmap-refs --output-file

Claude: [Gera relatório e salva]

✅ Relatório salvo em: documents/technical/AUDIT_ROADMAP_REFS_2025-10-27.md

Você pode revisar o relatório offline e decidir quais correções aplicar.
Para aplicar correções: `/audit-roadmap-refs --fix`
```

---

**Execute a auditoria de referências conforme o modo especificado em $ARGUMENTS**

Se nenhum argumento foi fornecido, executar em modo padrão (report only).
