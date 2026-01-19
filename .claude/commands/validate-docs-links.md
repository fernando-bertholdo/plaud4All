---
description: Valida e corrige links de documentação no repositório
argument-hint: [check|fix] [--dry-run]
---

# Validação de Links de Documentação

Você deve validar e corrigir links de documentação no repositório conforme o modo especificado: **$ARGUMENTS**

---

## 📋 Modos de Operação

### 1. CHECK (Padrão)
**Uso:** `/validate-docs-links` ou `/validate-docs-links check`

**Ação:**
- Escanear todos os arquivos .md no repositório
- Identificar links potencialmente quebrados ou desatualizados
- Gerar relatório detalhado sem modificar arquivos
- Listar sugestões de correção para cada problema encontrado

**Executar quando:**
- Após reorganização de pastas de documentação
- Antes de commits importantes
- Periodicamente para auditoria de integridade
- Após merge de branches grandes

---

### 2. FIX
**Uso:** `/validate-docs-links fix`

**Ação:**
- Escanear todos os arquivos .md no repositório
- Identificar links quebrados ou desatualizados
- Aplicar correções automaticamente quando possível
- Gerar relatório de mudanças aplicadas
- Marcar casos que precisam revisão manual

**Executar quando:**
- Após confirmar que as correções sugeridas estão corretas
- Quando você quer aplicar todas as correções de uma vez
- Após reorganização confirmada da estrutura

---

### 3. DRY-RUN
**Uso:** `/validate-docs-links fix --dry-run`

**Ação:**
- Mostrar exatamente o que seria alterado sem aplicar mudanças
- Útil para revisão antes de aplicar correções
- Gerar preview das mudanças que seriam feitas

**Executar quando:**
- Quer revisar mudanças antes de aplicar
- Precisa mostrar mudanças para aprovação
- Debugging de problemas de links

---

## 🔍 Algoritmo de Validação

### Estrutura Atual do Repositório

```
documents/
├── core/              # Projeto.md, Roadmap.md, TODO.md
├── technical/         # DATASET_GENERATION.md, COST_OPTIMIZATION.md, etc.
├── strategy/          # COMMERCIAL_ECOSYSTEM_STRATEGY.md, etc.
├── product/           # UX_STRATEGY.md, ANALYTICS_EXPANSION_GUIDE.md, etc.
└── archive/          # Documentos legados
```

### Mapeamento de Arquivos Conhecidos

```python
KNOWN_MAPPINGS = {
    # Technical Documents
    "DATASET_GENERATION.md": "documents/technical/",
    "COST_OPTIMIZATION.md": "documents/technical/",
    "NLP_ALTERNATIVES.md": "documents/technical/",
    "INCREMENTAL_TRAINING.md": "documents/technical/",
    "INFRASTRUCTURE.md": "documents/technical/",
    
    # Core Documents
    "Projeto.md": "documents/core/",
    "Roadmap.md": "documents/core/",
    "TODO.md": "documents/core/",
    
    # Product Documents
    "UX_STRATEGY.md": "documents/product/",
    "ANALYTICS_EXPANSION_GUIDE.md": "documents/product/",
    "AUDIENCE_INSIGHTS_FRAMEWORK.md": "documents/product/",
    "PRODUCT_ANALYTICS_FOUNDATION.md": "documents/product/",
    
    # Strategy Documents
    "COMMERCIAL_ECOSYSTEM_STRATEGY.md": "documents/strategy/",
    "OPEN_SOURCE_STRATEGY.md": "documents/strategy/",
    "ANALISE_COESAO_ESTRATEGICA.md": "documents/strategy/",
}
```

### Padrões a Detectar

1. **Links com padrão antigo:**
   - `](documents/[A-Z_]+\.md)` → deve ser `](documents/{core|technical|strategy|product}/`
   - `](../documents/[A-Z_]+\.md)` → deve ser `](../documents/{core|technical|strategy|product}/`

2. **Links relativos inconsistentes:**
   - Após mudança de local do arquivo fonte
   - Caminhos `../` calculados incorretamente

3. **Links para arquivos inexistentes:**
   - Arquivos que foram movidos ou deletados
   - Arquivos que nunca existiram

### Processo de Validação

**Para cada arquivo .md encontrado:**

1. **Ler conteúdo completo**
2. **Encontrar todos os links markdown** usando regex: `\[([^\]]+)\]\(([^)]+)\)`
3. **Para cada link encontrado:**
   - Extrair caminho do arquivo de destino
   - Determinar se é link relativo ou absoluto
   - Verificar se arquivo existe no caminho atual
   - Se não existir:
     - Buscar arquivo na nova estrutura usando KNOWN_MAPPINGS
     - Calcular caminho relativo correto baseado na localização do arquivo fonte
     - Sugerir/aplicar correção
   - Se existir:
     - Validar que caminho relativo está correto
     - Corrigir se necessário

