---
description: Audita e sugere melhorias para regras e comandos do projeto
argument-hint: [full|quick] [--output-file]
---

# Auditoria de Regras e Comandos

Você deve auditar regras (Claude + Cursor) e comandos do projeto conforme o modo especificado: **$ARGUMENTS**

---

## 📋 Modos de Operação

### 1. QUICK (Padrão)
**Uso:** `/audit-rules` ou `/audit-rules quick`

**Ação:**
- Verificações essenciais e flags óbvios
- Comparação de alto nível entre documentos
- Identificação de regras obviamente obsoletas
- Detecção de inconsistências críticas
- Não propõe novas regras (apenas identifica gaps)
- Tempo estimado: 1-2 minutos

**Executar quando:**
- Verificação rápida antes de commits importantes
- Manutenção mensal de regras
- Após adicionar nova tecnologia
- Validação após mudanças menores

---

### 2. FULL (Auditoria Completa)
**Uso:** `/audit-rules full`

**Ação:**
- Análise profunda de todas as regras e comandos
- Comparação detalhada com estado atual do projeto
- Leitura de código para identificar padrões emergentes
- Análise de consistência entre regras Claude vs Cursor
- Identificação de contradições e conflitos
- Proposta de consolidações e melhorias
- Sugestão de novas regras baseadas em padrões do código
- Tempo estimado: 5-10 minutos

**Executar quando:**
- Após milestone completado
- Antes de pivotagem importante
- Após decisões arquiteturais significativas
- Auditoria trimestral completa

---

### 3. OUTPUT-FILE
**Uso:** `/audit-rules full --output-file`

**Ação:**
- Salva relatório completo em `documents/technical/AUDIT_RULES_[DATA].md`
- Útil para revisão offline ou discussão em equipe
- Inclui todas as sugestões detalhadas e plano de ação
- Permite análise histórica de mudanças

**Executar quando:**
- Relatório precisa ser compartilhado
- Análise requer revisão detalhada
- Documentar evolução das regras ao longo do tempo

---

## 🔍 Escopo da Auditoria

### Arquivos Analisados

**Regras Claude:**
- `.claude/CLAUDE.md` (regras sempre ativas)
- `.claude/rules/*.md` (regras específicas)
- `.claude/commands/rules/*.md` (regras de processo)

**Regras Cursor:**
- `.cursor/rules/*.mdc` (todas as regras Cursor)

**Comandos:**
- `.claude/commands/*.md` (todos os comandos disponíveis)

### Fontes de Verdade (Comparação)

**Estado Atual do Projeto:**
- `documents/core/Roadmap.md` (milestone atual, DoR/DoD)
- `documents/core/TODO.md` (tarefas, progresso atual)
- `documents/core/Projeto.md` (decisões arquiteturais)
- `src/backend/` (código implementado)
- `requirements.txt` (dependências reais)
- `.env.example` (configurações necessárias)

---

## 🎯 Categorias de Análise

### A. Verificação de Relevância

**Para cada regra/comando, verificar:**

1. **Está sendo usado?**
   - Comando foi executado recentemente?
   - Regra é referenciada no código/docs?
   - Há evidências de aplicação prática?

2. **Está atualizado?**
   - Reflete decisões arquiteturais atuais?
   - Tecnologias mencionadas ainda são usadas?
   - Fluxos descritos correspondem ao implementado?

3. **É redundante?**
   - Duplica informação de outra regra?
   - Pode ser consolidada com outra?
   - Está em conflito com outra regra?

**Exemplo de análise:**
```markdown
### Regra: "NÃO usar Together AI no MVP"
- **Localização:** .claude/CLAUDE.md:linha 401
- **Status:** ✅ VÁLIDA - Confirmado em requirements.txt (ausente)
- **Uso:** Referenciada em 3 documentos
- **Recomendação:** Manter

### Regra: "Usar pgvector para RAG"
- **Localização:** .cursor/rules/architecture-guidelines.mdc:linha 156
- **Status:** ❌ OBSOLETA - Decisão mudou para long context Gemini
- **Conflito:** Contradiz .claude/CLAUDE.md:linha 395
- **Recomendação:** Remover ou atualizar para refletir decisão atual
```

