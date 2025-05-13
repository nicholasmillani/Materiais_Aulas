
# Parte 4 — Atividade Prática e Conceitual sobre Models e Controllers

## 🎯 Objetivo Geral

Consolidar o entendimento da estrutura MVC (Model-View-Controller) por meio de práticas com `models` e `controllers` no projeto em Node.js já construído.

---

## 🛠 Parte 4A — Atividade Prática: Atualização e Exclusão de Cursos

### 🔧 Desafio 1 — Atualizar um curso

#### 1. No `models/curso.js`, adicione:

```js
// Função no model para atualizar o nome de um curso pelo ID
async update(id, nome) {
  const query = 'UPDATE curso SET nome = $1 WHERE id = $2 RETURNING *';
  const result = await db.query(query, [nome, id]);
  return result.rows[0];
}
```

#### 2. No `controllers/cursoController.js`, adicione:

```js
// Controller que recebe os dados do formulário e chama o model para atualizar o curso
exports.update = async (req, res) => {
  const { id } = req.params;
  const { nome } = req.body;
  await Curso.update(id, nome);
  res.redirect('/alunos');
};
```

#### 3. No `routes/cursos.js`, adicione:

```js
// Rota para enviar o formulário de edição de curso ao controller
router.post('/edit/:id', controller.update);
```

#### 4. Em `views/alunos/index.ejs`, adicione:

```ejs
<!-- Loop para listar todos os cursos e permitir edição -->
<% cursos.forEach(curso => { %>
  <form action="/cursos/edit/<%= curso.id %>" method="POST" style="display:inline;">
    <input name="nome" value="<%= curso.nome %>" required>
    <button type="submit">✏️</button>
  </form>
<% }) %>
```

---

### 🔧 Desafio 2 — Excluir um curso

#### 1. No `models/curso.js`, adicione:

```js
// Função no model que remove um curso do banco de dados pelo ID
async delete(id) {
  await db.query('DELETE FROM curso WHERE id = $1', [id]);
}
```

#### 2. No `controllers/cursoController.js`, adicione:

```js
// Controller que chama o model para deletar o curso e redireciona
exports.delete = async (req, res) => {
  const { id } = req.params;
  await Curso.delete(id);
  res.redirect('/alunos');
};
```

#### 3. Em `routes/cursos.js`, adicione:

```js
// Rota para enviar o pedido de exclusão ao controller
router.post('/delete/:id', controller.delete);
```

#### 4. Em `views/alunos/index.ejs`, adicione:

```ejs
<!-- Formulário para deletar um curso -->
<form action="/cursos/delete/<%= curso.id %>" method="POST" style="display:inline;">
  <button type="submit" onclick="return confirm('Tem certeza que deseja excluir?')">🗑️</button>
</form>
```

---

## ✅ Resultado Esperado da Parte 4A

- Atualização de cursos pela interface.
- Exclusão de cursos diretamente da listagem.
- Consolidação prática da função de cada camada no padrão MVC.

---

# Parte 4B — # 👨‍🏫 Roteiro Prático: Criando o Recurso "Professores" com Node.js e MVC

## 🎯 Objetivo
Adicionar ao sistema a funcionalidade de **cadastrar, listar, editar e excluir professores**, utilizando o padrão de arquitetura **MVC (Model-View-Controller)** no projeto Node.js com EJS.

---

## 📁 Estrutura de Pastas

```
projeto/
├── controllers/
│   └── professorController.js
├── models/
│   └── professor.js
├── routes/
│   └── professores.js
├── views/
│   └── professores/
│       └── index.ejs
└── config/
    └── db.js
└── app.js
```

---

## 🛠 Script SQL para criar a tabela `professor`

Antes de começar o desenvolvimento, execute o seguinte script no seu banco de dados PostgreSQL:

```sql
CREATE TABLE professor (
  id SERIAL PRIMARY KEY,
  nome VARCHAR(100) NOT NULL,
  email VARCHAR(150) NOT NULL
);
```

---

## ✅ Passo a Passo

### 1️⃣ Criar o Model: `models/professor.js`

