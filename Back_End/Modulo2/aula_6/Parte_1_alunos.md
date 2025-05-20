
# 📘 CRUD de Alunos com HTML, JSON e MVC

Este projeto demonstra como implementar um CRUD de alunos utilizando a arquitetura MVC em Node.js com Express, HTML via EJS e endpoints com resposta em JSON.

---

## 🎯 Objetivo

- Criar páginas com formulário HTML básico (View)
- Processar dados recebidos e retorná-los em JSON (Controller + Model)
- Utilizar a arquitetura MVC separando responsabilidades

---

## 🗂️ Estrutura

```
📁 models/aluno.js
📁 controllers/alunoController.js
📁 routes/alunos.js
📁 views/alunos/index.ejs
📄 app.js
```

---

## 1️⃣ View: HTML com EJS

📄 `views/alunos/index.ejs`

```ejs
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Lista de Alunos</title>
</head>
<body>
  <h1>Alunos Cadastrados</h1>

  <table border="1">
    <thead>
      <tr>
        <th>Nome</th>
        <th>Email</th>
        <th>Curso</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody>
      <% alunos.forEach(aluno => { %>
        <tr>
          <td><%= aluno.nome %></td>
          <td><%= aluno.email %></td>
          <td><%= aluno.curso || "Sem curso" %></td>
          <td>
            <form action="/alunos/delete/<%= aluno.id %>" method="POST" style="display:inline;">
              <button type="submit">Deletar</button>
            </form>
            <form action="/alunos/edit/<%= aluno.id %>" method="POST" style="display:inline;">
              <input name="nome" placeholder="Novo nome" required />
              <input name="email" placeholder="Novo email" required />
              <button type="submit">Editar</button>
            </form>
          </td>
        </tr>
      <% }); %>
    </tbody>
  </table>

  <h2>Cadastrar Novo Aluno</h2>
  <form action="/alunos" method="POST">
    <input name="nome" placeholder="Nome" required />
    <input name="email" placeholder="Email" required />
    <select name="curso_id">
      <option value="">Selecione um curso</option>
      <% cursos.forEach(curso => { %>
        <option value="<%= curso.id %>"><%= curso.nome %></option>
      <% }) %>
    </select>
    <button type="submit">Cadastrar</button>
  </form>
</body>
</html>
```

---

## 2️⃣ Controller: `controllers/alunoController.js`

```js
const Aluno = require('../models/aluno');
const Curso = require('../models/curso');

exports.index = async (req, res) => {
  const alunos = await Aluno.findAllComCurso();
  const cursos = await Curso.findAll();
  res.render('alunos/index', { alunos, cursos });
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

exports.byCurso = async (req, res) => {
  const { curso_id } = req.params;
  const alunos = await Aluno.findByCurso(curso_id);
  res.json(alunos);
};
```

---

## 3️⃣ Model: `models/aluno.js`

```js
const db = require('../config/db');

module.exports = {
  async create(data) {
    const query = 'INSERT INTO aluno (nome, email, curso_id) VALUES ($1, $2, $3)';
    const values = [data.nome, data.email, data.curso_id || null];
    return db.query(query, values);
  },

  async findAllComCurso() {
    const query = `
      SELECT aluno.id, aluno.nome, aluno.email, curso.nome AS curso
      FROM aluno
      LEFT JOIN curso ON aluno.curso_id = curso.id
      ORDER BY aluno.id ASC
    `;
    const result = await db.query(query);
    return result.rows;
  },

  async findByCurso(curso_id) {
    const query = 'SELECT aluno.id, aluno.nome, aluno.email FROM aluno WHERE curso_id = $1 ORDER BY nome ASC';
    const result = await db.query(query, [curso_id]);
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

## 4️⃣ Rotas: `routes/alunos.js`

```js
const express = require('express');
const router = express.Router();
const controller = require('../controllers/alunoController');

router.get('/', controller.index);
router.post('/', controller.store);
router.post('/edit/:id', controller.update);
router.post('/delete/:id', controller.destroy);
router.get('/curso/:curso_id', controller.byCurso);

module.exports = router;
```

---

## ✅ Testes

| Caminho                             | Resultado                            |
|-------------------------------------|---------------------------------------|
| `/alunos`                           | Lista alunos e formulário             |
| `/alunos/curso/:curso_id`           | Retorna alunos em JSON                |

---

## ✅ Conclusão

Esse exemplo implementa a arquitetura MVC completa, utilizando HTML básico com EJS e rotas RESTful com entrada e saída de dados via JSON.