### B. Análise de Gaps (Lacunas)

**Identificar regras/comandos que deveriam existir mas não existem:**

1. **Padrões de código recorrentes sem regra:**
   - Verificar `src/backend/services/` para padrões comuns
   - Ex: Se todos os services usam mesmo pattern de error handling
   - Sugerir: Criar regra documentando o pattern

2. **Tecnologias usadas sem guidance:**
   - Verificar `requirements.txt` vs regras existentes
   - Ex: Se Redis é usado mas não há regra sobre sessões
   - Sugerir: Criar regra sobre Redis session management

3. **Fluxos implementados sem documentação:**
   - Verificar routers vs regras de fluxo
   - Ex: Se endpoint existe mas fluxo não está documentado
   - Sugerir: Atualizar CLAUDE.md com fluxo

### C. Verificação de Referências no Roadmap/TODO

**NOVA CATEGORIA:** Verificar se Roadmap.md e TODO.md referenciam comandos/regras apropriadamente:

1. **Milestones sem referências a comandos:**
   - Verificar se cada milestone menciona `/rules/dor` e `/rules/dod`
   - Verificar se milestones com APIs mencionam `/rules/api-integration`
   - Verificar se há seção "Comandos Úteis" ou similar

2. **DoR/DoD sections sem comandos de validação:**
   - DoR deve mencionar `/rules/dor [milestone-id]`
   - DoD deve mencionar `/rules/dod [milestone-id]`, `/update-docs system`, `/validate-docs-links check`
   - Seções de testes devem mencionar `/rules/testing`
   - Seções de código devem mencionar `/rules/code-quality`

3. **Comandos não documentados no Roadmap:**
   - Verificar se comandos novos são mencionados em planejamento futuro
   - Verificar se há seção de "Comandos Úteis" atualizada
   - Sugerir adição se faltando

**Exemplo de análise:**
```markdown
### Milestone 1.3: Fluxo Completo
- **Status:** 🟡 Sem referências a comandos
- **Problema:** DoR e DoD não mencionam `/rules/dor 1.3` ou `/rules/dod 1.3`
- **Recomendação:** Adicionar seção "🔧 Comandos Aplicáveis" com referências apropriadas
- **Impacto:** Baixo - mas melhora discoverability de comandos
```

**Exemplo de gap:**
```markdown
### Gap Identificado: Session Management
- **Tipo:** Regra faltante
- **Evidência:** 
  - Redis usado em `src/backend/services/session_service.py`
  - `redis` em requirements.txt
  - Nenhuma regra sobre TTL, estrutura de dados, cleanup
- **Impacto:** Médio - Desenvolvedores podem implementar inconsistentemente
- **Sugestão:** Criar regra em `.claude/rules/session-management.md`
- **Conteúdo sugerido:**
  ```markdown
  # Session Management - Redis
  
  ## TTL Padrão
  - Sessões ativas: 30 minutos
  - Após confirmação: 5 minutos
  
  ## Estrutura de Dados
  - Key: `session:{user_id}`
  - Formato: JSON serializado
  - Campos obrigatórios: timestamp, state, data
  
  ## Cleanup
  - Automático via Redis TTL
  - Não armazenar PII sem encryption
  ```
```

### C. Análise de Consistência

**Verificar alinhamento entre diferentes documentos:**

1. **Claude vs Cursor:**
   - Regras conflitantes?
   - Padrões de código diferentes?
   - Nomenclaturas inconsistentes?

2. **Regras vs Código Implementado:**
   - Código segue as regras?
   - Regras refletem a implementação real?
   - Há desvios justificados?

3. **Comandos vs Workflow Real:**
   - Comandos são usados na prática?
   - Faltam comandos para tarefas comuns?
   - Comandos refletem processo atual?

