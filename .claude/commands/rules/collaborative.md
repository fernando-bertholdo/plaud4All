# Comando: /rules/collaborative

**Arquivo:** `.claude/commands/rules/collaborative.md`

## Descrição

Aplica os princípios de desenvolvimento colaborativo inspirados no comentário de Karpathy, adaptados para o projeto Annotr AI.

## Uso

```
/rules/collaborative
```

## Checklist de Validação

### Antes de Implementar
- [ ] **Funcionalidade identificada** e quebrada em chunks ≤100 linhas
- [ ] **Documentação oficial** das APIs pesquisada
- [ ] **Trade-offs identificados** e alternativas consideradas
- [ ] **Suposições explícitas** documentadas
- [ ] **Incertezas** identificadas para discussão

### Durante a Implementação
- [ ] **Chunks pequenos** (≤100 linhas, justificar se maior)
- [ ] **Comentários explicativos** (porquê, não apenas o quê)
- [ ] **Referências específicas** a documentação oficial
- [ ] **Trade-offs documentados** (alternativas consideradas)
- [ ] **APIs validadas** contra documentação oficial

### Após Implementar
- [ ] **Código revisado** para garantir explicações claras
- [ ] **Testes executados** com dados reais (quando aplicável)
- [ ] **Aprendizados documentados** para referência futura
- [ ] **Princípios validados** (todos os checkboxes acima)

## Exemplos de Aplicação

### ✅ Implementação Correta
```python
# Gemini Files API - https://ai.google.dev/gemini-api/docs/audio
# Decisão técnica: Usar Files API ao invés de base64 porque:
# 1. Suporte a arquivos maiores (>10MB)
# 2. Melhor performance para áudios longos
# 3. Cleanup automático pelo Gemini
# Ref: "Uploading files" section in Gemini Audio API docs
file = genai.upload_file(audio_path)
```

### ❌ Implementação Incorreta
```python
# Upload do arquivo
file = genai.upload_file(audio_path)
```

## APIs Principais do Annotr

### Gemini API
- **Documentação:** https://ai.google.dev/gemini-api/docs/audio
- **Rate Limits:** 10 req/min (free tier), 180 req/dia
- **Best Practices:** Files API para áudios > 10MB, cleanup obrigatório

### Notion API
- **Documentação:** https://developers.notion.com/reference/intro
- **Rate Limits:** 3 req/s (crítico)
- **Best Practices:** Retry com exponential backoff, validação de schema

### Evolution API
- **Documentação:** https://doc.evolution-api.com/v2/pt/get-started/introduction
- **Webhook Events:** messages_upsert, messages_update, connection_update
- **Best Practices:** Validação de payload, whitelist de números

## Anti-Patterns a Evitar

1. **Código sem explicação** - Apenas descreve o que faz
2. **Suposições não documentadas** - Assume comportamento sem validar
3. **Chunks muito grandes** - >100 linhas sem justificativa
4. **Referências genéricas** - Links vagos sem seção específica
5. **Trade-offs não discutidos** - Não menciona alternativas consideradas

## Integração com Workflow

### DoR (Definition of Ready)
- Decisões técnicas pesquisadas e documentadas
- APIs validadas contra documentação oficial
- Chunks de implementação planejados (≤100 linhas)

### DoD (Definition of Done)
- Implementação em chunks ≤100 linhas (justificar se maior)
- Decisões técnicas documentadas com referências
- Comentários explicativos obrigatórios
- APIs validadas contra documentação oficial
- Trade-offs discutidos e documentados

## Referências

- [Regra Completa](../rules/collaborative-development.md)
- [Exemplos Práticos](../docs/system/collaborative-examples.md)
- [Comentário Original do Karpathy](https://x.com/karpathy/status/...)

---

**Regra de Ouro:** "Se não posso explicar o porquê de uma decisão técnica em 2-3 frases, preciso pesquisar mais ou perguntar antes de implementar."
