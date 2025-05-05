# 📘 Parte 1 - Subir o Projeto Node.js MVC com PostgreSQL

Este projeto demonstra a estrutura básica de uma aplicação Node.js utilizando o padrão MVC (Model-View-Controller) com conexão a um banco de dados PostgreSQL.

O objetivo é fornecer um exemplo simples e didático, ideal para iniciantes em backend e APIs REST.

---

## ✅ Como Executar

### 1. Clone o repositório

```bash
git clone https://github.com/afonsobrandaointeli/mvc-boilerplate.git
cd mvc-boilerplate
```

### 2. Instale as dependências

```bash
npm install
npm init -y
npm install express ejs
```

### 3. Configure o banco de dados PostgreSQL

Adicione as seguintes configurações em um arquivo `.env`:

```env
DB_HOST=
DB_PORT=5432
DB_USER=
DB_PASSWORD=
DB_DATABASE=
DB_SSL=true
PORT=3000
```

---

## 🧱 Estrutura do Projeto

```text
mvc-boilerplate-main/
├── .env
├── .gitignore
├── jest.config.js
├── package-lock.json
├── package.json
├── readme.md
├── rest.http
├── server.js
├── assets/
│   └── favicon.ico
├── config/
│   └── db.js
├── controllers/
│   └── userController.js
├── documentos/
│   └── wad.md
├── models/
│   └── userModel.js
```

---

## 🧩 Explicação de Cada Parte

### `server.js` - Ponto de entrada do servidor (arquivo vazio).

Este arquivo inicia o servidor Express e define as rotas disponíveis.

---

### `config/db.js` - Conexão com o banco de dados PostgreSQL

Este módulo utiliza a biblioteca `pg` para conectar ao banco usando variáveis do `.env`.

```javascript
const { Pool } = require('pg');
require('dotenv').config();

const pool = new Pool({
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_DATABASE,
  ssl: {
    rejectUnauthorized: false
  }
});

module.exports = pool;

```

---

### `models/userModel.js` - Modelagem e consultas no banco

Define funções para buscar e criar usuários diretamente no banco.

```javascript
const db = require('../config/db');

class User {
  static async getAll() {
    const result = await db.query('SELECT * FROM users');
    return result.rows;
  }

  static async getById(id) {
    const result = await db.query('SELECT * FROM users WHERE id = $1', [id]);
    return result.rows[0];
  }

  static async create(data) {
    const result = await db.query(
      'INSERT INTO users (name, email) VALUES ($1, $2) RETURNING *',
      [data.name, data.email]
    );
    return result.rows[0];
  }

  static async update(id, data) {
    const result = await db.query(
      'UPDATE users SET name = $1, email = $2 WHERE id = $3 RETURNING *',
      [data.name, data.email, id]
    );
    return result.rows[0];
  }

  static async delete(id) {
    const result = await db.query('DELETE FROM users WHERE id = $1 RETURNING *', [id]);
    return result.rowCount > 0;
  }
}

module.exports = User;
```

---

### `controllers/userController.js` - Lógica das requisições

Intermedia o que chega pela requisição e o que será feito com os dados.

```javascript
// controllers/userController.js

const userService = require('../services/userService');

const getAllUsers = async (req, res) => {
  try {
    const users = await userService.getAllUsers();
    res.status(200).json(users);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

const getUserById = async (req, res) => {
  try {
    const user = await userService.getUserById(req.params.id);
    if (user) {
      res.status(200).json(user);
    } else {
      res.status(404).json({ error: 'Usuário não encontrado' });
    }
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

const createUser = async (req, res) => {
  try {
    const { name, email } = req.body;
    const newUser = await userService.createUser(name, email);
    res.status(201).json(newUser);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

const updateUser = async (req, res) => {
  try {
    const { name, email } = req.body;
    const updatedUser = await userService.updateUser(req.params.id, name, email);
    if (updatedUser) {
      res.status(200).json(updatedUser);
    } else {
      res.status(404).json({ error: 'Usuário não encontrado' });
    }
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

const deleteUser = async (req, res) => {
  try {
    const deletedUser = await userService.deleteUser(req.params.id);
    if (deletedUser) {
      res.status(200).json(deletedUser);
    } else {
      res.status(404).json({ error: 'Usuário não encontrado' });
    }
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

module.exports = {
  getAllUsers,
  getUserById,
  createUser,
  updateUser,
  deleteUser
};
```

