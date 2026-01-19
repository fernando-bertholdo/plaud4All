---
description: Checklist de atualização de documentação antes de commits
argument-hint:
---

# Análise Pré-Commit

Antes de realizar QUALQUER commit, execute esta análise completa:

## 1. Análise do Trabalho Realizado

Responda estas perguntas:

### Arquivos Modificados
- ✅ Listar todos os arquivos modificados/criados
- ✅ Identificar tipo de mudança (código, config, docs, testes)

### Funcionalidade Impactada
- ✅ Qual funcionalidade foi implementada/alterada?
- ✅ Qual fase/milestone do Roadmap.md foi impactada?
- ✅ Algum DoD foi completado?

### Componentes Afetados
- ✅ Há novos endpoints, services ou módulos?
- ✅ Há mudanças arquiteturais relevantes?
- ✅ Novas dependências foram adicionadas?

## 2. Atualização do Projeto.md

Verifique se [documents/Projeto.md](../../documents/Projeto.md) precisa de atualização nas seguintes seções:

### Se adicionou código:
- [ ] Estrutura do Projeto (novos arquivos/diretórios)
- [ ] Serviços (se criou/modificou services)
- [ ] Fluxos (se alterou lógica de fluxo)
- [ ] Exemplos de código (se há novos patterns)

### Se mudou configuração:
- [ ] Variáveis de Ambiente (novas env vars)
- [ ] Setup Rápido (novos passos)
- [ ] Dependências (requirements.txt atualizado)

### Se completou milestone:
- [ ] Roadmap (atualizar status)
- [ ] Troubleshooting (adicionar novos casos)
- [ ] Changelog (documentar mudanças)

### Se adicionou features:
- [ ] Visão Geral da Arquitetura
- [ ] Componentes Mínimos/Opcionais
- [ ] Métricas de Sucesso

## 3. Atualização do TODO.md

Verifique [documents/TODO.md](../../documents/TODO.md):
- [ ] Marcar tarefas completadas
- [ ] Atualizar progresso (%)
- [ ] Adicionar novas tarefas descobertas
- [ ] Atualizar dependências

## 4. Validação de Segurança

Checklist de segurança obrigatório:
- [ ] Nenhum secret hardcoded no código
- [ ] Nenhum dado sensível sendo commitado
- [ ] .env não está sendo commitado
- [ ] API keys usando environment variables
- [ ] Logging não expõe dados sensíveis

## 5. Validação de Qualidade

- [ ] Type hints em todas funções novas
- [ ] Docstrings completas
- [ ] Testes escritos (se aplicável)
- [ ] Linting sem erros
- [ ] Conventional commits em português

## 6. Mensagem de Commit

Gerar mensagem seguindo Conventional Commits:

**Formato:**
```
tipo: descrição breve

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

**Tipos permitidos:**
- `feat:` Nova funcionalidade
- `fix:` Correção de bug
- `docs:` Documentação
- `refactor:` Refatoração
- `test:` Testes
- `chore:` Manutenção

## Exceções

Você pode PULAR esta análise APENAS se:
- Commit é exclusivamente de documentação (não código)
- Commit é fix de typo ou formatação
- Mudanças são triviais (comentários, logs)

**Para todos os outros commits: ANÁLISE OBRIGATÓRIA**

---

Execute esta análise agora e apresente os resultados antes de prosseguir com o commit.
