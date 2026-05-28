# Contribuindo para o projeto 🎉

Boa vinda ao time! Este guia cobre tudo que você precisa pra contribuir com
confiança. Se tiver dúvida, abre uma issue ou chama alguém no canal do time.

---

## TL;DR — Fluxo em 5 passos

```text
1. Clone o repo e rode npm install
2. Crie uma branch: feature/TKT-123-minha-feature
3. Escreva código + testes
4. Commit: feat(escopo): descrição no imperativo
5. Abra PR pequeno, focado, com contexto claro
```

Simples assim. O resto deste guia detalha cada passo.

---

## 🚀 Getting Started

### Pré-requisitos

| Ferramenta | Versão mínima |
|------------|---------------|
| Node.js    | 20.x |
| npm        | 10.x |
| pnpm       | 9.x (opcional) |
| Git        | qualquer versão recente |

### Clone e setup

```bash
# Clone o repositório
git clone https://github.com/CaramaschiLevva/eden-agents.git
cd eden-agents

# Instale as dependências (npm)
npm install

# Ou com pnpm
pnpm install

# Copie o arquivo de variáveis de ambiente
cp .env.example .env
# Edite .env com os valores do seu ambiente local (nunca commite esse arquivo)
```

### Rodar localmente

```bash
# Desenvolvimento com hot reload
npm run dev

# Build de produção
npm run build

# Rodar testes
npm test

# Lint
npm run lint

# Type check
npm run typecheck
```

