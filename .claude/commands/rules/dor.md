---
description: Validar Definition of Ready antes de iniciar milestone
argument-hint: [milestone-id]
---

# Validação DoR (Definition of Ready)

Execute este checklist ANTES de iniciar o milestone: **$ARGUMENTS**

## Processo de Validação

Consultando [documents/Roadmap.md](../../documents/Roadmap.md), seção do milestone **$ARGUMENTS**:

## 1. Identificar Pré-requisitos do DoR

Liste TODOS os itens do DoR do milestone:

```markdown
### DoR do Milestone $ARGUMENTS

- [ ] Item 1 do DoR
- [ ] Item 2 do DoR
- [ ] Item 3 do DoR
...
```

## 2. Validar Cada Item

Para CADA item do DoR, verificar:

### Dependências de Milestones Anteriores
- [ ] Todos milestones anteriores estão completos
- [ ] DoD dos milestones anteriores foi 100% atendido
- [ ] Não há tarefas pendentes de milestones anteriores

### Recursos Necessários
- [ ] APIs necessárias estão disponíveis
- [ ] Contas/tokens criados e configurados
- [ ] Ferramentas instaladas e funcionando
- [ ] Ambiente de desenvolvimento pronto

### Documentação de Referência
- [ ] Documentação das APIs lida e compreendida
- [ ] Exemplos de código analisados
- [ ] Best practices estudadas
- [ ] Limitações conhecidas documentadas

### Conhecimento Técnico
- [ ] Stack tecnológica compreendida
- [ ] Padrões arquiteturais claros
- [ ] Fluxos de dados mapeados
- [ ] Testes planejados

## 3. Análise de Bloqueios

Se algum item do DoR estiver INCOMPLETO, identificar:

### O que está faltando?
```
[Listar itens incompletos]
```

### Como resolver?
```
[Ações necessárias para completar cada item]
```

### Estimativa de tempo
```
[Quanto tempo para resolver cada bloqueio]
```

## 4. Decisão Final

### ✅ DoR 100% Completo
```
✓ Todos pré-requisitos validados
✓ Dependências completas
✓ Recursos disponíveis
✓ Documentação estudada

DECISÃO: PODE INICIAR MILESTONE $ARGUMENTS
```

### ❌ DoR Incompleto
```
✗ Items pendentes: [listar]
✗ Bloqueios identificados: [listar]

DECISÃO: NÃO INICIAR - TRABALHAR EM DEPENDÊNCIAS PRIMEIRO

Próximos passos:
1. [Resolver bloqueio 1]
2. [Resolver bloqueio 2]
3. [Re-executar validação DoR]
```

## Regra Crítica

**Se DoR não está 100% completo, você DEVE:**
1. Informar ao usuário o que está faltando
2. Sugerir como completar os itens pendentes
3. NÃO prosseguir com o milestone até DoR estar completo

---

Execute esta validação agora para o milestone: **$ARGUMENTS**
