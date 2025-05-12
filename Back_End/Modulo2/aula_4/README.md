
# Parte 3 — Relacionamento entre Tabelas, Chaves Estrangeiras e JOINs no CRUD

## 🎯 Objetivo

- Criar um relacionamento entre alunos e cursos (1:N).
- Aplicar LEFT JOIN para exibir o nome do curso junto com os dados do aluno.
- Permitir cadastro e listagem de cursos.
- Permitir associar um curso ao aluno via formulário.

---

## 1. 🧱 Atualize o `init.sql`

> Criamos a tabela `curso` e adicionamos uma chave estrangeira `curso_id` na tabela `aluno`.
> Isso estabelece um relacionamento 1:N entre cursos e alunos.

```sql
CREATE TABLE IF NOT EXISTS curso (
  id SERIAL PRIMARY KEY,
  nome TEXT NOT NULL
);

ALTER TABLE aluno
ADD COLUMN IF NOT EXISTS curso_id INTEGER,
ADD CONSTRAINT fk_curso FOREIGN KEY (curso_id) REFERENCES curso(id) ON DELETE SET NULL;
```

---

## 2. 📄 Crie `models/curso.js`

> Este model fornece funções para listar todos os cursos e criar um novo curso no banco de dados.

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
  }
};
```

---

## 3. 📄 Deixe o mesmo conforme abaixo `models/aluno.js`

> Incluímos os métodos para inserir um aluno com `curso_id`, listar todos os alunos com nome do curso (JOIN),
> e buscar alunos filtrando por um curso específico.

```js
const db = require('../config/db');

module.exports = {
  async create(data) {
    const query = 'INSERT INTO aluno (nome, email, curso_id) VALUES ($1, $2, $3)';
    const values = [data.nome, data.email, data.curso_id || null];
    return db.query(query, values);
  },

  async findAll() {
    const result = await db.query('SELECT * FROM aluno ORDER BY id ASC');
    return result.rows;
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
  const query = `
    SELECT aluno.id, aluno.nome, aluno.email
    FROM aluno
    WHERE curso_id = $1
    ORDER BY nome ASC
  `;
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

## 4. 📄 Crie `controllers/cursoController.js`

> Este controlador permite criar um novo curso através de um formulário HTML e redireciona de volta para `/alunos`.

```js
const Curso = require('../models/curso');

exports.create = async (req, res) => {
  const { nome } = req.body;
  await Curso.create(nome);
  res.redirect('/alunos');
};
```

---

## 5. 📄 Deixe o mesmo conforme abaixo `controllers/alunoController.js`

> Modificamos o controlador para:
> - Listar os cursos disponíveis junto com os alunos.
> - Adicionar uma nova rota que retorna os alunos de um curso específico.

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

## 6. 📄 Crie `routes/cursos.js`

> Define uma nova rota POST para criar cursos.

```js
const express = require('express');
const router = express.Router();
const controller = require('../controllers/cursoController');

router.post('/', controller.create);

module.exports = router;
```

---

## 7. 📄 Deixe o mesmo conforme abaixo `routes/alunos.js`

> Adicionamos rota GET para buscar alunos de um curso específico.

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

## 8. 📄 Deixe o mesmo conforme abaixo `app.js`

> Importamos as rotas de cursos e adicionamos ao Express.

```js
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

const cursosRoutes = require('./routes/cursos');
app.use('/cursos', cursosRoutes);

```

---

## 9. 🖼 Deixe conforme abaixo `views/alunos/index.ejs`

### Formulário de seleção de curso

> Esse trecho gera um `select` com os cursos disponíveis, permitindo relacionar um aluno a um curso no cadastro.

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

    <!-- Seleção de curso -->
    <select name="curso_id">
      <option value="">Selecione um curso</option>
      <% cursos.forEach(curso => { %>
        <option value="<%= curso.id %>"><%= curso.nome %></option>
      <% }) %>
    </select>

    <button type="submit">Adicionar</button>
  </form>

  <!-- Formulário adicional para cadastrar novos cursos -->
  <h2>Cadastrar novo curso</h2>
  <form action="/cursos" method="POST">
    <input name="nome" placeholder="Nome do curso" required>
    <button type="submit">Adicionar Curso</button>
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
```

---

## ▶️ Como Rodar o Projeto

```bash
npm run init-db
node app.js
```

Acesse: [http://localhost:3000](http://localhost:3000)

---

## ✅ Resultado Esperado

- Cadastro de cursos via formulário.
- Associação de curso ao cadastrar aluno.
- Listagem de alunos com nome do curso associado.
- Filtro de alunos por curso via rota `/alunos/curso/:id`.