---

### `init.sql` - Criação da tabela no banco

Script SQL que pode ser executado diretamente no banco para criar a tabela `usuarios`.

```sql
Erro ao ler: [Errno 2] No such file or directory: 'mvc-boilerplate-main/init.sql'
```

---

### `runSQLScripts.js` - Executa o SQL automaticamente via Node

Roda o script `init.sql` automaticamente usando Node.js.

```javascript
-- init.sql

-- Criar extensão para suportar UUIDs, se ainda não estiver ativada
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Criar tabela de usuários com UUID como chave primária
CREATE TABLE IF NOT EXISTS users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL
);

-- Inserir 20 usuários com nomes e emails aleatórios
INSERT INTO users (name, email)
VALUES 
  ('Alice Smith', 'alice.smith@example.com'),
  ('Bob Johnson', 'bob.johnson@example.com'),
  ('Carol Williams', 'carol.williams@example.com'),
  ('David Jones', 'david.jones@example.com'),
  ('Emma Brown', 'emma.brown@example.com'),
  ('Frank Davis', 'frank.davis@example.com'),
  ('Grace Wilson', 'grace.wilson@example.com'),
  ('Henry Moore', 'henry.moore@example.com'),
  ('Isabella Taylor', 'isabella.taylor@example.com'),
  ('Jack Lee', 'jack.lee@example.com'),
  ('Kate Clark', 'kate.clark@example.com'),
  ('Liam Martinez', 'liam.martinez@example.com'),
  ('Mia Rodriguez', 'mia.rodriguez@example.com'),
  ('Noah Garcia', 'noah.garcia@example.com'),
  ('Olivia Hernandez', 'olivia.hernandez@example.com'),
  ('Patrick Martinez', 'patrick.martinez@example.com'),
  ('Quinn Lopez', 'quinn.lopez@example.com'),
  ('Rose Thompson', 'rose.thompson@example.com'),
  ('Samuel Perez', 'samuel.perez@example.com'),
  ('Tara Scott', 'tara.scott@example.com');
```

---

## 🧪 Teste com REST Client

Use o VSCode com a extensão REST Client para executar requisições via o arquivo `rest.http`.

---

## ▶️ Execução do Projeto

Após tudo configurado:

```bash
node server.js
```

Saída esperada:

```
Servidor rodando em http://localhost:3000
```

---

## 🧠 Observação Didática

- **Model** → A camada que acessa diretamente o banco de dados.
- **Controller** → Recebe a requisição, executa lógica e responde.
- **Config** → Guarda configurações de conexão.
- **View** → Não está implementada neste exemplo, mas poderia ser feita com EJS.


---
---
---


# 🧠 Parte 2 - Projeto CRUD com Node.js + PostgreSQL (Supabase)

## 🎯 Objetivo

Este projeto ensina como criar uma aplicação web com formulário que realiza operações **CRUD** — Criar, Ler (listar), Atualizar e Deletar registros — utilizando os seguintes recursos:

- 🧱 Arquitetura MVC (Model-View-Controller): separa as responsabilidades em camadas organizadas.
- 🟩 Node.js + Express: back-end em JavaScript com servidor HTTP leve.
- 🛢️ PostgreSQL (via Supabase): banco de dados relacional gerenciado.
- 🖼️ EJS (Embedded JavaScript): mecanismo para renderizar HTML com dados dinâmicos.

---

## 1. 🗃️ Criação da Tabela `aluno` no Supabase

