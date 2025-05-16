# 🚀 Projeto Back-end II — Endpoints de Leitura e Escrita com JSON

Este repositório contém um projeto baseado na arquitetura MVC em Node.js com PostgreSQL. O foco desta etapa é a criação de **endpoints RESTful** que trabalham com **entrada e saída de dados no formato JSON**.

---

## 🎯 Objetivo da Aula

Nesta aula, vamos:

- Criar endpoints RESTful para os recursos `aluno`, `curso` e `professor`.
- Utilizar os métodos `GET` e `POST` para consumir e enviar dados no formato JSON.
- Aprender a documentar o funcionamento das rotas.
- Testar as rotas utilizando Postman ou HTML básico com `fetch`.
- Consolidar o entendimento da arquitetura MVC: separação entre model (dados), controller (lógica) e route (entrada).

---

## 📁 Estrutura Esperada

Antes de começarmos, verifique se sua estrutura de pastas está organizada da seguinte maneira:

```
projeto/
├── models/          # Lógica de acesso ao banco de dados
├── controllers/     # Lógica de controle (receber requisições e enviar respostas)
├── routes/          # Definição das rotas (URLs)
├── views/           # Interfaces HTML (opcional nesta aula)
├── config/          # Configurações de banco
└── app.js           # Arquivo principal do projeto
```

---

## 🧱 Etapa 1 — Criar o Controller com Endpoints JSON

Nesta etapa, vamos construir os métodos no controller responsáveis por responder às requisições via JSON.

### Exemplo: `controllers/alunoController.js`

```js
const Aluno = require('../models/aluno');

// Lista todos os alunos no formato JSON
exports.apiList = async (req, res) => {
  const alunos = await Aluno.findAll();
  res.json(alunos);
};

// Busca um aluno por ID
exports.apiGetById = async (req, res) => {
  const { id } = req.params;
  const aluno = await Aluno.findById(id);
  if (!aluno) {
    return res.status(404).json({ error: 'Aluno não encontrado' });
  }
  res.json(aluno);
};

// Cria um novo aluno a partir de dados JSON
exports.apiCreate = async (req, res) => {
  const { nome, idade } = req.body;
  const novo = await Aluno.create(nome, idade);
  res.status(201).json(novo);
};
```

---

## 📌 Etapa 2 — Criar as Rotas

As rotas são os caminhos que o navegador ou o Postman irão acessar para chamar os métodos do controller.

### Exemplo: `routes/alunos.js`

```js
const express = require('express');
const router = express.Router();
const controller = require('../controllers/alunoController');

// Rota para listar alunos
router.get('/api', controller.apiList);

// Rota para buscar aluno por ID
router.get('/api/:id', controller.apiGetById);

// Rota para criar novo aluno via JSON
router.post('/api', controller.apiCreate);

module.exports = router;
```

### E no `app.js`, registre a rota:

```js
const alunosRoutes = require('./routes/alunos');
app.use('/alunos', alunosRoutes);
```

---

## 🧠 Etapa 3 — Criar os Métodos no Model

O model representa o acesso direto ao banco de dados. Aqui criamos funções que serão chamadas pelo controller.

### Exemplo: `models/aluno.js`

```js
const db = require('../config/db');

module.exports = {
  // Busca todos os alunos no banco
  async findAll() {
    const result = await db.query('SELECT * FROM aluno ORDER BY nome ASC');
    return result.rows;
  },

  // Busca aluno específico pelo ID
  async findById(id) {
    const result = await db.query('SELECT * FROM aluno WHERE id = $1', [id]);
    return result.rows[0];
  },

  // Insere novo aluno
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

## 🧪 Etapa 4 — Testando com Postman

### Requisição GET (listar alunos)

- URL: `http://localhost:3000/alunos/api`

### Requisição GET por ID

- URL: `http://localhost:3000/alunos/api/1`

### Requisição POST (criar novo aluno)

- URL: `http://localhost:3000/alunos/api`
- Body (JSON):
```json
{
  "nome": "Lucas Silva",
  "idade": 20
}
```

Use o Postman ou Insomnia para testar suas rotas.

---

## 🔁 Etapa 5 — Repita a estrutura para cursos e professores

Crie os arquivos:

- `models/curso.js`, `controllers/cursoController.js`, `routes/cursos.js`
- `models/professor.js`, `controllers/professorController.js`, `routes/professores.js`

Use o mesmo padrão adotado para `aluno`.

---

## 🖥️ Etapa 6 — Visualizando com HTML + fetch (opcional)

Você pode criar uma página HTML simples (com EJS) que consome os dados da API via JavaScript:

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

```
📘 Como melhorar a reflexão no README dos alunos
Você pode pedir algo como:

“No final, edite seu README.md pessoal explicando:

O que você implementou

Como funcionam os endpoints que criou (GET, POST)

O que acontece em cada camada (model, controller, rota)

O que deu errado e como resolveu (se houve erro)

Como testou os dados (Postman, navegador, HTML com fetch)”
```

```

## ✅ Conclusão

Este roteiro guiou você pela construção de endpoints JSON com MVC em Node.js, proporcionando:

- Uma arquitetura modular e escalável;
- Integração com banco de dados PostgreSQL;
- Facilidade de consumo via front-end ou ferramentas de testes de API;
- Base sólida para autenticação e manipulação de dados via API RESTful.

