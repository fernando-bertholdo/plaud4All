# Comando: /rules/explain-code

**Arquivo:** `.claude/commands/rules/explain-code.md`

## Descrição

Explica código implementado seguindo os princípios de desenvolvimento colaborativo, garantindo que decisões técnicas sejam claras e justificadas.

## Uso

```
/rules/explain-code
```

## Template de Explicação

### 1. Contexto e Objetivo
- **O que** o código faz
- **Por que** foi implementado
- **Quando** é usado

### 2. Decisões Técnicas
- **Por que** escolheu esta abordagem
- **Alternativas** consideradas
- **Trade-offs** envolvidos

### 3. Referências e Validação
- **Documentação oficial** consultada
- **APIs** validadas contra docs
- **Testes** executados (quando aplicável)

### 4. Suposições e Limitações
- **Suposições** feitas
- **Limitações** conhecidas
- **Melhorias** futuras possíveis

## Exemplo de Explicação

### Código a Explicar
```python
def transcribe_audio(audio_path: str) -> str:
    file = genai.upload_file(audio_path)
    result = model.generate_content([file, prompt])
    genai.delete_file(file.name)
    return result.text
```

### Explicação Seguindo Template

#### 1. Contexto e Objetivo
- **O que:** Transcreve áudio usando Gemini Audio API
- **Por que:** Necessário para processar áudios recebidos via WhatsApp
- **Quando:** Chamado pelo webhook quando usuário envia áudio

#### 2. Decisões Técnicas
- **Por que Files API:** Suporte a arquivos >10MB, melhor performance, cleanup automático
- **Alternativas consideradas:** Base64 (rejeitado - limitado a 10MB)
- **Trade-offs:** Files API é mais complexo mas necessário para arquivos grandes

#### 3. Referências e Validação
- **Documentação:** https://ai.google.dev/gemini-api/docs/audio#uploading-files
- **APIs validadas:** Gemini Files API conforme documentação oficial
- **Testes:** Executado com áudios de 1MB, 5MB e 15MB

#### 4. Suposições e Limitações
- **Suposições:** Arquivo existe e é válido (validação prévia)
- **Limitações:** Máximo 20MB por arquivo (limite Gemini)
- **Melhorias:** Implementar cache para arquivos repetidos

## Checklist de Explicação

### Contexto
- [ ] **Objetivo claro** - O que o código faz
- [ ] **Justificativa** - Por que foi implementado
- [ ] **Uso** - Quando é chamado/usado

### Decisões Técnicas
- [ ] **Abordagem explicada** - Por que escolheu esta solução
- [ ] **Alternativas mencionadas** - Outras opções consideradas
- [ ] **Trade-offs documentados** - Prós e contras da escolha

### Validação
- [ ] **Documentação referenciada** - Links para docs oficiais
- [ ] **APIs validadas** - Uso correto conforme documentação
- [ ] **Testes mencionados** - Validação com dados reais

### Transparência
- [ ] **Suposições explícitas** - O que foi assumido
- [ ] **Limitações conhecidas** - Restrições atuais
- [ ] **Melhorias futuras** - O que pode ser melhorado

## Exemplos por Tipo de Código

### Implementação de API
```python
# Explicar:
# 1. Por que escolheu este endpoint/verb
# 2. Como valida input/output
# 3. Como trata erros
# 4. Referências à documentação da API
```

### Processamento de Dados
```python
# Explicar:
# 1. Por que esta estrutura de dados
# 2. Como valida entrada
# 3. Como trata casos edge
# 4. Performance considerations
```

### Integração Externa
```python
# Explicar:
# 1. Por que esta biblioteca/API
# 2. Como configura autenticação
# 3. Como trata rate limits
# 4. Como faz cleanup
```

## Integração com Outras Regras

### Com `/rules/collaborative`
- Aplicar princípios antes de implementar
- Usar este comando para explicar após implementar

### Com `/rules/code-quality`
- Garantir que explicação segue padrões
- Validar que comentários são explicativos

### Com `/rules/dod`
- Incluir explicação como critério de DoD
- Validar que código está bem documentado

## Referências

- [Regra Completa](../rules/collaborative-development.md)
- [Exemplos Práticos](../docs/system/collaborative-examples.md)
- [Padrões de Qualidade](../rules/code-quality.md)

---

**Lembre-se:** Uma boa explicação permite que qualquer desenvolvedor entenda não apenas o que o código faz, mas por que foi implementado desta forma.
