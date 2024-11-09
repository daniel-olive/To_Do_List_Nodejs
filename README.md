Aqui está a documentação formatada para ser usada no GitHub:

---

# API de Tarefas com TypeScript e Express

Este projeto implementa uma API RESTful em Node.js utilizando o framework Express e a ORM Sequelize para manipulação de um banco de dados PostgreSQL. A API permite realizar operações CRUD (Criar, Ler, Atualizar e Deletar) para gerenciar tarefas (todos).

---

## Estrutura de Pastas e Arquivos

- **`src/controllers/todoController.ts`** - Controladores com as lógicas de CRUD para as tarefas.
- **`src/instances/pg.ts`** - Configuração da instância do banco de dados PostgreSQL.
- **`src/models/Todo.ts`** - Modelo da entidade `Todo`, definindo a estrutura e os tipos de dados.
- **`src/routes/api.ts`** - Rotas da API, associando URLs às funções de controle.
- **`src/server.ts`** - Configuração e inicialização do servidor Express.
- **`.env`** - Arquivo de configuração com variáveis de ambiente.
- **`package.json`** - Gerenciamento de dependências e scripts de execução.

---

## Endpoints da API

### 1. Listar Tarefas
- **Rota**: `GET /todo`
- **Descrição**: Retorna todas as tarefas salvas no banco.
- **Resposta**: Um JSON com a lista de tarefas.

### 2. Adicionar Tarefa
- **Rota**: `POST /todo`
- **Descrição**: Adiciona uma nova tarefa ao banco de dados.
- **Parâmetros**:
  - `title` (string) - Título da tarefa (obrigatório).
  - `done` (boolean) - Status da tarefa (opcional).
- **Resposta**: Retorna a tarefa criada ou uma mensagem de erro se os dados não forem enviados corretamente.

### 3. Atualizar Tarefa
- **Rota**: `PUT /todo/:id`
- **Descrição**: Atualiza uma tarefa existente.
- **Parâmetros**:
  - `id` (path) - ID da tarefa a ser atualizada.
  - `title` (string) - Novo título (opcional).
  - `done` (string: "true"/"false" ou "1"/"0") - Novo status.
- **Resposta**: Retorna a tarefa atualizada ou uma mensagem de erro se o ID não for encontrado.

### 4. Remover Tarefa
- **Rota**: `DELETE /todo/:id`
- **Descrição**: Deleta uma tarefa com base no ID fornecido.
- **Parâmetros**:
  - `id` (path) - ID da tarefa a ser deletada.
- **Resposta**: Confirma a exclusão ou retorna uma mensagem de erro se o ID não for encontrado.

---

## Descrição dos Arquivos

### 1. `controllers/todoController.ts`
Contém os controladores para os endpoints CRUD. Cada função (`all`, `add`, `update`, `remove`) é assíncrona e realiza operações com o modelo `Todo`:
   - `all`: Recupera todas as tarefas.
   - `add`: Cria uma nova tarefa caso o `title` seja fornecido.
   - `update`: Atualiza o `title` ou `done` de uma tarefa pelo ID.
   - `remove`: Exclui uma tarefa pelo ID.

### 2. `instances/pg.ts`
Configura uma conexão com o PostgreSQL usando o Sequelize. Utiliza variáveis de ambiente (`PG_DB`, `PG_USER`, `PG_PASSWORD`, `PG_PORT`) armazenadas no arquivo `.env`.

### 3. `models/Todo.ts`
Define o modelo `Todo` com `sequelize.define`, especificando:
   - `id`: Chave primária, autoincrementada e do tipo inteiro.
   - `title`: Título da tarefa como string.
   - `done`: Status booleano com valor padrão `false`.
   - Configura o nome da tabela como `todos` e desabilita os timestamps.

### 4. `routes/api.ts`
Define as rotas para os endpoints `GET`, `POST`, `PUT` e `DELETE` de tarefas. Importa os métodos do `todoController` e os associa aos endpoints.

### 5. `server.ts`
Configuração do servidor Express:
   - Habilita o `cors` para permitir requisições de origens diferentes.
   - Configura a pasta `public` como estática.
   - Usa `express.urlencoded` para interpretar requisições `POST` com payload `urlencoded`.
   - Configura a rota de saúde `GET /ping` que responde com `{ pong: true }`.
   - Implementa um middleware de tratamento de erros e resposta 404 para rotas não encontradas.

### 6. `.env`
Contém variáveis de ambiente para configuração do servidor e do banco de dados:

```env
PORT=4000
PG_DB=node_todo_simples
PG_USER=postgres
PG_PASSWORD=1234
PG_PORT=5432
```

### 7. `package.json`
Contém scripts e dependências do projeto, incluindo:

- **Dependências**:
  - `cors`: Middleware de CORS para Express.
  - `dotenv`: Carrega variáveis de ambiente.
  - `express`: Framework de servidor HTTP.
  - `pg` e `pg-hstore`: Conectores para PostgreSQL usados com Sequelize.
  - `sequelize`: ORM para interagir com o banco de dados.
  - `validator`: Biblioteca para validação de dados (opcional).
- **DevDependencies**:
  - Tipagens para TypeScript: `@types/cors`, `@types/express`, `@types/node`, `@types/sequelize`, `@types/validator`.
- **Scripts**:
  - `"start-dev"`: Executa o servidor em modo de desenvolvimento com o `nodemon`.

---

## Execução

1. Instale as dependências:
   ```bash
   npm install
   ```

2. Configure o banco de dados PostgreSQL e atualize as variáveis no arquivo `.env`.

3. Inicie o servidor em modo de desenvolvimento:
   ```bash
   npm run start-dev
   ```

---

## Observações

- **Tratamento de Erros**: O arquivo `server.ts` inclui um middleware de tratamento de erros que responde com uma mensagem genérica caso ocorra algum erro inesperado.
- **Validação Simples**: O código verifica apenas a existência de `title` ao adicionar uma nova tarefa, mas uma validação mais robusta pode ser implementada utilizando a biblioteca `validator`. 

Este projeto básico oferece uma base sólida para implementar uma API RESTful com Express e Sequelize.