**Exemplo de inconsistência:**
```markdown
### Inconsistência Detectada: Nomenclatura de Services

**Claude (.claude/CLAUDE.md:linha 250):**
- Diz: "Usar sufixo Service (ex: ASRService)"

**Cursor (.cursor/rules/code-quality-standards.mdc:linha 45):**
- Diz: "Classes de serviço devem terminar em Handler"

**Código Real (src/backend/services/):**
- `asr_service.py`: class ASRService ✅
- `llm_service.py`: class LLMService ✅
- Todos seguem padrão Claude

**Recomendação:**
- Atualizar regra Cursor para alinhar com Claude
- Pattern Service é consistente no codebase
```

### D. Análise de Decisões Arquiteturais

**Comparar regras com decisões documentadas:**

1. **Ler `documents/core/Projeto.md`:**
   - Extrair decisões técnicas principais
   - Comparar com regras arquiteturais
   - Verificar se mudanças foram refletidas

2. **Verificar stack tecnológica:**
   - `requirements.txt` vs regras
   - APIs mencionadas vs integradas
   - Serviços externos documentados

3. **Proibições arquiteturais:**
   - Lista de "NÃO FAZER" está atualizada?
   - Há tecnologias proibidas mas usadas?
   - Há tecnologias permitidas mas não usadas?

**Exemplo de desalinhamento:**
```markdown
### Decisão Arquitetural: Migração de Whisper para Gemini

**Roadmap.md (Milestone 1.2):**
- "Decisão: Usar Gemini Audio API ao invés de Whisper local"
- Data: 10/Out/2025
- Status: ✅ Completo

**Regras Atuais:**
- `.claude/CLAUDE.md`: ✅ "Usar Gemini Audio API (primário)"
- `.cursor/rules/architecture-guidelines.mdc`: ❌ Ainda menciona Whisper como opção

**Código Real:**
- `asr_service.py`: Usa Gemini Audio API ✅
- `requirements.txt`: Apenas google-generativeai ✅

**Recomendação:**
- Atualizar `.cursor/rules/architecture-guidelines.mdc`
- Remover menções a Whisper como alternativa válida para MVP
```

### E. Análise de Milestone e Fase

**Verificar se regras são apropriadas para fase atual:**

1. **Milestone atual vs Regras:**
   - Regras de fases futuras confundem?
   - Regras de fases passadas ainda relevantes?
   - DoR/DoD atual coberto por regras?

2. **Progressão do projeto:**
   - Se Fase 1 completa, remover regras temporárias?
   - Se Fase 2 começou, adicionar novas regras?
   - Atualizar prioridades conforme fase?

**Exemplo de análise temporal:**
```markdown
### Regras Temporais: Fase 1 vs Fase 2

**Status Atual (TODO.md):**
- Fase 1.3: 40% completo (em andamento)
- Fase 2: 0% (planejado)

**Regras de Fase 2 Prematuras:**

1. `.claude/rules/rag-optimization.md`
   - **Criado:** Milestone 2.1 (futuro)
   - **Status:** ⚠️ PREMATURO
   - **Problema:** Fase 2 não começou, pode confundir
   - **Recomendação:** Mover para `.claude/rules/future/` ou adicionar nota "Aplicar a partir de Fase 2"

**Regras de Fase 1 Temporárias:**

1. `.claude/rules/mvp-constraints.md`
   - **Criado:** Início do projeto
   - **Aplicável:** Apenas até final Fase 1
   - **Status:** ✅ ÚTIL AGORA
   - **Recomendação:** Marcar como "REMOVER após Fase 1 completa"
```

---

## 📊 Formato dos Relatórios

### Relatório FULL

