# Collection Platform - Documentação da API

## Visão Geral

Esta documentação descreve a implementação da API CRUD para a Collection Platform, uma plataforma para gerenciamento de coleções e seus itens. A API foi desenvolvida utilizando Express.js, TypeScript e Prisma ORM, proporcionando uma interface robusta e tipada para operações de banco de dados.

## Estrutura do Projeto

O projeto está organizado da seguinte forma:

```
collection_platform/
├── prisma/
│   ├── schema.prisma       # Definição dos modelos de dados
│   └── migrations/         # Migrações do banco de dados
├── src/
│   ├── api/
│   │   ├── prisma.ts       # Cliente Prisma compartilhado
│   │   ├── server.ts       # Configuração do servidor Express
│   │   ├── routes/
│   │   │   ├── collections.ts  # Rotas para coleções
│   │   │   └── items.ts        # Rotas para itens
│   │   └── tests/
│   │       └── api.test.ts     # Testes da API
│   └── app/                # Aplicação Next.js (frontend)
├── .env                    # Variáveis de ambiente
├── tsconfig.json           # Configuração TypeScript para Next.js
├── tsconfig.api.json       # Configuração TypeScript para a API
└── package.json            # Dependências e scripts
```

## Modelos de Dados

A API trabalha com dois modelos principais:

### Collection (Coleção)

Representa uma coleção de itens relacionados.

```prisma
model Collection {
  id          Int       @id @default(autoincrement())
  name        String
  description String?
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
  items       Item[]
}
```

### Item

Representa um item individual pertencente a uma coleção.

```prisma
model Item {
  id           Int        @id @default(autoincrement())
  name         String
  description  String?
  imageUrl     String?
  collectionId Int
  collection   Collection @relation(fields: [collectionId], references: [id], onDelete: Cascade)
  createdAt    DateTime   @default(now())
  updatedAt    DateTime   @updatedAt
}
```

## Endpoints da API

A API fornece os seguintes endpoints para operações CRUD:

### Coleções

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | /api/collections | Lista todas as coleções |
| GET | /api/collections/:id | Busca uma coleção específica por ID |
| POST | /api/collections | Cria uma nova coleção |
| PUT | /api/collections/:id | Atualiza uma coleção existente |
| DELETE | /api/collections/:id | Remove uma coleção |

### Itens

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | /api/items | Lista todos os itens |
| GET | /api/items/:id | Busca um item específico por ID |
| GET | /api/items/collection/:collectionId | Lista itens de uma coleção específica |
| POST | /api/items | Cria um novo item |
| PUT | /api/items/:id | Atualiza um item existente |
| DELETE | /api/items/:id | Remove um item |

## Configuração e Execução

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

O projeto utiliza SQLite como banco de dados por padrão, o que facilita a configuração inicial. O arquivo `.env` já está configurado com a URL do banco de dados:

```
DATABASE_URL="file:./dev.db"
```

Para inicializar o banco de dados e aplicar as migrações:

```bash
npx prisma migrate dev
```

### Executando a API

Para iniciar a API em modo de desenvolvimento:

```bash
npm run api:dev
```

A API estará disponível em `http://localhost:3001`.

Para visualizar o banco de dados através do Prisma Studio:

```bash
npm run prisma:studio
```

O Prisma Studio estará disponível em `http://localhost:5555`.

### Construindo para Produção

Para construir a API para produção:

```bash
npm run api:build
```

Para iniciar a API em modo de produção:

```bash
npm run api:start
```

## Exemplos de Uso

### Criando uma Coleção

```bash
curl -X POST http://localhost:3001/api/collections \
  -H "Content-Type: application/json" \
  -d '{"name": "Minha Coleção", "description": "Descrição da minha coleção"}'
```

Resposta:

```json
{
  "id": 1,
  "name": "Minha Coleção",
  "description": "Descrição da minha coleção",
  "createdAt": "2025-05-23T00:00:00.000Z",
  "updatedAt": "2025-05-23T00:00:00.000Z"
}
```

### Adicionando um Item à Coleção

```bash
curl -X POST http://localhost:3001/api/items \
  -H "Content-Type: application/json" \
  -d '{"name": "Meu Item", "description": "Descrição do meu item", "collectionId": 1}'
```

Resposta:

```json
{
  "id": 1,
  "name": "Meu Item",
  "description": "Descrição do meu item",
  "imageUrl": null,
  "collectionId": 1,
  "createdAt": "2025-05-23T00:00:00.000Z",
  "updatedAt": "2025-05-23T00:00:00.000Z"
}
```

### Listando Coleções com seus Itens

```bash
curl http://localhost:3001/api/collections
```

Resposta:

```json
[
  {
    "id": 1,
    "name": "Minha Coleção",
    "description": "Descrição da minha coleção",
    "createdAt": "2025-05-23T00:00:00.000Z",
    "updatedAt": "2025-05-23T00:00:00.000Z",
    "items": [
      {
        "id": 1,
        "name": "Meu Item",
        "description": "Descrição do meu item",
        "imageUrl": null,
        "collectionId": 1,
        "createdAt": "2025-05-23T00:00:00.000Z",
        "updatedAt": "2025-05-23T00:00:00.000Z"
      }
    ]
  }
]
```

## Testando a API

O projeto inclui um script de teste para verificar o funcionamento dos endpoints. Para executar os testes:

```bash
npx ts-node src/api/tests/api.test.ts
```

Este script testa todas as operações CRUD para coleções e itens, garantindo que a API esteja funcionando corretamente.

## Integração com o Frontend

A API foi projetada para ser facilmente integrada com o frontend Next.js existente. Para consumir a API no frontend, você pode utilizar o módulo `axios` ou o `fetch` nativo:

```typescript
// Exemplo usando fetch
async function fetchCollections() {
  const response = await fetch('http://localhost:3001/api/collections');
  const collections = await response.json();
  return collections;
}

// Exemplo usando axios
import axios from 'axios';

async function createItem(item) {
  const response = await axios.post('http://localhost:3001/api/items', item);
  return response.data;
}
```

## Considerações de Segurança

- A API atualmente não implementa autenticação ou autorização, o que seria necessário em um ambiente de produção.
- Para produção, considere adicionar middleware de segurança como helmet, rate limiting e validação mais rigorosa de entrada.
- O CORS está configurado para permitir todas as origens durante o desenvolvimento. Em produção, restrinja as origens permitidas.

## Próximos Passos

- Implementar autenticação e autorização
- Adicionar validação de entrada mais robusta
- Implementar paginação para endpoints de listagem
- Adicionar documentação interativa com Swagger/OpenAPI
- Implementar testes automatizados mais abrangentes

## Conclusão

Esta implementação fornece uma base sólida para o CRUD da Collection Platform, com uma API RESTful bem estruturada e integração com banco de dados através do Prisma ORM. A arquitetura modular facilita a manutenção e extensão do código, permitindo adicionar novas funcionalidades conforme necessário.
