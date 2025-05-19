
# 🚀 Projeto Back-end II — Endpoints com JSON via Navegador

Este repositório contém um projeto baseado na arquitetura MVC com Node.js e PostgreSQL. Nesta etapa, o foco é **criar endpoints RESTful** que utilizam **JSON para entrada e saída de dados**, **sem o uso de Postman**, apenas com **HTML + JavaScript no navegador**.

---

## 🎯 Objetivos da Aula

- Criar endpoints `GET` e `POST` para os recursos `aluno`, `curso` e `professor`.
- Testar tudo usando apenas o navegador (sem Postman).
- Consolidar o uso de JSON e da arquitetura MVC.
- Criar uma pequena interface HTML para visualizar e enviar dados.

---

## 📁 Estrutura Esperada

Certifique-se de ter a seguinte estrutura:

```
projeto/
├── models/
├── controllers/
├── routes/
├── views/            # Aqui ficará nosso HTML
├── config/
└── app.js
```

> 💡 Dica: Se os arquivos `aluno.js`, `alunoController.js`, e `alunos.js` ainda não existirem, crie-os agora. Use letras minúsculas e nomes coerentes.

---

## 🧭 Como funciona o fluxo MVC

1. O usuário interage com a **interface HTML** no navegador.
2. O navegador envia uma requisição para uma **rota (route)**.
3. A rota direciona o pedido para o **controller**.
4. O controller chama o **model**, que acessa o banco de dados.
5. A resposta (JSON) volta do model para o controller, e então ao navegador.

---

## 🧱 Etapa 1 — Criando o Controller

```javascript
// controllers/alunoController.js

const Aluno = require('../models/aluno');

// Retorna todos os alunos em JSON
exports.apiList = async (req, res) => {
  const alunos = await Aluno.findAll(); // Busca no banco
  res.json(alunos); // Retorna como JSON
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

// Cria novo aluno a partir dos dados enviados pelo navegador
exports.apiCreate = async (req, res) => {
  const { nome, idade } = req.body;
  const novo = await Aluno.create(nome, idade); // Insere no banco
  res.status(201).json(novo); // Retorna o aluno criado
};
```

---

## 📌 Etapa 2 — Criar as Rotas

```javascript
// routes/alunos.js

const express = require('express');
const router = express.Router();
const controller = require('../controllers/alunoController');

// Lista todos os alunos
router.get('/api', controller.apiList);

// Busca um aluno específico por ID
router.get('/api/:id', controller.apiGetById);

// Cadastra um novo aluno
router.post('/api', controller.apiCreate);

module.exports = router;
```

```javascript
// app.js

const alunosRoutes = require('./routes/alunos');
app.use('/alunos', alunosRoutes);

// Para servir arquivos HTML da pasta views
app.use(express.static('views'));
```

---

## 🧠 Etapa 3 — Criar o Model

```javascript
// models/aluno.js

const db = require('../config/db');

module.exports = {
  // Retorna todos os alunos
  async findAll() {
    const result = await db.query('SELECT * FROM aluno ORDER BY nome ASC');
    return result.rows;
  },

  // Busca por ID
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

## 🖥️ Etapa 4 — HTML com `fetch()` no navegador

Crie um arquivo chamado `index.html` dentro da pasta `views/`.

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Cadastro de Alunos</title>
</head>
<body>
  <h2>📋 Lista de Alunos</h2>
  <button onclick="carregarAlunos()">Carregar Alunos</button>
  <pre id="saida"></pre>

  <h2>➕ Novo Aluno</h2>
  <input id="nome" placeholder="Nome" />
  <input id="idade" type="number" placeholder="Idade" />
  <button onclick="enviarAluno()">Cadastrar</button>
  <p id="resposta"></p>

  <script>
    async function carregarAlunos() {
      const res = await fetch('/alunos/api');
      const dados = await res.json();
      document.getElementById('saida').textContent = JSON.stringify(dados, null, 2);
    }

    async function enviarAluno() {
      const nome = document.getElementById('nome').value;
      const idade = parseInt(document.getElementById('idade').value);
      const resposta = await fetch('/alunos/api', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ nome, idade })
      });
      const json = await resposta.json();
      document.getElementById('resposta').textContent = 'Aluno cadastrado: ' + JSON.stringify(json);
      carregarAlunos(); // Atualiza lista
    }
  </script>
</body>
</html>
```

---

## 🔁 Etapa 5 — Repita a estrutura para cursos e professores

Crie os arquivos `models/curso.js`, `controllers/cursoController.js`, `routes/cursos.js`, e o HTML correspondente.

---

## ✏️ Desafio extra

Crie um endpoint que retorna apenas os alunos com idade maior que 18.  
- Rota: `/alunos/api/maiores`  
- No model, use SQL com `WHERE idade > 18`.

---

## 📘 Responda no README

1. O que você implementou?
2. Como os dados trafegam entre navegador, controller e model?
3. Qual a vantagem de usar apenas HTML + JS?
4. Houve algum erro? Como resolveu?

---

## ✅ Conclusão

- Você usou apenas o navegador para testar toda a API.
- Criou uma interface mínima funcional com HTML e `fetch`.
- Entendeu como funciona o ciclo completo do padrão MVC.
- Está pronto para implementar novos recursos com HTML e JSON.