```markdown
# Auditoria de Regras e Comandos - [Data]
**Milestone Atual:** Fase 1, M1.3 (40% completo)
**Versão do Projeto:** [extrair de algum lugar]
**Tempo de Análise:** 8 minutos

---

## 📊 Resumo Executivo

### Estatísticas
- **Regras Claude:** 12 analisadas
  - ✅ 9 válidas e atualizadas
  - ⚠️ 2 precisam atualização
  - ❌ 1 obsoleta
- **Regras Cursor:** 8 analisadas
  - ✅ 6 válidas e atualizadas
  - ⚠️ 1 precisa atualização
  - ❌ 1 conflitante com Claude
- **Comandos:** 3 analisados
  - ✅ 2 funcionais e úteis
  - ⚠️ 1 precisa melhoria
- **Gaps Identificados:** 4 áreas sem regras
- **Inconsistências:** 2 conflitos entre documentos

### Prioridades
1. 🔴 **URGENTE:** Resolver inconsistência de nomenclatura (Claude vs Cursor)
2. 🟡 **IMPORTANTE:** Criar regra para session management (gap crítico)
3. 🟢 **BAIXA:** Atualizar referências a tecnologias antigas

---

## 🔍 Análise Detalhada

### A. Regras Obsoletas ou Desatualizadas

#### 1. Regra: "Considerar Together AI para produção"
- **Localização:** `.claude/CLAUDE.md:linha 178`
- **Criada:** Set/2025
- **Status:** ❌ OBSOLETA
- **Razão:** 
  - Decisão mudou para Gemini-only (Projeto.md, 05/Out/2025)
  - requirements.txt não inclui Together AI
  - Contradiz regra principal "Gemini-First"
- **Impacto:** Médio - Pode causar confusão sobre stack tecnológica
- **Recomendação:** 
  ```markdown
  REMOVER ou atualizar para:
  "Together AI: Considerar apenas para escala comercial (Fase 4+)"
  ```

#### 2. Regra Cursor: "Usar Whisper para transcrição"
- **Localização:** `.cursor/rules/architecture-guidelines.mdc:linha 89`
- **Status:** ❌ OBSOLETA
- **Razão:** Migração completa para Gemini Audio (M1.2)
- **Código:** `asr_service.py` usa 100% Gemini
- **Recomendação:** 
  ```markdown
  REMOVER menção a Whisper
  ADICIONAR: "ASR: Usar exclusivamente Gemini Audio API"
  ```

---

### B. Gaps (Regras/Comandos Faltantes)

#### Gap 1: Session Management (Redis)
- **Tipo:** Regra faltante - Alto Impacto
- **Evidência:**
  - Redis usado extensivamente em `session_service.py`
  - Nenhuma regra sobre TTL, estrutura, cleanup
  - `SessionService` implementa padrões não documentados
- **Risco:** Inconsistência em futuras implementações
- **Sugestão:** Criar `.claude/rules/session-management.md`
- **Conteúdo proposto:**
  ```markdown
  # Session Management - Redis
  
  ## Padrões
  - TTL padrão: 30 minutos
  - Key format: `session:{user_id}`
  - Cleanup automático via Redis EXPIRE
  
  ## Segurança
  - Encrypt dados sensíveis com Fernet
  - Não armazenar tokens de API
  - Limpar sessão após ação completada
  
  ## Implementação
  Ver: `src/backend/services/session_service.py`
  ```

#### Gap 2: Comando para Verificar Dependências
- **Tipo:** Comando faltante - Médio Impacto
- **Necessidade:** 
  - requirements.txt muda frequentemente
  - Vulnerabilidades precisam ser checadas
  - Versões desatualizadas podem causar bugs
- **Sugestão:** Criar `/check-dependencies`
- **Funcionalidade:**
  ```markdown
  - Verificar versões desatualizadas (pip list --outdated)
  - Scan de vulnerabilidades (safety check)
  - Comparar requirements.txt vs imports reais
  - Sugerir atualizações seguras
  ```

---

### C. Inconsistências e Conflitos

#### Conflito 1: Nomenclatura de Classes de Serviço
- **Claude:** "Usar sufixo Service" (.claude/CLAUDE.md:250)
- **Cursor:** "Usar sufixo Handler" (.cursor/rules/code-quality-standards.mdc:45)
- **Código Real:** 100% usa Service (ASRService, LLMService, etc.)
- **Impacto:** Baixo - Código já seguiu um padrão
- **Recomendação:** 
  ```markdown
  Atualizar Cursor para alinhar com Claude:
  - Trocar "Handler" por "Service"
  - Justificar: Consistência com codebase existente
  ```

#### Conflito 2: Limites de Free Tier Gemini
- **Claude (.claude/CLAUDE.md):** "Manter < 180 RPD"
- **Cursor (.cursor/rules/cost-optimization.mdc):** "Limite 200 RPM"
- **Documentação Oficial Gemini:** 15 RPM, 1500 RPD (atualizado)
- **Impacto:** Alto - Limites incorretos podem causar erros
- **Recomendação:**
  ```markdown
  Atualizar AMBOS para valores corretos:
  - RPM: 15 (requests por minuto)
  - RPD: 1500 (requests por dia)
  - Fonte: https://ai.google.dev/pricing
  ```

---

### D. Regras Válidas mas com Melhorias Sugeridas

#### Regra: "Docs First" (CLAUDE.md:100)
- **Status:** ✅ VÁLIDA
- **Uso:** Alta frequência
- **Melhoria Sugerida:**
  ```markdown
  Adicionar métricas de sucesso:
  "Objetivo: 80% das tasks devem consultar docs ANTES de grep/glob"
  
  Adicionar exemplos negativos:
  "❌ ERRADO: grep -r 'ASRService' . (sem consultar docs)"
  "✅ CORRETO: Read .claude/docs/README.md → architecture.md"
  ```

---

### E. Comandos: Análise de Uso e Efetividade

#### Comando: /update-docs
- **Status:** ✅ FUNCIONAL
- **Frequência de Uso:** 12x nos últimos 30 dias
- **Efetividade:** Alta (docs estão atualizados)
- **Melhoria Sugerida:**
  ```markdown
  Adicionar modo "auto":
  /update-docs auto
  - Detecta automaticamente o que mudou
  - Atualiza apenas arquivos relevantes
  - Gera commit message sugerido
  ```

#### Comando: /validate-docs-links
- **Status:** ✅ NOVO (criado hoje)
- **Uso:** Ainda não testado em produção
- **Recomendação:** 
  ```markdown
  - Executar após 1 semana de uso
  - Coletar feedback sobre false positives
  - Ajustar KNOWN_MAPPINGS se necessário
  ```

---

## 🎯 Plano de Ação Recomendado

### Prioridade 1 - URGENTE (Fazer Hoje)
1. ✅ Corrigir limites Gemini (Claude + Cursor)
2. ✅ Resolver inconsistência Service vs Handler
3. ✅ Remover/atualizar regra Together AI obsoleta

### Prioridade 2 - IMPORTANTE (Fazer Esta Semana)
1. 📝 Criar regra session-management.md
2. 📝 Atualizar Cursor rules: remover Whisper
3. 📝 Criar comando /check-dependencies

### Prioridade 3 - MELHORIA (Fazer Este Mês)
1. 🔧 Melhorar regra "Docs First" com métricas
2. 🔧 Adicionar modo "auto" em /update-docs
3. 🔧 Revisar todas as regras de Fase 1 antes de iniciar Fase 2

### Prioridade 4 - MONITORAMENTO (Contínuo)
1. 📊 Re-executar /audit-rules a cada milestone completado
2. 📊 Verificar uso de comandos mensalmente
3. 📊 Atualizar KNOWN_MAPPINGS conforme estrutura evolui

---

## 📚 Arquivos Para Revisar/Criar

### Atualizar
- [ ] `.claude/CLAUDE.md` (3 mudanças)
- [ ] `.cursor/rules/architecture-guidelines.mdc` (2 mudanças)
- [ ] `.cursor/rules/cost-optimization.mdc` (1 mudança)
- [ ] `.cursor/rules/code-quality-standards.mdc` (1 mudança)

### Criar
- [ ] `.claude/rules/session-management.md` (novo)
- [ ] `.claude/commands/check-dependencies.md` (novo)

### Revisar (sem mudança imediata)
- [ ] `.claude/commands/update-docs.md` (considerar modo auto)
- [ ] `.claude/docs/README.md` (verificar se índice está completo)

---

## 🔄 Próxima Auditoria

**Recomendação:** Re-executar `/audit-rules full` quando:
- ✅ Milestone 1.3 for completado (próximo marco)
- ✅ Qualquer decisão arquitetural importante for tomada
- ✅ Nova tecnologia for adicionada ao projeto
- ✅ Após 30 dias (auditoria periódica)

**Data sugerida próxima auditoria:** [Data + 30 dias]

---

**Gerado em:** [Data e Hora]
**Comando:** `/audit-rules full`
**Versão:** 1.0.0
```

