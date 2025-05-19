
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

> 💡 **Dica pedagógica:** Se você ainda não criou os arquivos `aluno.js`, `alunoController.js`, e `alunos.js`, crie agora dentro de suas respectivas pastas. Nomeie sempre com letras minúsculas e evite espaços.

---

## 🧭 Como o fluxo funciona?

1. O usuário faz uma requisição para uma rota, como `/alunos/api`.
2. A rota encaminha a requisição para o `controller`.
3. O controller processa os dados e se comunica com o `model`, que acessa o banco de dados.
4. O controller então retorna uma resposta ao navegador ou API client (Postman, Insomnia).

---

## 🧱 Etapa 1 — Criar o Controller com Endpoints JSON

```javascript
// controllers/alunoController.js

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

```javascript
// routes/alunos.js

const express = require('express');
const router = express.Router();
const controller = require('../controllers/alunoController');

router.get('/api', controller.apiList);
router.get('/api/:id', controller.apiGetById);
router.post('/api', controller.apiCreate);

module.exports = router;
```

```javascript
// app.js

const alunosRoutes = require('./routes/alunos');
app.use('/alunos', alunosRoutes);
```

---

## 🧠 Etapa 3 — Criar os Métodos no Model

```javascript
// models/aluno.js

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

## 🧪 Etapa 4 — Testando com Postman

- `GET /alunos/api` → Lista alunos
- `GET /alunos/api/:id` → Busca aluno por ID
- `POST /alunos/api` com body:
```json
{
  "nome": "Lucas Silva",
  "idade": 20
}
```

---

## ✏️ Miniatividade

Crie um endpoint `GET` para buscar alunos com idade > 18.

- Rota: `/alunos/api/maiores`
- Crie um método `findByMaiorIdade` no `model`.

---

## 📄 Como documentar endpoints manualmente

- **Rota:** `/alunos/api`
- **Método:** `GET`
- **Entrada:** nenhuma
- **Saída:** lista de alunos
- **Exemplo:**
```json
[
  { "id": 1, "nome": "Lucas", "idade": 20 }
]
```

---

## 🖥️ Etapa 5 — HTML + fetch

```html
<script>
  async function carregarAlunos() {
    const res = await fetch('/alunos/api');
    const dados = await res.json();
    document.getElementById('saida').textContent = JSON.stringify(dados, null, 2);
  }

  async function enviarAluno() {
    await fetch('/alunos/api', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ nome: 'João', idade: 22 })
    });
  }
</script>
<body onload="carregarAlunos()">
  <button onclick="enviarAluno()">Adicionar João</button>
  <h2>Lista de Alunos</h2>
  <pre id="saida"></pre>
</body>
```

---

## 🔁 Etapa 6 — Repita para cursos e professores

Crie os arquivos:

- `models/curso.js`, `controllers/cursoController.js`, `routes/cursos.js`
- `models/professor.js`, `controllers/professorController.js`, `routes/professores.js`

Use o mesmo padrão de `aluno`.

---

## 📘 Responda no README

1. O que você implementou?
2. Como funcionam os endpoints que criou?
3. O que acontece em cada camada (model, controller, rota)?
4. O que deu errado e como resolveu?
5. Como testou os dados (Postman, navegador, fetch)?

---

## ✅ Conclusão

- Aplicação prática da arquitetura MVC;
- Endpoints RESTful com JSON;
- Integração com banco PostgreSQL;
- Visualização via front-end básico;
- Base sólida para autenticação e API completas.
