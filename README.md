# ticket-benefits-api 🎫

> API de gestão de benefícios alimentação e refeição para empresas brasileiras — produto regulado da Edenred Ticket Brasil

[![CI](https://img.shields.io/github/actions/workflow/status/CaramaschiLevva/ticket-benefits-api/ci.yml?branch=main&label=CI&style=flat-square)](https://github.com/CaramaschiLevva/ticket-benefits-api/actions)
[![Versão](https://img.shields.io/github/package-json/v/CaramaschiLevva/ticket-benefits-api?style=flat-square)](./package.json)
[![Node](https://img.shields.io/badge/node-%3E%3D20-brightgreen?style=flat-square&logo=node.js)](https://nodejs.org)
[![Licença](https://img.shields.io/badge/licen%C3%A7a-UNLICENSED-blue?style=flat-square)](./LICENSE)

---

## TL;DR

- **O que é:** Serviço NestJS/TypeScript que gerencia benefícios, saldos e elegibilidade de cartões Ticket Restaurante e Alimentação
- **Stack:** TypeScript · Node.js · NestJS
- **Domínio:** Benefícios financeiros regulados — dados sensíveis, LGPD em vigor
- **Pra rodar:** clone → instala deps → copia `.env.example` → `npm run dev`
- **Antes de codar:** lê o [`AGENTS.md`](./AGENTS.md) (contexto pra AI) e o
  [`CONTRIBUTING.md`](./CONTRIBUTING.md) (fluxo de trabalho)

---

## ✨ Features

- 🎫 Consulta de saldo e extrato de benefícios em tempo real
- 🔐 Autenticação e autorização integrada com Keycloak
- 👤 Gestão de elegibilidade de beneficiários
- 🔄 Integração com autorizador Edenred
- 📊 Relatórios e auditoria de uso
- 🧪 Suite de testes com cobertura > 80%

---

## 📋 Requisitos

| Ferramenta | Versão mínima | Notas |
|---|---|---|
| Node.js | `>= 20` | Recomendado via [nvm](https://github.com/nvm-sh/nvm) |
| npm | `>= 9` | ou pnpm `>= 8` |
| PostgreSQL | `>= 15` | ou via Docker Compose |
| Docker | `>= 24` | Opcional, mas recomendado pra infra local |

> **Note**
> Usando nvm? Rode `nvm use` na raiz do projeto — o `.nvmrc` já tá configurado.

---

## 🚀 Quick Start

Bora lá. Menos de 5 minutos do zero ao servidor rodando.

### 1. Clone o repositório

```bash
git clone https://github.com/CaramaschiLevva/eden-agents.git
cd ticket-benefits-api
```

### 2. Instale as dependências

```bash
# npm
npm install

# ou pnpm
pnpm install
```

### 3. Configure as variáveis de ambiente

```bash
cp .env.example .env
```

Abre o `.env` e preenche os valores marcados com `# REQUIRED`. Os opcionais já
têm defaults razoáveis.

> **Warning**
> ⚠️ Nunca commita o `.env` real. Ele já tá no `.gitignore`, mas fica de olho.

### 4. Sobe a infra local (opcional, via Docker)

```bash
docker compose up -d
```

### 5. Roda o servidor de desenvolvimento

```bash
npm run dev
```

Tá na mão. O servidor sobe em `http://localhost:3000`.

---

## 📁 Estrutura do Projeto

```text
ticket-benefits-api/
├── src/
│   ├── auth/          # Autenticação e autorização
│   ├── benefits/      # Lógica de negócio de benefícios
│   ├── transactions/  # Processamento de transações
│   ├── shared/                        # Utilitários, tipos e helpers compartilhados
│   └── main.ts                        # Entry point da aplicação
├── tests/
│   ├── unit/                          # Testes unitários (Jest/Vitest)
│   ├── integration/                   # Testes de integração
│   └── fixtures/                      # Dados de teste — NUNCA dados reais
├── .github/
│   └── workflows/                     # Pipelines de CI/CD (GitHub Actions)
├── docker-compose.yml                 # Infra local (banco, cache, etc.)
├── .env.example                       # Template de variáveis de ambiente
├── AGENTS.md                          # Contexto para AI agents
├── CONTRIBUTING.md                    # Guia de contribuição
└── package.json
```

> **Note**
> Monorepo? A estrutura acima representa um único serviço/pacote. Em monorepos,
> cada pacote fica em `packages/[nome]/` com a mesma estrutura interna.

---

## 🛠️ Scripts Disponíveis

| Comando | O que faz | Quando usar |
|---|---|---|
| `npm run dev` | Sobe o servidor com hot-reload | Desenvolvimento do dia a dia |
| `npm run build` | Compila TypeScript para `dist/` | Antes de deployar |
| `npm run start` | Roda o build compilado | Produção / staging |
| `npm run test` | Roda todos os testes | Antes de abrir PR |
| `npm run test:watch` | Testes em modo watch | Durante desenvolvimento |
| `npm run test:coverage` | Testes com relatório de cobertura | Verificar gaps de teste |
| `npm run lint` | Checa estilo e erros com ESLint | CI e pré-commit |
| `npm run lint:fix` | Corrige erros de lint automaticamente | Antes de commitar |
| `npm run typecheck` | Valida tipos sem compilar | Checar erros de tipo rapidinho |
| `npm run db:migrate` | Roda migrations pendentes do banco de dados | Após pull com novas migrations |

---

## 🔒 Segurança

Este projeto opera em domínio financeiro regulado. Leva isso a sério.

> **Important**
> 🚨 Qualquer mudança em lógica de benefício, saldo, elegibilidade, autorização,
> LGPD ou integrações corporativas **exige revisão humana** antes do merge. Não
> tem exceção.

**Regras inegociáveis:**

- **Secrets:** nunca commita `.env` real, tokens, certificados ou chaves de API.
  Usa variáveis de ambiente ou um vault (AWS Secrets Manager)
- **PII/LGPD:** CPF, nome completo, endereço, dados bancários e qualquer dado
  pessoal não podem aparecer em logs, testes, fixtures, screenshots ou
  comentários de código
- **Dados de produção:** nunca usar em ambiente local ou de testes. Fixtures
  usam dados sintéticos
- **Credencial hardcoded:** se achar uma, abre uma issue imediatamente. Não
  commita o fix sem review de segurança
- **Dependências:** rode `npm audit` regularmente. PRs com vulnerabilidades
  críticas são bloqueados no CI

---

## 🐛 Troubleshooting

**Sintoma:** `Error: Cannot connect to database`
**Causa:** Banco não tá rodando ou as credenciais no `.env` estão erradas
**Fix:** Confere se o Docker tá up (`docker compose ps`) e se `DATABASE_URL` no
`.env` bate com o que tá no `docker-compose.yml`

---

**Sintoma:** `Module not found` ou erros de import estranhos após `git pull`
**Causa:** Dependências novas foram adicionadas e você não rodou install
**Fix:** `npm install` (ou `pnpm install`) e tenta de novo

---

**Sintoma:** TypeScript reclamando de tipos que parecem corretos
**Causa:** Cache do compilador desatualizado
**Fix:** Apaga a pasta `dist/` e o `tsconfig.tsbuildinfo`, depois
`npm run typecheck`

---

**Sintoma:** Testes falhando só na sua máquina (passam no CI)
**Causa:** Variável de ambiente faltando ou versão de Node diferente
**Fix:** Confere o `.env` contra o `.env.example` e verifica a versão do Node
com `node -v`

---

**Sintoma:** Hot-reload não funciona / servidor não reinicia ao salvar
**Causa:** Problema com o watcher do sistema operacional (comum no WSL/Windows)
**Fix:** Tenta `npm run dev -- --poll` ou configura `CHOKIDAR_USEPOLLING=true`
no `.env`

---

## 🤝 Contributing

Boa pergunta — a gente tem um guia completo. Antes de abrir PR, lê o
[`CONTRIBUTING.md`](./CONTRIBUTING.md). Lá tem o fluxo de branches, padrão de
commits, como rodar os testes e o checklist de PR.

---

## 🤖 AI Agents

Usando Copilot, Cursor, Claude ou qualquer AI agent? O [`AGENTS.md`](./AGENTS.md)
tem o contexto do projeto, guardrails de segurança e instruções específicas pra
não gerar código que viola as regras do domínio.

---

## 📄 License

UNLICENSED — veja o arquivo [LICENSE](./LICENSE) para detalhes.