### Relatório QUICK

```markdown
# Auditoria Rápida de Regras - [Data]

## ⚡ Verificações Essenciais

### Regras Obviamente Obsoletas
- ❌ `.claude/CLAUDE.md:linha 178` - Menciona Together AI (decisão mudou)
- ❌ `.cursor/rules/architecture-guidelines.mdc:linha 89` - Menciona Whisper (removido)

### Gaps Críticos
- ⚠️ Session management (Redis) sem regra formal
- ⚠️ Rate limits Gemini desatualizados em 2 arquivos

### Inconsistências
- ⚠️ Nomenclatura: Service (Claude) vs Handler (Cursor)

## 🎯 Ação Imediata
Execute `/audit-rules full` para análise completa e recomendações detalhadas.

---
**Tempo:** 1m 23s
```

---

## 🔧 Algoritmo de Implementação

### Heurísticas de Detecção

#### Regra Obsoleta
```python
def is_obsolete(rule, project_state):
    """Detecta se regra está obsoleta."""
    
    # Menciona tecnologia removida?
    removed_techs = ["whisper", "together-ai", "twilio"]
    if any(tech in rule.content.lower() for tech in removed_techs):
        if tech not in project_state["dependencies"]:
            return True, f"Mentions {tech} but not in dependencies"
    
    # Contradiz decisão arquitetural?
    for decision in project_state["architecture_decisions"]:
        if contradicts(rule, decision):
            return True, f"Contradicts decision: {decision}"
    
    # Data de criação > 6 meses e não usada?
    if rule.age > 180 and not rule.referenced:
        return True, "Old and unused"
    
    return False, None
```

