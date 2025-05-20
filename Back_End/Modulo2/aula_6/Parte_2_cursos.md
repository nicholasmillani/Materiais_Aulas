
# 📘 CRUD de Cursos com Integração à Página de Alunos (MVC com HTML básico e Express)

## 🎯 Objetivo

Implementar um sistema de cadastro, edição e remoção de **cursos**, utilizando:

- Formulários HTML já existentes em `index.ejs`
- Arquitetura **MVC** com `Model`, `Controller` e `Route`
- Integração visual e funcional com o CRUD de alunos

---

## 🗂️ Estrutura Utilizada

```
📁 models/curso.js
📁 controllers/cursoController.js
📁 routes/cursos.js
📄 views/alunos/index.ejs (compartilhado com alunos)
📄 app.js
```

---

## 1️⃣ View (HTML com EJS)

📄 Caminho: `views/alunos/index.ejs`

```ejs
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Gestão de Alunos e Cursos</title>
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
      <% }); %>
    </select>
    <button type="submit">Cadastrar</button>
  </form>

  <hr>

  <h2>Cursos Cadastrados</h2>
  <table border="1">
    <thead>
      <tr>
        <th>Nome do Curso</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody>
      <% cursos.forEach(curso => { %>
        <tr>
          <td><%= curso.nome %></td>
          <td>
            <form action="/cursos/delete/<%= curso.id %>" method="POST" style="display:inline;">
              <button type="submit">Deletar</button>
            </form>
            <form action="/cursos/edit/<%= curso.id %>" method="POST" style="display:inline;">
              <input name="nome" placeholder="Novo nome do curso" required />
              <button type="submit">Editar</button>
            </form>
          </td>
        </tr>
      <% }); %>
    </tbody>
  </table>

  <h3>Adicionar Novo Curso</h3>
  <form action="/cursos" method="POST">
    <input name="nome" placeholder="Nome do curso" required />
    <button type="submit">Cadastrar</button>
  </form>
</body>
</html>

```

---

## 2️⃣ Controller

📄 `controllers/cursoController.js`

```js
const Curso = require('../models/curso');

exports.create = async (req, res) => {
  const { nome } = req.body;
  await Curso.create(nome);
  res.redirect('/alunos');
};

exports.update = async (req, res) => {
  const { id } = req.params;
  const { nome } = req.body;
  await Curso.update(id, nome);
  res.redirect('/alunos');
};

exports.delete = async (req, res) => {
  const { id } = req.params;
  await Curso.delete(id);
  res.redirect('/alunos');
};

```

---

## 3️⃣ Model

📄 `models/curso.js`

```js
const db = require('../config/db');

module.exports = {
  async findAll() {
    const result = await db.query('SELECT * FROM curso ORDER BY nome ASC');
    return result.rows;
  },

  async create(nome) {
    const query = 'INSERT INTO curso (nome) VALUES ($1) RETURNING *';
    const result = await db.query(query, [nome]);
    return result.rows[0];
  },

  async update(id, nome) {
    const query = 'UPDATE curso SET nome = $1 WHERE id = $2 RETURNING *';
    const result = await db.query(query, [nome, id]);
    return result.rows[0];
  },

  async delete(id) {
    await db.query('DELETE FROM curso WHERE id = $1', [id]);
  }
};

```

---

## 4️⃣ Rotas

📄 `routes/cursos.js`

```js
const express = require('express');
const router = express.Router();
const controller = require('../controllers/cursoController');

router.post('/', controller.create);
router.post('/edit/:id', controller.update);
router.post('/delete/:id', controller.delete);

module.exports = router;
```

---

## 5️⃣ Registro no `app.js` talvez vc ja tenha essa parte, caso ja possua desconsidere

```js
const cursosRoutes = require('./routes/cursos');
app.use('/cursos', cursosRoutes);
```

---

## ✅ Resultado Esperado

| Ação                              | Resultado                                |
|-----------------------------------|-------------------------------------------|
| Cadastro de curso via formulário  | Curso salvo e listado na tabela           |
| Edição de curso                   | Nome alterado após submissão              |
| Remoção de curso                  | Curso excluído da listagem                |
| Associação ao aluno               | Curso aparece no `<select>` do formulário |

---

## ✅ Conclusão

Você agora possui um sistema completo com:

- Interface HTML única para alunos e cursos
- Backend modular em Express com controllers e models separados
- Banco de dados gerenciado via modelos SQL
