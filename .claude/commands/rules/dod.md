---
description: Validar Definition of Done antes de concluir milestone
argument-hint: [milestone-id]
---

# Validação DoD (Definition of Done)

Execute este checklist ANTES de concluir o milestone: **$ARGUMENTS**

## Processo de Validação

Consultando [documents/Roadmap.md](../../documents/Roadmap.md), seção do milestone **$ARGUMENTS**:

## 1. Critérios do DoD

Liste TODOS os critérios do DoD do milestone:

```markdown
### DoD do Milestone $ARGUMENTS

- [ ] Critério 1
- [ ] Critério 2
- [ ] Critério 3
...

Status Atual: X/Y critérios completos (Z%)
```

## 2. Verificação Completa por Categoria

### Código
- [ ] Todos arquivos/módulos implementados
- [ ] Type hints em todas funções públicas
- [ ] Docstrings completas (Google style)
- [ ] Error handling robusto implementado
- [ ] Logs estruturados em operações críticas
- [ ] Sem código comentado ou debug prints
- [ ] Variáveis e funções com nomes descritivos

### Testes
- [ ] Testes unitários escritos
- [ ] Testes de integração (se aplicável)
- [ ] Testes E2E (se aplicável)
- [ ] Todos testes passando (100%)
- [ ] Cobertura adequada (Services 70%+, Models 80%+)
- [ ] Fixtures e mocks apropriados
- [ ] Testes de casos de erro

### Documentação
- [ ] documents/Projeto.md atualizado
- [ ] README.md atualizado (se aplicável)
- [ ] documents/TODO.md checkboxes marcados
- [ ] Comentários inline adequados
- [ ] API endpoints documentados
- [ ] Variáveis de ambiente documentadas no .env.example

### Qualidade
- [ ] Linting sem erros (black, flake8)
- [ ] Formatação consistente (black line-length=100)
- [ ] Sem code smells óbvios
- [ ] Performance aceitável (latências medidas)
- [ ] Memory leaks verificados (se aplicável)
- [ ] Rate limits respeitados

### Segurança
- [ ] Nenhum secret hardcoded
- [ ] Input validation implementada
- [ ] Sanitização de dados implementada
- [ ] Logging não expõe dados sensíveis
- [ ] Whitelist/autenticação funcionando
- [ ] Dependencies sem vulnerabilidades conhecidas

### Métricas (se aplicável)
- [ ] Latência medida e documentada
- [ ] Acurácia validada (se IA)
- [ ] Taxa de sucesso verificada
- [ ] Uso de recursos monitorado
- [ ] Limites de API respeitados

### Evidências
- [ ] Screenshots capturados (se aplicável)
- [ ] Logs de testes salvos
- [ ] Métricas documentadas no Projeto.md
- [ ] Vídeo demo (se milestone final de fase)

### Integração
- [ ] CI/CD passando (se configurado)
- [ ] Docker build funcionando
- [ ] Variáveis de ambiente configuradas
- [ ] Dependencies instaladas corretamente
- [ ] Health checks implementados

## 3. Análise de Pendências

Se algum critério do DoD estiver INCOMPLETO:

### O que está faltando?
```
[Listar critérios incompletos]
```

### Por que não foi completado?
```
[Razões para cada item pendente]
```

### Quanto tempo para completar?
```
[Estimativa para cada item]
```

### Há bloqueios?
```
[Identificar e documentar bloqueios]
```

## 4. Decisão Final

### ✅ DoD 100% Atendido
```
✓ Código completo e testado
✓ Documentação atualizada
✓ Qualidade validada
✓ Segurança verificada
✓ Evidências documentadas

DECISÃO: MILESTONE $ARGUMENTS COMPLETO

Próximos passos:
1. Atualizar Roadmap.md (marcar milestone como ✅)
2. Atualizar TODO.md (marcar todas tarefas do milestone)
3. Commit com mensagem descritiva
4. Preparar para próximo milestone
```

### ❌ DoD Incompleto
```
✗ Critérios pendentes: [X/Y]
✗ Categoria(s) incompleta(s): [listar]

DECISÃO: MILESTONE $ARGUMENTS NÃO ESTÁ COMPLETO

Items pendentes:
- [ ] Pendência 1: [descrição] - [estimativa]
- [ ] Pendência 2: [descrição] - [estimativa]

Próximos passos:
1. Completar items pendentes
2. Re-executar validação DoD
3. Documentar progresso no TODO.md
```

## 5. Comunicação de Status

Preparar relatório de status do milestone:

```markdown
## Status do Milestone $ARGUMENTS

### DoR
✅ Completo (iniciado em [data])

### Entregas
[Listar features/componentes entregues]

### DoD ($STATUS)
- Código: [X/Y] ✓
- Testes: [X/Y] ✓
- Documentação: [X/Y] ✓
- Qualidade: [X/Y] ✓
- Segurança: [X/Y] ✓

### Métricas Alcançadas
[Listar métricas do DoD]

### Evidências
[Links para screenshots, logs, vídeos]

### Próximos Passos
[Se completo: próximo milestone]
[Se incompleto: pendências a resolver]
```

## Regra Crítica

**Milestone só está "Done" quando DoD está 100% atendido.**

Não há meio termo. Se falta 1 item do DoD, o milestone NÃO ESTÁ COMPLETO.

---

Execute esta validação agora para o milestone: **$ARGUMENTS**
