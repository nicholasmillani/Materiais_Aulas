
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

# Parte 4B — Entendendo a Estrutura MVC: Models e Controllers na Prática

## 🎯 Objetivo

Compreender a divisão de responsabilidades entre `models` e `controllers` usando o padrão MVC, destacando seu papel na organização do código e na interação entre banco de dados e interface.

---

## 1. 🧠 Conceito Introdutório

**MVC** é uma arquitetura que separa a lógica da aplicação em três camadas:

- **Model** → Regras de negócio e acesso aos dados.
- **View** → Interface apresentada ao usuário.
- **Controller** → Lógica de controle que conecta model e view.

### 🧩 Analogia:

Imagine um restaurante:

- O **model** é o cozinheiro (prepara os dados).
- O **controller** é o garçom (leva o pedido e traz a comida).
- A **view** é o cardápio e o prato servido ao cliente.

---

## 2. 🧱 Estrutura Padrão do Projeto

```
project/
│
├── models/
│   ├── aluno.js         ← lógica de acesso ao banco para aluno
│   └── curso.js         ← lógica de acesso ao banco para curso
│
├── controllers/
│   ├── alunoController.js  ← coordena os dados dos alunos
│   └── cursoController.js  ← coordena os dados dos cursos
│
├── views/
│   └── alunos/index.ejs    ← interface com formulário e listagem
│
└── routes/
    ├── alunos.js           ← define as rotas dos alunos
    └── cursos.js           ← define as rotas dos cursos
```

---

## 3. 🧩 Explicação Visual do Fluxo MVC

**Exemplo: Cadastro de um curso pelo formulário**

```
Usuário envia formulário (view)
        ↓
cursoController.create() (controller)
        ↓
Curso.create(nome) (model)
        ↓
INSERT INTO curso (nome) VALUES (...) (SQL)
        ↓
Redireciona para /alunos com dados atualizados
```

🧠 O controller é o cérebro que decide qual função do model será usada com base no que o usuário faz na view.

---

## 4. 🛠 Atividade Prática Guiada

### Desafio 1 — Entendendo os papéis

Em duplas, abra `alunoController.js` e destaque com comentários:

- Onde o controller está chamando o model.
- Quais dados são enviados para a view.

Depois, faça o mesmo com `cursoController.js`.

### Desafio 2 — Novo endpoint

Crie um novo método no controller para deletar um curso:

- No model: `async delete(id)`
- No controller: `exports.delete = async (req, res) => { ... }`
- Na rota: `router.post('/delete/:id', controller.delete);`
- Na view: botão de deletar ao lado de cada curso.

---

## 5. 🧭 Roteiro de Reforço Didático

- 🔹 O **model** nunca tem `res.send()` ou `res.render()` → ele só se comunica com o banco.
- 🔹 O **controller** nunca tem SQL direto → ele só chama funções do model e responde à view.
- 🔹 A **view** nunca tem lógica de acesso ao banco → ela só exibe os dados recebidos.

---

## ✅ Resultado Esperado

- Aluno entende o papel de cada camada na prática.
- Aluno consegue criar novos métodos nos `models` e `controllers`.
- Aluno compreende o fluxo entre formulário, controller e banco de dados.