---

## 📊 Formato dos Relatórios

### Relatório CHECK

```markdown
# Relatório de Validação de Links - [Data Atual]

## 📈 Resumo
- ✅ Links válidos: X
- ⚠️ Links suspeitos: Y  
- ❌ Links quebrados: Z
- 📁 Arquivos analisados: N
- 🔍 Total de links verificados: T

## 🔍 Detalhes

### ❌ Links Quebrados
1. **Arquivo:** `data/README.md` (linha 183)
   - **Link atual:** `../documents/DATASET_GENERATION.md`
   - **Status:** ❌ Arquivo não encontrado no caminho especificado
   - **Sugestão:** `../documents/technical/DATASET_GENERATION.md`
   - **Confiança:** Alta (arquivo existe na nova localização)

2. **Arquivo:** `README.md` (linha 205)
   - **Link atual:** `documents/Projeto.md`
   - **Status:** ❌ Arquivo não encontrado no caminho especificado
   - **Sugestão:** `documents/core/Projeto.md`
   - **Confiança:** Alta (arquivo existe na nova localização)

### ⚠️ Links Suspeitos
1. **Arquivo:** `QUICKSTART.md` (linha 158)
   - **Link atual:** `documents/TODO.md`
   - **Status:** ⚠️ Padrão antigo detectado (arquivo existe mas pode estar desatualizado)
   - **Sugestão:** `documents/core/TODO.md`
   - **Confiança:** Média (arquivo existe mas estrutura mudou)

### 🔗 Links para Arquivos Inexistentes
1. **Arquivo:** `README.md` (linha 206)
   - **Link atual:** `gemini-stack-migration.plan.md`
   - **Status:** ❌ Arquivo não existe em lugar nenhum
   - **Sugestão:** Remover link ou criar arquivo
   - **Confiança:** Alta (arquivo não encontrado em todo o repositório)

## 🎯 Ações Recomendadas
- Execute `/validate-docs-links fix` para aplicar correções automáticas
- Revise manualmente os casos marcados como "Confiança: Média"
- Para arquivos inexistentes, decida se deve remover o link ou criar o arquivo
- Execute `/validate-docs-links fix --dry-run` para preview das mudanças
```

### Relatório FIX

```markdown
# Correções Aplicadas - [Data Atual]

## 📈 Resumo
- ✅ Links corrigidos automaticamente: X
- ⚠️ Links que precisam revisão manual: Y
- 📁 Arquivos modificados: N
- ❌ Links não corrigidos (arquivos inexistentes): Z

## 📝 Arquivos Modificados

### data/README.md
- **Linha 183:** `../documents/DATASET_GENERATION.md` → `../documents/technical/DATASET_GENERATION.md`
- **Linha 184:** `../documents/COST_OPTIMIZATION.md` → `../documents/technical/COST_OPTIMIZATION.md`
- **Linha 185:** `../documents/NLP_ALTERNATIVES.md` → `../documents/technical/NLP_ALTERNATIVES.md`

### README.md
- **Linha 180:** `documents/Projeto.md` → `documents/core/Projeto.md`
- **Linha 205:** `documents/Projeto.md` → `documents/core/Projeto.md`
- **Linha 206:** Removido link para arquivo inexistente `gemini-stack-migration.plan.md`

### QUICKSTART.md
- **Linha 158:** `documents/TODO.md` → `documents/core/TODO.md`
- **Linha 203:** `documents/Roadmap.md` → `documents/core/Roadmap.md`
- **Linha 204:** `documents/TODO.md` → `documents/core/TODO.md`

## ⚠️ Revisão Manual Necessária
(Se houver casos ambíguos que o comando não pode resolver automaticamente)

Nenhum caso encontrado nesta execução.

## 🔗 Links para Arquivos Inexistentes
1. **README.md (linha 206):** `gemini-stack-migration.plan.md`
   - **Ação tomada:** Link removido
   - **Justificativa:** Arquivo não existe no repositório
```

---

## 🛠️ Casos Especiais

### Links Ambíguos
**Quando:** Múltiplos arquivos com mesmo nome existem em diferentes pastas
**Ação:** Reportar e pedir confirmação manual
**Exemplo:** Se existir `documents/core/README.md` e `documents/technical/README.md`

### Caminhos Relativos Complexos
**Quando:** Arquivos em subpastas com múltiplos `../`
**Ação:** Calcular corretamente baseado na localização do arquivo fonte
**Exemplo:** `documents/archive/milestones/ORDEM_DE_EXECUCAO.md` → `../../core/TODO.md`

