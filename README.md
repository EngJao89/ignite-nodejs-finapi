## FinAPI - Financeira

API REST para simulação de operações bancárias: criação de contas, depósitos, saques, extrato e saldo. Projeto didático do Ignite (Node.js), desenvolvido com Express.

---

## Como rodar o projeto

### Pré-requisitos

- [Node.js](https://nodejs.org/) (versão LTS recomendada)
- npm ou yarn

### Instalação

```bash
# Clone o repositório (se ainda não tiver)
git clone <url-do-repositorio>
cd ignite-nodejs-finapi

# Instale as dependências
npm install
# ou
yarn install
```

### Execução

```bash
# Modo desenvolvimento (com hot reload via nodemon)
npm run dev
# ou
yarn dev
```

A API estará disponível em **http://localhost:3333**.

### Endpoints principais

| Método | Rota | Descrição |
|--------|------|-----------|
| POST | `/account` | Criar conta (body: `cpf`, `name`) |
| GET | `/statement` | Extrato (header: `cpf`) |
| GET | `/statement/date?date=YYYY-MM-DD` | Extrato por data (header: `cpf`) |
| POST | `/deposit` | Depósito (header: `cpf`, body: `description`, `amount`) |
| POST | `/withdraw` | Saque (header: `cpf`, body: `amount`) |
| GET | `/balance` | Saldo (header: `cpf`) |
| GET | `/account` | Dados da conta (header: `cpf`) |
| PUT | `/account` | Atualizar nome (header: `cpf`, body: `name`) |
| DELETE | `/account` | Deletar conta (header: `cpf`) |

Em todas as rotas que exigem conta (exceto `POST /account`), envie o **CPF no header**: `cpf: <numero-do-cpf>`.

---

## Decisões técnicas e arquiteturais

- **Runtime e framework:** Node.js com **Express** para roteamento, middlewares e parsing de JSON. Escolha simples e adequada para uma API REST didática.
- **Identificação do cliente:** Uso do **CPF no header** (`request.headers.cpf`) para identificar a conta em todas as operações autenticadas, sem camada de login/senha, mantendo o foco em fluxo de negócio.
- **Middleware de validação:** O middleware `verifyIfExistsAccountCPF` centraliza a busca da conta por CPF e a injeção do `customer` em `request`, evitando repetição de código e garantindo que rotas protegidas só executem com conta existente.
- **Persistência em memória:** Dados armazenados em um array `customers` no processo. Não há banco de dados; ao reiniciar o servidor, os dados são perdidos. Adequado para estudo e testes.
- **IDs únicos:** Uso da biblioteca **uuid** (v4) para identificador único de cada conta (`id`), permitindo evolução futura para chave estável mesmo com mudança de CPF.
- **Extrato (statement):** Lista de operações por conta, com `type` (`credit`/`debit`), `amount`, `created_at` e opcionalmente `description`. O saldo é calculado em tempo de requisição via `getBalance(statement)`.
- **Filtro de extrato por data:** Implementado em rota própria (`GET /statement/date`) com comparação por data (ignorando horário), usando `Date` e `toDateString()`.
- **Convenção de commits:** Projeto configurado com **Commitizen** e **cz-conventional-changelog** (`npm run commit`) para mensagens de commit padronizadas.

---

## Requisitos

- [x] Deve ser possível criar uma conta
- [x] Deve ser possível buscar o extrato bancário do cliente
- [x] Deve ser possível realizar um depósito
- [x] Deve ser possível realizar um saque
- [x] Deve ser possível buscar o extrato bancário do cliente por data
- [x] Deve ser possível atualizar dados da conta do cliente
- [x] Deve ser possível obter dados da conta do cliente
- [x] Deve ser possível deletar uma conta
- [x] Deve ser possível retornar o balance

---

## Regras de negócio

- [x] Não deve ser possível cadastrar uma conta com CPF já existente
- [x] Não deve ser possível buscar extrato em uma conta não existente
- [x] Não deve ser possível fazer depósito em uma conta não existente
- [x] Não deve ser possível fazer saque em uma conta não existente
- [x] Não deve ser possível fazer saque quando o saldo for insuficiente
- [x] Não deve ser possível excluir uma conta não existente

<!--START_SECTION:footer-->

<br />
<br />

<p align="center">
  <a href="https://discord.gg/rocketseat" target="_blank">
    <img align="center" src="https://storage.googleapis.com/golden-wind/comunidade/rodape.svg" alt="banner"/>
  </a>
</p>

<!--END_SECTION:footer-->