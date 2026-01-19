---
description: Atualiza documentação técnica do projeto conforme tipo especificado
argument-hint: [initialize|system|task|sop] [nome-opcional]
---

# Atualização de Documentação Técnica

Você deve atualizar a documentação em `.claude/docs/` conforme o tipo especificado: **$ARGUMENTS**

---

## 📚 Tipos de Documentação

### 1. Initialize (Primeira Execução)
**Uso:** `/update-docs initialize`

**Ação:**
- Criar estrutura `.claude/docs/` (se não existir)
- Criar README.md principal
- Criar templates em `system/` (architecture, api-endpoints, pipeline-flow)
- Criar `.gitkeep` em `tasks/` e `sops/`
- Gerar architecture.md inicial baseado no código atual

**Executar quando:**
- Primeira vez configurando docs
- Após clone do projeto

---

### 2. System (Arquitetura e Sistema)
**Uso:** `/update-docs system [arquivo-opcional]`

**Arquivos disponíveis:**
- `architecture` - Atualiza arquitetura geral
- `pipeline-flow` - Atualiza fluxo do pipeline
- `notion-schema` - Atualiza schema do Notion

**Ação:**
- Analisar código atual em `src/`
- Verificar componentes implementados
- Verificar fluxo do pipeline
- Verificar schemas Pydantic em models/
- Atualizar documentação relevante com status atual
- Marcar como ✅ o que está implementado
- Manter ⏳ o que está pendente

**Executar quando:**
- Após adicionar novo componente
- Após modificar fluxo do pipeline
- Após modificar schema Pydantic
- Após mudanças arquiteturais
- Final de cada milestone

**Exemplo:**
```
/update-docs system architecture
/update-docs system
```

---

### 3. Task (PRD / Implementation Plan)
**Uso:** `/update-docs task [milestone-id]`

**Ação:**
- Salvar o último plan gerado (se houver) em `.claude/docs/tasks/`
- Nomear como `milestone-[FASE].[NUMERO]-[NOME].md` ou `feature-[NOME]-plan.md`
- Incluir no arquivo:
  - Data de criação
  - Status (planejado/em-progresso/completo)
  - Plan de implementação completo
  - Decisões tomadas
  - Trade-offs considerados
  - Referências a docs relacionados
- Atualizar `.claude/docs/README.md` com link para o novo task
- Marcar checkboxes apropriados

**Executar quando:**
- Após gerar plan em Plan Mode
- Antes de iniciar implementação de milestone
- Após implementar feature significativa

**Exemplo:**
```
/update-docs task 1.1
/update-docs task feature-transcription
```

---

### 4. SOP (Standard Operating Procedure)
**Uso:** `/update-docs sop [nome-do-sop]`

**Ação:**
- Criar SOP em `.claude/docs/sops/[nome-do-sop].md`
- Estrutura padrão:
  ```markdown
  # SOP: [Nome]

  ## Quando Usar
  [Descrição de quando aplicar este SOP]

  ## Pré-requisitos
  - Item 1
  - Item 2

  ## Passo a Passo
  1. Passo 1 com detalhes
  2. Passo 2 com detalhes
  ...

  ## Exemplo
  [Código ou exemplo prático]

  ## Troubleshooting
  - Problema X → Solução Y
  - Problema Z → Solução W

  ## Documentação Relacionada
  - [Link para doc 1]
  - [Link para doc 2]
  ```
- Atualizar `.claude/docs/README.md` com link para o novo SOP
- Categorizar apropriadamente (Desenvolvimento, Integrações, Troubleshooting, Processos)

**Executar quando:**
- Após implementar processo repetível com sucesso
- Após resolver erro pela primeira vez
- Após corrigir bug recorrente
- Quando identificar padrão que deve ser seguido

**Exemplos de SOPs úteis:**
```
/update-docs sop setting-up-icloud-sync
/update-docs sop integrating-gemini
/update-docs sop creating-notion-page
/update-docs sop common-gemini-errors
/update-docs sop troubleshooting-folder-watcher
```

---

## 🎯 Regras de Atualização

### SEMPRE:
1. **Ler `.claude/docs/README.md` primeiro** para entender estrutura
2. **Preservar formato existente** nos arquivos
3. **Usar markdown consistente** com resto da doc
4. **Atualizar status** (⏳/✅/🔴) apropriadamente
5. **Atualizar "Última Atualização"** com data atual
6. **Adicionar link** no README.md se criar novo arquivo
7. **Manter checkboxes** sincronizados
8. **Usar exemplos concretos** (código real quando possível)
9. **Referenciar** outros docs relacionados
10. **Ser específico** - evitar descrições genéricas
11. **Verificar Roadmap/TODO** - Ao atualizar milestones/features, verifique se Roadmap.md e TODO.md incluem referências apropriadas a comandos e regras (DoR/DoD, code-quality, testing, api-integration, etc)

### NUNCA:
1. ❌ Deletar conteúdo existente sem justificativa
2. ❌ Criar documentação genérica sem detalhes
3. ❌ Esquecer de atualizar o README.md principal
4. ❌ Misturar status (ex: marcar ✅ algo que está ⏳)
5. ❌ Copiar/colar sem adaptar ao contexto
6. ❌ Documentar código inexistente como se existisse

---

## 📝 Templates de Conteúdo