### Links para Arquivos Externos
**Quando:** Links para URLs externas ou outros repositórios
**Ação:** Validar se URL está acessível (opcional, pode ser desabilitado)
**Exemplo:** `https://github.com/user/repo/blob/main/README.md`

### Links para Seções Internas
**Quando:** Links para seções dentro do mesmo arquivo (`#seção`)
**Ação:** Verificar se a seção existe no arquivo
**Exemplo:** `[Sumário](#sumário)`

---

## 🔄 Integração com Workflow

### Quando Usar

1. **Após reorganização de pastas:**
   ```bash
   /validate-docs-links check
   # Revisar relatório
   /validate-docs-links fix
   ```

2. **Antes de commits importantes:**
   ```bash
   /validate-docs-links check
   # Corrigir problemas encontrados
   git add .
   git commit -m "fix: corrige links de documentação"
   ```

3. **Periodicamente (mensal/trimestral):**
   ```bash
   /validate-docs-links check
   # Manter documentação sempre atualizada
   ```

4. **Após merge de branches grandes:**
   ```bash
   /validate-docs-links check
   # Verificar se merge não quebrou links
   ```

### Pre-commit Checklist

```markdown
- [ ] Executar `/validate-docs-links check`
- [ ] Corrigir links quebrados se encontrados
- [ ] Verificar relatório antes de commit
- [ ] Aplicar correções com `/validate-docs-links fix` se necessário
```

---

## 🎯 Regras de Execução

### SEMPRE:
1. **Escanear TODOS os arquivos .md** no repositório
2. **Preservar conteúdo original** quando possível
3. **Gerar relatório detalhado** com justificativas
4. **Usar mapeamento conhecido** para correções automáticas
5. **Calcular caminhos relativos** corretamente
6. **Marcar confiança** das correções (Alta/Média/Baixa)
7. **Preservar formatação** dos arquivos
8. **Atualizar timestamp** nos relatórios
9. **Ser específico** nas sugestões
10. **Documentar decisões** tomadas

### NUNCA:
1. ❌ Modificar arquivos sem confirmação (exceto modo FIX)
2. ❌ Quebrar links funcionais
3. ❌ Criar links para arquivos inexistentes
4. ❌ Ignorar casos ambíguos
5. ❌ Aplicar correções sem justificativa
6. ❌ Modificar conteúdo não relacionado a links
7. ❌ Deletar links sem confirmação explícita

---

## 📝 Exemplos de Uso

### Exemplo 1: Verificação Após Reorganização
```bash
/validate-docs-links check
```
**Output esperado:** Relatório detalhado com todos os links quebrados e sugestões de correção.

### Exemplo 2: Aplicar Correções
```bash
/validate-docs-links fix
```
**Output esperado:** Relatório de correções aplicadas com lista de arquivos modificados.

### Exemplo 3: Preview de Mudanças
```bash
/validate-docs-links fix --dry-run
```
**Output esperado:** Mostra exatamente o que seria alterado sem aplicar as mudanças.

### Exemplo 4: Verificação Periódica
```bash
/validate-docs-links
```
**Output esperado:** Relatório de status atual de todos os links no repositório.

---

## 🔧 Troubleshooting

### Problema: Muitos falsos positivos
**Causa:** Mapeamento desatualizado ou arquivos em locais não mapeados
**Solução:** Atualizar KNOWN_MAPPINGS ou adicionar exceções

### Problema: Caminhos relativos incorretos
**Causa:** Cálculo incorreto baseado na localização do arquivo fonte
**Solução:** Verificar algoritmo de cálculo de caminhos relativos

### Problema: Links para arquivos externos sendo reportados
**Causa:** Regex capturando URLs como links internos
**Solução:** Melhorar regex para distinguir URLs de caminhos de arquivos

### Problema: Arquivos não encontrados
**Causa:** Arquivos foram movidos ou deletados
**Solução:** Atualizar estrutura de pastas ou remover links órfãos

---

## 📚 Documentação Relacionada

- [CLAUDE.md](../CLAUDE.md) - Regras gerais do projeto
- [update-docs.md](update-docs.md) - Comando para atualizar documentação
- [documents/core/Projeto.md](../../documents/core/Projeto.md) - Documentação principal do projeto
- [documents/core/Roadmap.md](../../documents/core/Roadmap.md) - Roadmap de desenvolvimento

---

**Execute a validação de links conforme o modo especificado em $ARGUMENTS**

Se nenhum argumento foi fornecido, execute no modo CHECK (apenas verificação e relatório).