Se algo não subir, vai direto para a seção [Troubleshooting](#-troubleshooting-de-contribuição).

---

## 🌿 Branch Naming

### Convenção

```text
[prefixo]/[TICKET]-[descricao-curta-em-kebab-case]
```

### Prefixos disponíveis

| Prefixo | Quando usar |
|---------|-------------|
| `feature/` | Nova funcionalidade |
| `fix/` | Correção de bug |
| `hotfix/` | Correção urgente em produção |
| `chore/` | Tarefas de manutenção, deps, config |
| `refactor/` | Refatoração sem mudança de comportamento |
| `docs/` | Documentação apenas |

### Exemplos

```bash
feature/TKT-123-adicionar-elegibilidade
fix/TKT-456-corrigir-saldo-negativo
hotfix/TKT-789-token-expirado-em-producao
chore/TKT-321-atualizar-typescript
refactor/TKT-654-extrair-validacao-token
docs/TKT-987-atualizar-contributing
```

Branch principal: `main`

Nunca commite direto na branch principal. Sempre via PR.

---

## 💬 Conventional Commits

### Formato

```text
tipo(escopo): descrição no imperativo
```

### Tipos

| Tipo | Quando usar |
|------|-------------|
| `feat` | Nova feature |
| `fix` | Correção de bug |
| `chore` | Manutenção, deps, build |
| `refactor` | Refatoração |
| `docs` | Documentação |
| `test` | Testes |
| `perf` | Melhoria de performance |
| `ci` | Mudanças no CI/CD |

### Exemplos do domínio

```text
feat(saldo): adicionar endpoint de consulta de saldo por beneficiário
fix(elegibilidade): corrigir validação de período de graça
chore(deps): atualizar typescript para 5.4
refactor(auth): extrair validação de token para middleware
docs(contributing): adicionar seção de troubleshooting
test(saldo): adicionar testes para edge case de saldo negativo
perf(consulta): adicionar cache em consulta de beneficiários
ci(actions): adicionar step de type check no pipeline
```

### Regras

- Imperativo: "adicionar" não "adicionado" ou "adicionando"
- Máximo 72 caracteres na primeira linha
- Sem ponto final
- Escopo em minúsculo
- Se a mudança quebra compatibilidade, adicione `!` após o tipo:
  `feat(auth)!: remover suporte a token legado`

---

## 📝 Abrindo um PR

### Template de PR

Quando abrir um PR, preencha este template:

```markdown
## O que mudou
<!-- Descreva brevemente o que foi alterado -->

## Por que mudou
<!-- Qual problema resolve ou qual feature adiciona -->

## Como testar
<!-- Passos para verificar que funciona -->

## Checklist
- [ ] Testes passando localmente (`npm test`)
- [ ] Lint sem erros (`npm run lint`)
- [ ] Sem secrets ou dados pessoais no diff
- [ ] PR pequeno e focado (< 400 linhas se possível)
- [ ] Documentação atualizada (se necessário)
- [ ] Reviewers adicionados
```

### Antes de abrir o PR

Faça isso antes de pedir review:

- [ ] CI local passou (`npm test && npm run lint && npm run typecheck`)
- [ ] Branch atualizada com `main`
  (`git pull --rebase origin main`)
- [ ] Commits limpos e com mensagens descritivas
- [ ] Diff revisado por você mesmo (leia o próprio PR antes de pedir review)
- [ ] Nenhum `console.log` de debug esquecido
- [ ] Nenhum arquivo `.env` ou secret no diff

### Quem revisar

Adicione @CaramaschiLevva como reviewer. Para mudanças em auth,
pagamentos ou integrações externas, o review de segurança é obrigatório.

---

## 🔍 Code Review

### O que o autor deve fazer

Antes de pedir review, o autor é responsável por:

1. Revisar o próprio diff no GitHub (você vai pegar coisas que esqueceu)
2. Deixar comentários explicativos em trechos não óbvios
3. Responder todos os comentários antes de pedir re-review
4. Não fechar comentários sem resolver ou explicar por que não vai resolver

### O que o reviewer deve verificar

- Lógica de negócio correta para o domínio (saldo, elegibilidade, beneficiários)
- Testes cobrem os casos relevantes, incluindo edge cases
- Sem vazamento de dados sensíveis (PII, saldo, CPF)
- Tratamento de erros adequado
- Performance: sem N+1, sem chamadas desnecessárias
- Segurança: validação de input, autenticação, autorização

### Como responder a comentários

- Comentário resolvido: responda o que foi feito e marque como resolvido
- Discordância: argumente com dados ou contexto, não com opinião
- Dúvida: pergunte antes de assumir
- Prefixe sugestões opcionais com `nit:` para deixar claro que não é bloqueante

### Critério de merge

CI verde + 1 aprovação(ões) de @CaramaschiLevva.

Não faça merge sem CI verde. Nunca.

---

## 🔒 Segurança nas Contribuições

Este projeto lida com dados financeiros e pessoais. Segurança não é opcional.

> **Important**
> 🚨 Este projeto opera em domínio financeiro regulado. Qualquer violação de
> segurança pode ter impacto legal e regulatório. Quando em dúvida, escale para
> revisão humana.

### O que nunca commitar

- Arquivos `.env` com valores reais
- Tokens, API keys, senhas, certificados
- CPF, nome completo, saldo ou qualquer dado de beneficiário real
- Credenciais de banco de dados ou serviços externos

### LGPD e PII

Dados de beneficiários são protegidos por lei. Nas fixtures e testes:

- Use dados fictícios gerados (ex: CPF `000.000.000-00`, nome
  `Beneficiário Teste`)
- Nunca copie dados de produção para testes
- Se precisar de dados realistas, use uma biblioteca de geração de dados fake

### Domínios sensíveis

Mudanças nos seguintes módulos exigem review de segurança obrigatório:

- Autenticação e autorização
- Processamento de pagamentos e saldo
- Integrações com sistemas externos
- Qualquer coisa que toque em dados de beneficiários

Não reduza requisitos de segurança pra "resolver rápido". Se o prazo está
apertado, escala o problema.

### Encontrou uma credencial hardcoded?

1. Não commite o arquivo com a credencial
2. Abre uma issue privada descrevendo onde está
3. Avisa o time de segurança imediatamente
4. A credencial precisa ser rotacionada antes de qualquer merge

---

## 🐛 Troubleshooting de Contribuição

### CI falhou mas localmente passa

**Sintoma:** Pipeline vermelho, mas `npm test` local está verde.

**Causa:** Diferença de versão do Node, variável de ambiente faltando no CI,
ou teste com dependência de ordem.

**Fix:** Verifique a versão do Node no CI (`.github/workflows/`)
e compare com a local. Rode `npm ci` (não `npm install`) pra simular o
ambiente do CI.

---

### Merge conflict

**Sintoma:** GitHub mostra "This branch has conflicts that must be resolved".

**Causa:** Alguém alterou os mesmos arquivos na branch principal enquanto você
trabalhava.

**Fix:**

```bash
git fetch origin
git rebase origin/main
# Resolva os conflitos nos arquivos marcados
git add .
git rebase --continue
git push --force-with-lease
```

---

### Lint error no CI

**Sintoma:** Step de lint falha com erros de formatação ou regras ESLint.

**Causa:** Código não formatado ou violação de regra.

**Fix:**

```bash
npm run lint -- --fix
# Verifique o que não foi corrigido automaticamente
npm run lint
```

---

### Testes quebrando após rebase

**Sintoma:** Testes que passavam param de passar depois do rebase.

**Causa:** Mudança na branch principal alterou comportamento que seus testes
dependem.

**Fix:** Leia o diff do rebase, entenda o que mudou e atualize seus testes
para refletir o novo comportamento esperado.

---

### PR não mergeia mesmo com aprovação

**Sintoma:** Botão de merge desabilitado mesmo com aprovações suficientes.

**Causa:** CI ainda rodando, branch desatualizada, ou branch protection rule
não satisfeita.

**Fix:** Aguarde o CI terminar. Se a branch estiver desatualizada, clique em
"Update branch" ou faça rebase manual. Verifique se todos os checks obrigatórios
estão verdes.

---

## 📚 Mais Recursos

- [README do projeto](./README.md) — visão geral, setup e arquitetura
- [AGENTS.md](./AGENTS.md) — contexto para agentes de AI e automações
- [Documentação de arquitetura](./docs/architecture.md) —
  decisões técnicas e diagramas
- [Conventional Commits](https://www.conventionalcommits.org/pt-br/) —
  especificação completa
- [CI/CD](./.github/workflows/) — pipelines e configurações

---

Dúvidas? Abre uma issue ou chama no canal do time. Boas contribuições! ✅
