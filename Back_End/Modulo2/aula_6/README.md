# 🚀 Projeto Back-end II — Endpoints de Leitura e Escrita com JSON

Este repositório contém um projeto baseado na arquitetura MVC em Node.js com PostgreSQL. O foco desta etapa é a criação de **endpoints RESTful** que trabalham com **entrada e saída de dados no formato JSON**.

---

## 🎯 Objetivo da Aula

- Criar endpoints RESTful para os recursos `aluno`, `curso` e `professor`.
- Utilizar os métodos `GET` e `POST` para consumir e enviar dados em JSON.
- Documentar o funcionamento das rotas e validar as requisições com o Postman ou HTML básico.
- Consolidar o entendimento da arquitetura MVC: separação de responsabilidade entre models, controllers e routes.

---

## 📁 Estrutura Esperada

```
projeto/
├── models/
│   ├── aluno.js
│   ├── curso.js
│   └── professor.js
├── controllers/
│   ├── alunoController.js
│   ├── cursoController.js
│   └── professorController.js
├── routes/
│   ├── alunos.js
│   ├── cursos.js
│   └── professores.js
├── views/
├── config/
│   └── db.js
└── app.js
```

---

## 🧱 Etapa 1 — Criar o Controller com Endpoints JSON

### Exemplo: `controllers/alunoController.js`

```js
const Aluno = require('../models/aluno');

exports.apiList = async (req, res) => {
  const alunos = await Aluno.findAll();
  res.json(alunos);
};

exports.apiGetById = async (req, res) => {
  const { id } = req.params;
  const aluno = await Aluno.findById(id);
  if (!aluno) {
    return res.status(404).json({ error: 'Aluno não encontrado' });
  }
  res.json(aluno);
};

exports.apiCreate = async (req, res) => {
  const { nome, idade } = req.body;
  const novo = await Aluno.create(nome, idade);
  res.status(201).json(novo);
};
```

---

## 📌 Etapa 2 — Criar as Rotas

### Exemplo: `routes/alunos.js`

```js
const express = require('express');
const router = express.Router();
const controller = require('../controllers/alunoController');

router.get('/api', controller.apiList);
router.get('/api/:id', controller.apiGetById);
router.post('/api', controller.apiCreate);

module.exports = router;
```

E registre no `app.js`:

```js
const alunosRoutes = require('./routes/alunos');
app.use('/alunos', alunosRoutes);
```

---

## 🧠 Etapa 3 — Criar os Métodos no Model

### Exemplo: `models/aluno.js`

```js
const db = require('../config/db');

module.exports = {
  async findAll() {
    const result = await db.query('SELECT * FROM aluno ORDER BY nome ASC');
    return result.rows;
  },

  async findById(id) {
    const result = await db.query('SELECT * FROM aluno WHERE id = $1', [id]);
    return result.rows[0];
  },

  async create(nome, idade) {
    const result = await db.query(
      'INSERT INTO aluno (nome, idade) VALUES ($1, $2) RETURNING *',
      [nome, idade]
    );
    return result.rows[0];
  }
};
```

---

## 🧪 Etapa 4 — Testando no Postman

### Requisição GET:

- URL: `http://localhost:3000/alunos/api`

### Requisição GET por ID:

- URL: `http://localhost:3000/alunos/api/1`

### Requisição POST:

- URL: `http://localhost:3000/alunos/api`
- Body (JSON):
```json
{
  "nome": "Lucas Silva",
  "idade": 20
}
```

---

## 🔁 Repetir para `curso` e `professor`

Siga o mesmo padrão para os arquivos:

- `models/curso.js`, `controllers/cursoController.js`, `routes/cursos.js`
- `models/professor.js`, `controllers/professorController.js`, `routes/professores.js`

---

## 🖥️ Etapa 5 — Visualizando com HTML + fetch (extra)

### Exemplo simples em EJS:

```html
<script>
  async function carregarAlunos() {
    const res = await fetch('/alunos/api');
    const dados = await res.json();
    document.getElementById('saida').textContent = JSON.stringify(dados, null, 2);
  }
</script>

<body onload="carregarAlunos()">
  <h2>Lista de Alunos</h2>
  <pre id="saida"></pre>
</body>
```

---

## ✅ Conclusão

Este roteiro orienta a implementação completa de endpoints RESTful com JSON no projeto MVC, promovendo uma compreensão clara da estrutura back-end moderna e servindo de base para futuras integrações com front-ends modernos como React, Vue ou Svelte.
