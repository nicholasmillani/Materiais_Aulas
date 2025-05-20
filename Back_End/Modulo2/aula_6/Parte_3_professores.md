
# 📘 CRUD de Professores (MVC com HTML básico e EJS)

Este roteiro descreve como implementar o CRUD completo de **professores**, utilizando a arquitetura MVC com Node.js e Express, e interface HTML com EJS.

---

## 🎯 Objetivo

- Permitir o cadastro, edição e remoção de professores
- Apresentar e manipular os dados na view `views/professores/index.ejs`
- Usar a arquitetura MVC para separar responsabilidades

---

## 🗂️ Estrutura Utilizada

```
📁 models/professor.js
📁 controllers/professorController.js
📁 routes/professores.js
📄 views/professores/index.ejs
📄 app.js
```

---

## 1️⃣ View – `views/professores/index.ejs`

```ejs
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Gestão de Professores</title>
</head>
<body>
  <h1>Professores Cadastrados</h1>

  <table border="1">
    <thead>
      <tr>
        <th>Nome</th>
        <th>Email</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody>
      <% professores.forEach(prof => { %>
        <tr>
          <td><%= prof.nome %></td>
          <td><%= prof.email %></td>
          <td>
            <form action="/professores/delete/<%= prof.id %>" method="POST" style="display:inline;">
              <button type="submit">Deletar</button>
            </form>
            <form action="/professores/edit/<%= prof.id %>" method="POST" style="display:inline;">
              <input name="nome" placeholder="Novo nome" required />
              <input name="email" placeholder="Novo email" required />
              <button type="submit">Editar</button>
            </form>
          </td>
        </tr>
      <% }); %>
    </tbody>
  </table>

  <h2>Cadastrar Novo Professor</h2>
  <form action="/professores" method="POST">
    <input name="nome" placeholder="Nome" required />
    <input name="email" placeholder="Email" required />
    <button type="submit">Cadastrar</button>
  </form>
</body>
</html>
```

---

## 2️⃣ Controller – `controllers/professorController.js`

```js
const Professor = require('../models/professor');

exports.index = async (req, res) => {
  const professores = await Professor.findAll(); 
  res.render('professores/index', { professores });
};

exports.create = async (req, res) => {
  const { nome, email } = req.body;
  await Professor.create(nome, email);
  res.redirect('/professores');
};

exports.update = async (req, res) => {
  const { id } = req.params;
  const { nome, email } = req.body;
  await Professor.update(id, nome, email);
  res.redirect('/professores');
};

exports.delete = async (req, res) => {
  const { id } = req.params;
  await Professor.delete(id);
  res.redirect('/professores');
};
```

---

## 3️⃣ Model – `models/professor.js`

```js
const db = require('../config/db');

module.exports = {
  async findAll() {
    const result = await db.query('SELECT * FROM professor ORDER BY nome ASC');
    return result.rows;
  },

  async create(nome, email) {
    const result = await db.query(
      'INSERT INTO professor (nome, email) VALUES ($1, $2) RETURNING *',
      [nome, email]
    );
    return result.rows[0];
  },

  async update(id, nome, email) {
    const result = await db.query(
      'UPDATE professor SET nome = $1, email = $2 WHERE id = $3 RETURNING *',
      [nome, email, id]
    );
    return result.rows[0];
  },

  async delete(id) {
    await db.query('DELETE FROM professor WHERE id = $1', [id]);
  }
};
```

---

## 4️⃣ Rotas – `routes/professores.js`

```js
const express = require('express');
const router = express.Router();
const controller = require('../controllers/professorController');

router.get('/', controller.index);
router.post('/', controller.create);
router.post('/edit/:id', controller.update);
router.post('/delete/:id', controller.delete);

module.exports = router;
```

---

## 5️⃣ Registro no `app.js`

📄 `app.js`

```js
const professoresRoutes = require('./routes/professores');
app.use('/professores', professoresRoutes);
```

---

## ✅ Testes Esperados

| Caminho                             | Ação                                 |
|-------------------------------------|--------------------------------------|
| `GET /professores`                  | Exibe lista de professores           |
| `POST /professores`                 | Cadastra novo professor              |
| `POST /professores/edit/:id`        | Atualiza dados do professor          |
| `POST /professores/delete/:id`      | Remove professor do banco            |

---

## ✅ Conclusão

Este módulo fecha o ciclo de CRUDs no projeto, mantendo a padronização e clareza da arquitetura MVC, com rotas bem organizadas e uma view funcional e familiar aos alunos.

---

## 🧠 Perguntas para medir aprendizado (responder neste README)

1. **Explique com suas palavras o papel de cada camada da arquitetura MVC usada neste projeto.**  
   *Como o Model, o Controller e a View interagem entre si?*

2. **Como ocorre o envio e o recebimento de dados no formato JSON neste projeto?**  
   *Cite uma rota que responde em JSON e explique seu funcionamento.*

3. **Qual a importância de usar HTML básico com formulários e tabelas para organizar e manipular dados no navegador?**  
   *Por que esse tipo de estrutura ainda é útil em projetos back-end com Node.js?*