### Template SOP
```markdown
# SOP: [Nome do Processo]

> **Categoria:** [Desenvolvimento|Integrações|Troubleshooting|Processos]
>
> **Criado em:** [Data]
>
> **Última atualização:** [Data]

## Quando Usar

[Descrição clara de quando este SOP deve ser aplicado]

## Pré-requisitos

- [ ] Pré-requisito 1
- [ ] Pré-requisito 2
- [ ] Pré-requisito 3

## Passo a Passo

### 1. [Nome do Passo]
[Descrição detalhada do passo]

```python
# Código de exemplo
```

### 2. [Nome do Passo]
[Descrição detalhada do passo]

```bash
# Comandos necessários
```

## Exemplo Completo

[Exemplo prático e real de uso]

## Troubleshooting

### Problema: [Descrição do problema]
**Sintoma:** [Como identificar]
**Causa:** [Por que acontece]
**Solução:** [Como resolver]

## Documentação Relacionada

- [Link interno 1]
- [Link interno 2]
- [Link externo 1]

---

**Criado em:** [Data]
**Última atualização:** [Data]
**Usado em:** [Milestone X.Y]
```

### Template Task/PRD
```markdown
# [Milestone X.Y / Feature Name] - Implementation Plan

> **Status:** [🟡 Planejado | 🔵 Em Progresso | ✅ Completo]
>
> **Milestone:** [Fase X, Milestone X.Y]
>
> **Criado em:** [Data]
>
> **Implementado em:** [Data ou "Pendente"]

## Objetivo

[Descrição clara do que será implementado]

## Contexto

[Por que estamos implementando isso? Qual problema resolve?]

## Decisões de Design

### Decisão 1: [Nome]
**Opções consideradas:**
- Opção A: [descrição]
- Opção B: [descrição]

**Escolhida:** Opção A

**Razão:** [Justificativa detalhada]

**Trade-offs:** [O que ganhamos e o que perdemos]

## Arquitetura

[Diagrama ou descrição da arquitetura]

## Implementação

### Componentes a Criar/Modificar
- [ ] `FolderWatcher` - [responsabilidade]
- [ ] `TranscriptionService` - [responsabilidade]

### Fluxos
- [ ] Detecção de novo arquivo
- [ ] Transcrição com Gemini
- [ ] Publicação no Notion

### Models/Schemas
- [ ] `AudioFile` - [campos]
- [ ] `Transcription` - [campos]

### Testes
- [ ] Unit tests para TranscriptionService
- [ ] Integration tests para Gemini API
- [ ] E2E test para fluxo completo

## Critérios de Aceite (DoD)

- [ ] Critério 1
- [ ] Critério 2
- [ ] Critério 3

## Documentação Relacionada

- [Link para SOP relevante]
- [Link para doc de Pipeline]
- [Link para Roadmap]

---

**Criado em:** [Data]
**Última atualização:** [Data]
**Responsável:** Claude Code + Fernando
```

---

## 🔄 Fluxo de Uso

### Cenário 1: Início de Novo Milestone
```
1. Usar Plan Mode para gerar implementation plan
2. /update-docs task [milestone-id]
3. Revisar plan salvo
4. Iniciar implementação
```

### Cenário 2: Após Implementar Feature
```
1. /update-docs system
2. Verificar se architecture.md reflete mudanças
3. Verificar se pipeline-flow.md tem novos componentes
4. Commit
```

### Cenário 3: Após Resolver Bug
```
1. /update-docs sop [nome-do-erro]
2. Documentar causa, sintomas, solução
3. Referenciar no troubleshooting
4. Commit
```

### Cenário 4: Após Integrar Nova API
```
1. /update-docs sop integrating-[api-name]
2. Documentar passo a passo da integração
3. Incluir exemplos de código
4. Incluir common errors
5. Commit
```

---

## 📊 Validação

Após executar qualquer `/update-docs`, verificar:

- [ ] Arquivo foi criado/atualizado em `.claude/docs/`
- [ ] Status (⏳/✅/🔴) está correto
- [ ] Data de "Última atualização" foi atualizada
- [ ] README.md foi atualizado (se novo arquivo)
- [ ] Checkboxes foram atualizados
- [ ] Links estão funcionais
- [ ] Exemplos estão completos e corretos
- [ ] Referências a outros docs estão presentes

### Validação Adicional: Roadmap/TODO

Quando atualizar documentação relacionada a milestones ou features, verifique se Roadmap.md e TODO.md incluem:

- [ ] **DoR sections:** Mencionam `/rules/dor [milestone-id]`
- [ ] **DoD sections:** Mencionam `/rules/dod [milestone-id]`, `/update-docs system`, `/validate-docs-links check`, `/rules/pre-commit`
- [ ] **Durante desenvolvimento:** Mencionam regras temáticas (code-quality, testing, collaborative)
- [ ] **Integrações de APIs:** Mencionam `/rules/api-integration [api-name]` onde aplicável
- [ ] **Seção de comandos úteis:** Existe e está atualizada

Se faltando, considere sugerir adição de referências ao usuário ou use `/audit-roadmap-refs` para análise completa.

---

**Execute a atualização de documentação conforme o tipo especificado em $ARGUMENTS**

Se nenhum argumento foi fornecido, pergunte ao usuário qual tipo de atualização deseja realizar.
