# Collection Platform - README

## Sobre o Projeto

Collection Platform é uma aplicação web para gerenciamento de coleções e seus itens. Este projeto inclui:

- Frontend em Next.js
- API RESTful com Express e TypeScript
- Banco de dados SQLite com Prisma ORM
- Operações CRUD completas para coleções e itens

## Estrutura do Projeto

```
collection_platform/
├── prisma/                 # Configurações do Prisma ORM
│   ├── schema.prisma       # Definição dos modelos de dados
│   └── migrations/         # Migrações do banco de dados
├── src/
│   ├── api/                # Backend API
│   │   ├── prisma.ts       # Cliente Prisma compartilhado
│   │   ├── server.ts       # Servidor Express
│   │   ├── routes/         # Rotas da API
│   │   └── tests/          # Testes da API
│   └── app/                # Frontend Next.js
├── .env                    # Variáveis de ambiente
├── API_DOCUMENTATION.md    # Documentação detalhada da API
├── tsconfig.json           # Configuração TypeScript para Next.js
├── tsconfig.api.json       # Configuração TypeScript para a API
└── package.json            # Dependências e scripts
```

## Começando

### Pré-requisitos

- Node.js (versão 18 ou superior)
- npm ou yarn
- VS Code (recomendado)

### Instalação

1. Clone o repositório ou extraia os arquivos do projeto
2. Abra o projeto no VS Code
3. Instale as dependências:

```bash
npm install
```

### Configuração do Banco de Dados

O projeto utiliza SQLite como banco de dados. Para inicializar o banco de dados:

```bash
npx prisma migrate dev
```

### Executando a API

Para iniciar a API em modo de desenvolvimento:

```bash
npm run api:dev
```

A API estará disponível em `http://localhost:3001`.

### Executando o Frontend

Para iniciar o frontend Next.js:

```bash
npm run dev
```

O frontend estará disponível em `http://localhost:3000`.

## Scripts Disponíveis

- `npm run dev` - Inicia o frontend Next.js em modo de desenvolvimento
- `npm run api:dev` - Inicia a API em modo de desenvolvimento
- `npm run api:build` - Compila a API para produção
- `npm run api:start` - Inicia a API em modo de produção
- `npm run prisma:studio` - Abre o Prisma Studio para visualizar o banco de dados

## Documentação

Para uma documentação detalhada da API, consulte o arquivo [API_DOCUMENTATION.md](./API_DOCUMENTATION.md).

## Licença

Este projeto está licenciado sob a licença MIT.