#### Gap de Regra
```python
def identify_rule_gaps(project_state, existing_rules):
    """Identifica padrões sem regras."""
    
    gaps = []
    
    # Padrão: Todos os services seguem estrutura similar
    services = glob("src/backend/services/*.py")
    patterns = extract_patterns(services)
    
    for pattern in patterns:
        if not has_rule_for_pattern(pattern, existing_rules):
            gaps.append({
                "type": "missing_rule",
                "pattern": pattern,
                "files": pattern.files,
                "suggested_rule": generate_rule_template(pattern)
            })
    
    # Tecnologia usada sem guidance
    for dep in project_state["dependencies"]:
        if not has_rule_for_tech(dep, existing_rules):
            gaps.append({
                "type": "missing_tech_rule",
                "tech": dep,
                "suggested_rule": f"Create rule for {dep} usage"
            })
    
    return gaps
```

#### Inconsistência
```python
def find_inconsistencies(claude_rules, cursor_rules):
    """Detecta conflitos entre Claude e Cursor."""
    
    conflicts = []
    
    # Tópicos comuns
    topics = extract_common_topics(claude_rules, cursor_rules)
    
    for topic in topics:
        claude_view = get_rule_for_topic(topic, claude_rules)
        cursor_view = get_rule_for_topic(topic, cursor_rules)
        
        if claude_view != cursor_view:
            conflicts.append({
                "topic": topic,
                "claude_says": claude_view,
                "cursor_says": cursor_view,
                "recommendation": resolve_conflict(claude_view, cursor_view)
            })
    
    return conflicts
```

---

## 🔄 Integração com Workflow

### Quando Executar

1. **Após milestone completado:**
   ```bash
   /audit-rules full
   # Revisar relatório
   # Aplicar mudanças recomendadas
   ```