```js
const db = require('../config/db');

module.exports = {
  // Listar todos os professores
  async findAll() {
    const result = await db.query('SELECT * FROM professor ORDER BY nome ASC');
    return result.rows;
  },

  // Criar um novo professor
  async create(nome, email) {
    const result = await db.query(
      'INSERT INTO professor (nome, email) VALUES ($1, $2) RETURNING *',
      [nome, email]
    );
    return result.rows[0];
  },

  // Atualizar um professor existente
  async update(id, nome, email) {
    const result = await db.query(
      'UPDATE professor SET nome = $1, email = $2 WHERE id = $3 RETURNING *',
      [nome, email, id]
    );
    return result.rows[0];
  },

  // Deletar um professor
  async delete(id) {
    await db.query('DELETE FROM professor WHERE id = $1', [id]);
  }
};
```

---

### 2️⃣ Criar o Controller: `controllers/professorController.js`

```js
const Professor = require('../models/professor');

// Listar todos os professores
exports.index = async (req, res) => {
  const professores = await Professor.findAll();
  res.render('professores/index', { professores });
};

// Criar novo professor
exports.create = async (req, res) => {
  const { nome, email } = req.body;
  await Professor.create(nome, email);
  res.redirect('/professores');
};

// Atualizar dados do professor
exports.update = async (req, res) => {
  const { id } = req.params;
  const { nome, email } = req.body;
  await Professor.update(id, nome, email);
  res.redirect('/professores');
};

// Deletar professor
exports.delete = async (req, res) => {
  const { id } = req.params;
  await Professor.delete(id);
  res.redirect('/professores');
};
```

---

### 3️⃣ Criar as Rotas: `routes/professores.js`

```js
const express = require('express');
const router = express.Router();
const controller = require('../controllers/professorController');

// Rota principal
router.get('/', controller.index);

// Criar novo professor
router.post('/', controller.create);

// Editar professor
router.post('/edit/:id', controller.update);

// Deletar professor
router.post('/delete/:id', controller.delete);

module.exports = router;
```

---

### 4️⃣ Criar a View: `views/professores/index.ejs`

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Cadastro de Professores</title>
</head>
<body>
  <h1>Cadastro de Professores</h1>

  <!-- Formulário para adicionar professor -->
  <form action="/professores" method="POST">
    <input name="nome" placeholder="Nome do professor" required>
    <input name="email" placeholder="Email do professor" required>
    <button type="submit">Adicionar</button>
  </form>

  <hr>

  <h2>Lista de Professores</h2>
  <% professores.forEach(prof => { %>
    <!-- Editar professor -->
    <form action="/professores/edit/<%= prof.id %>" method="POST" style="display:inline;">
      <input name="nome" value="<%= prof.nome %>" required>
      <input name="email" value="<%= prof.email %>" required>
      <button type="submit">✏️</button>
    </form>

    <!-- Deletar professor -->
    <form action="/professores/delete/<%= prof.id %>" method="POST" style="display:inline;">
      <button type="submit" onclick="return confirm('Tem certeza que deseja excluir?')">🗑️</button>
    </form>
    <br>
  <% }) %>
</body>
</html>
```

---

### 5️⃣ Registrar a Rota no `app.js`

No arquivo principal do seu projeto (geralmente `app.js` adicione:

```js
const professoresRoutes = require('./routes/professores');
app.use('/professores', professoresRoutes);
```

---

## ✅ Resultado Esperado

Acesse no navegador:

```
http://localhost:3000/professores
```

Você poderá:

- ✅ Criar novos professores
- ✅ Ver a lista de professores cadastrados
- ✅ Editar nome e e-mail dos professores
- ✅ Deletar qualquer professor

---

## 💡 Dica Extra

> Este roteiro pode ser facilmente adaptado para outros recursos como `disciplinas`, `turmas`, `notas`, etc.  
> Basta replicar a estrutura de `Model`, `Controller`, `Routes` e `View` com os nomes apropriados.

---

## 👨‍🏫 Autor

Professor Cristiano Benites – Inteli / Estiam Paris  
Desenvolvido para alunos em formação prática de sistemas MVC com Node.js.