Acesse o painel do [Supabase](https://app.supabase.com/), vá até **SQL Editor** e cole o seguinte código:

```sql
CREATE TABLE IF NOT EXISTS aluno (
  id SERIAL PRIMARY KEY,
  nome TEXT NOT NULL,
  email TEXT NOT NULL,
  criado_em TIMESTAMP DEFAULT NOW()
);

CREATE INDEX IF NOT EXISTS idx_aluno_email ON aluno (email);
```

> 🔧 Esse script cria a tabela `aluno` com os campos `id`, `nome`, `email` e a data de criação. Também é criado um índice para buscas rápidas pelo campo `email`.

Clique em **Run** para executar a query.

---

## 2. ✅ Estrutura do Projeto

```
📦 mvc-boilerplate
┣ 📂config         → Conexão com o banco (db.js)
┣ 📂models         → Acesso aos dados (aluno.js)
┣ 📂controllers    → Lógica da aplicação (alunoController.js)
┣ 📂routes         → Rotas da aplicação (alunos.js)
┣ 📂views          → Arquivos HTML renderizados com EJS
┃ ┗ 📂alunos
┃   ┗ 📜 index.ejs
┣ 📜 app.js        → Arquivo principal que inicializa a aplicação
┣ 📜 .env          → Variáveis de ambiente (credenciais do banco)
┣ 📜 package.json  → Dependências do projeto
```

> 📌 Essa estrutura segue o padrão MVC, facilitando a manutenção e organização do código.

---

## 3. 📁 Arquivo `models/aluno.js`

Responsável por interagir diretamente com o banco de dados Supabase:

```javascript
const db = require('../config/db');

module.exports = {
  async create(data) {
    const query = 'INSERT INTO aluno (nome, email) VALUES ($1, $2)';
    const values = [data.nome, data.email];
    return db.query(query, values);
  },

  async findAll() {
    const result = await db.query('SELECT * FROM aluno ORDER BY id ASC');
    return result.rows;
  },

  async update(id, data) {
    const query = 'UPDATE aluno SET nome = $1, email = $2 WHERE id = $3';
    const values = [data.nome, data.email, id];
    return db.query(query, values);
  },

  async delete(id) {
    const query = 'DELETE FROM aluno WHERE id = $1';
    return db.query(query, [id]);
  }
};
```

---

## 4. 📁 Arquivo `controllers/alunoController.js`

Controla o fluxo de dados entre o model e a view.

```javascript
const Aluno = require('../models/aluno');

exports.index = async (req, res) => {
  const alunos = await Aluno.findAll();
  res.render('alunos/index', { alunos });
};

exports.store = async (req, res) => {
  await Aluno.create(req.body);
  res.redirect('/alunos');
};

exports.update = async (req, res) => {
  const { id } = req.params;
  await Aluno.update(id, req.body);
  res.redirect('/alunos');
};

exports.destroy = async (req, res) => {
  const { id } = req.params;
  await Aluno.delete(id);
  res.redirect('/alunos');
};
```

---

## 5. 📁 Arquivo `routes/alunos.js`

Define as rotas para as operações CRUD.

```javascript
const express = require('express');
const router = express.Router();
const controller = require('../controllers/alunoController');

router.get('/', controller.index);
router.post('/', controller.store);
router.post('/edit/:id', controller.update);
router.post('/delete/:id', controller.destroy);

module.exports = router;
```

---

## 6. 📜 Arquivo `app.js`

Configura o servidor Express e integra todas as rotas.

```javascript
const express = require('express');
const app = express();
const path = require('path');
const alunosRoutes = require('./routes/alunos');
const bodyParser = require('body-parser');
require('dotenv').config();

app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

app.use(bodyParser.urlencoded({ extended: true }));
app.use(express.static('public'));

app.use('/alunos', alunosRoutes);

app.get('/', (req, res) => {
  res.redirect('/alunos');
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Servidor rodando em http://localhost:${PORT}`);
});
```

---

## 7. 📄 Arquivo `views/alunos/index.ejs`

Renderiza a interface de cadastro e listagem de alunos.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>CRUD de Alunos</title>
</head>
<body>
  <h1>Cadastro de Alunos</h1>

  <form action="/alunos" method="POST">
    <input name="nome" placeholder="Nome" required>
    <input name="email" placeholder="Email" required>
    <button type="submit">Adicionar</button>
  </form>

  <hr>

  <ul>
    <% alunos.forEach(aluno => { %>
      <li>
        <%= aluno.nome %> - <%= aluno.email %>
        <form action="/alunos/edit/<%= aluno.id %>" method="POST" style="display:inline">
          <input name="nome" placeholder="Novo nome" required>
          <input name="email" placeholder="Novo email" required>
          <button type="submit">Editar</button>
        </form>
        <form action="/alunos/delete/<%= aluno.id %>" method="POST" style="display:inline">
          <button type="submit">Apagar</button>
        </form>
      </li>
    <% }) %>
  </ul>
</body>
</html>
```

---

## ▶️ Como Rodar o Projeto Localmente

1. Inicie o servidor:

```bash
node app.js
```

4. Acesse [http://localhost:3000](http://localhost:3000) no navegador.

---

## 📄 Licença

Este projeto está sob a licença MIT.