2. **Mensalmente (manutenção):**
   ```bash
   /audit-rules quick
   # Se encontrar issues, rodar full
   ```

3. **Antes de pivotagem importante:**
   ```bash
   /audit-rules full --output-file
   # Usar relatório para discussão de mudanças
   ```

4. **Após adicionar nova tecnologia:**
   ```bash
   /audit-rules quick
   # Verificar se precisa de novas regras
   ```

### Pre-commit Checklist

```markdown
- [ ] Executar `/audit-rules quick` antes de commits importantes
- [ ] Corrigir regras obsoletas se encontradas
- [ ] Verificar inconsistências Claude vs Cursor
- [ ] Aplicar mudanças recomendadas se críticas
```

---

## 🎯 Regras de Execução

### SEMPRE:
1. **Coletar contexto atual** do projeto (milestone, dependências, código)
2. **Analisar TODAS as regras** Claude e Cursor
3. **Comparar com fontes de verdade** (Roadmap, TODO, código)
4. **Gerar relatório estruturado** com prioridades claras
5. **Propor soluções concretas** (não apenas identificar problemas)
6. **Calcular tempo de análise** para otimização futura
7. **Marcar confiança** das recomendações (Alta/Média/Baixa)
8. **Incluir plano de ação** com prioridades
9. **Sugerir próxima auditoria** baseada em marcos do projeto
10. **Preservar histórico** se modo --output-file

### NUNCA:
1. ❌ Modificar arquivos sem confirmação explícita
2. ❌ Ignorar conflitos entre Claude e Cursor
3. ❌ Propor regras sem justificativa baseada no código
4. ❌ Criar regras redundantes sem consolidação
5. ❌ Sugerir mudanças sem considerar impacto
6. ❌ Ignorar contexto de milestone atual
7. ❌ Propor regras para tecnologias não implementadas

---

## 📝 Exemplos de Uso

### Exemplo 1: Verificação Após Reorganização
```bash
/audit-rules quick
```
**Output esperado:** Relatório rápido com flags óbvios de desatualização.

### Exemplo 2: Auditoria Completa
```bash
/audit-rules full
```
**Output esperado:** Relatório detalhado com análise profunda e recomendações.

### Exemplo 3: Relatório para Discussão
```bash
/audit-rules full --output-file
```
**Output esperado:** Arquivo salvo em `documents/technical/AUDIT_RULES_[DATA].md` para revisão offline.

### Exemplo 4: Verificação Periódica
```bash
/audit-rules
```
**Output esperado:** Relatório rápido de status atual das regras.

---

## 🔧 Troubleshooting

### Problema: Muitos falsos positivos
**Causa:** Mapeamento desatualizado ou arquivos em locais não mapeados
**Solução:** Atualizar heurísticas de detecção ou adicionar exceções

### Problema: Regras válidas marcadas como obsoletas
**Causa:** Algoritmo muito restritivo ou contexto insuficiente
**Solução:** Refinar heurísticas e incluir mais contexto do projeto

### Problema: Gaps não identificados
**Causa:** Padrões no código não são óbvios ou não há evidências suficientes
**Solução:** Melhorar análise de padrões e incluir mais fontes de evidência

### Problema: Inconsistências não detectadas
**Causa:** Regras Claude e Cursor não têm tópicos sobrepostos óbvios
**Solução:** Melhorar algoritmo de extração de tópicos comuns

---

## 📚 Documentação Relacionada

- [CLAUDE.md](../CLAUDE.md) - Regras gerais do projeto
- [update-docs.md](update-docs.md) - Comando para atualizar documentação
- [validate-docs-links.md](validate-docs-links.md) - Comando para validar links
- [documents/core/Projeto.md](../../documents/core/Projeto.md) - Decisões arquiteturais
- [documents/core/Roadmap.md](../../documents/core/Roadmap.md) - Milestones e progresso

---

**Execute a auditoria de regras conforme o modo especificado em $ARGUMENTS**

Se nenhum argumento foi fornecido, execute no modo QUICK (verificação rápida de issues óbvios).
