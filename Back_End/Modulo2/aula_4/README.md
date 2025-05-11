
# Parte 3 — Relacionamento entre Tabelas, Chaves Estrangeiras e JOINs no CRUD

## 🎯 Objetivo

- Criar um relacionamento entre alunos e cursos (1:N).
- Aplicar LEFT JOIN para exibir o nome do curso junto com os dados do aluno.
- Permitir cadastro e listagem de cursos.
- Permitir associar um curso ao aluno via formulário.

---

## 1. 🧱 Atualize o `init.sql`

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

## 3. 📄 Atualize `models/aluno.js`

```js
async create(data) {
  const query = 'INSERT INTO aluno (nome, email, curso_id) VALUES ($1, $2, $3)';
  const values = [data.nome, data.email, data.curso_id || null];
  return db.query(query, values);
},

async findAllComCurso() {
  const query = \`
    SELECT aluno.id, aluno.nome, aluno.email, curso.nome AS curso
    FROM aluno
    LEFT JOIN curso ON aluno.curso_id = curso.id
    ORDER BY aluno.id ASC
  \`;
  const result = await db.query(query);
  return result.rows;
},

async findByCurso(curso_id) {
  const query = \`
    SELECT aluno.id, aluno.nome, aluno.email
    FROM aluno
    WHERE curso_id = $1
    ORDER BY nome ASC
  \`;
  const result = await db.query(query, [curso_id]);
  return result.rows;
}
```

---

## 4. 📄 Crie `controllers/cursoController.js`

```js
const Curso = require('../models/curso');

exports.create = async (req, res) => {
  const { nome } = req.body;
  await Curso.create(nome);
  res.redirect('/alunos');
};
```

---

## 5. 📄 Atualize `controllers/alunoController.js`

```js
const Curso = require('../models/curso');

exports.index = async (req, res) => {
  const alunos = await Aluno.findAllComCurso();
  const cursos = await Curso.findAll();
  res.render('alunos/index', { alunos, cursos });
};

exports.byCurso = async (req, res) => {
  const { curso_id } = req.params;
  const alunos = await Aluno.findByCurso(curso_id);
  res.json(alunos);
};
```

---

## 6. 📄 Crie `routes/cursos.js`

```js
const express = require('express');
const router = express.Router();
const controller = require('../controllers/cursoController');

router.post('/', controller.create);

module.exports = router;
```

---

## 7. 📄 Atualize `routes/alunos.js`

```js
router.get('/curso/:curso_id', controller.byCurso);
```

---

## 8. 📄 Atualize `app.js`

```js
const cursosRoutes = require('./routes/cursos');
app.use('/cursos', cursosRoutes);
```

---

## 9. 🖼 Atualize `views/alunos/index.ejs`

### Formulário de seleção de curso:

```html
<select name="curso_id">
  <option value="">Selecione um curso</option>
  <% cursos.forEach(curso => { %>
    <option value="<%= curso.id %>"><%= curso.nome %></option>
  <% }) %>
</select>
```

### Formulário para criar novo curso:

```html
<h2>Cadastrar novo curso</h2>
<form action="/cursos" method="POST">
  <input name="nome" placeholder="Nome do curso" required>
  <button type="submit">Adicionar Curso</button>
</form>
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
