
# Parte 4 — Roteiro de Atividade: Models e Controllers com MVC

## 🎯 O que você vai aprender

Consolidar a compreensão prática e conceitual dos alunos sobre a separação de responsabilidades entre `models`, `controllers` e `views` no padrão MVC. Os alunos irão praticar a atualização e exclusão de cursos usando formulários, e refletir sobre a arquitetura do projeto.

---

## 📘 Parte 4A — Mão na Massa: Melhorando nosso sistema com MVC

### 💡 Cenário

Imagine que você foi chamado para melhorar um sistema de cadastro de alunos e cursos. Seu desafio nesta atividade será:

1. Permitir que cursos já cadastrados possam ser **editados**.
2. Permitir que cursos possam ser **excluídos**, respeitando o relacionamento com os alunos.

---

### 🛠 Tarefa 1 — Permitir que o nome de um curso seja editado

#### 1.1 — No `models/curso.js`

```js
// Atualiza o nome de um curso com base no ID fornecido
async update(id, nome) {
  const query = 'UPDATE curso SET nome = $1 WHERE id = $2 RETURNING *';
  const result = await db.query(query, [nome, id]);
  return result.rows[0];
}
```

---

#### 1.2 — No `controllers/cursoController.js`

```js
// Controlador responsável por atualizar o curso chamando o model
exports.update = async (req, res) => {
  const { id } = req.params;
  const { nome } = req.body;
  await Curso.update(id, nome);
  res.redirect('/alunos');
};
```

---

#### 1.3 — No `routes/cursos.js`

```js
// Rota que envia dados do formulário de edição para o controller
router.post('/edit/:id', controller.update);
```

---

#### 1.4 — Em `views/alunos/index.ejs`

```ejs
<!-- Lista de cursos com formulário de edição embutido -->
<% cursos.forEach(curso => { %>
  <form action="/cursos/edit/<%= curso.id %>" method="POST" style="display: flex; gap: 10px; margin-bottom: 5px;">
    <input name="nome" value="<%= curso.nome %>" required>
    <button type="submit">✏️ Editar</button>
  </form>
<% }) %>
```

---

### 🛠 Tarefa 2 — Permitir que um curso seja excluído

#### 2.1 — No `models/curso.js`

```js
// Deleta um curso por ID — ON DELETE SET NULL evita erro em relacionamentos
async delete(id) {
  await db.query('DELETE FROM curso WHERE id = $1', [id]);
}
```

---

#### 2.2 — No `controllers/cursoController.js`

```js
// Controlador responsável por deletar o curso
exports.delete = async (req, res) => {
  const { id } = req.params;
  await Curso.delete(id);
  res.redirect('/alunos');
};
```

---

#### 2.3 — No `routes/cursos.js`

```js
// Rota para deletar curso via POST
router.post('/delete/:id', controller.delete);
```

---

#### 2.4 — Em `views/alunos/index.ejs`

```ejs
<!-- Formulário para deletar curso -->
<form action="/cursos/delete/<%= curso.id %>" method="POST" style="display:inline;">
  <button type="submit" onclick="return confirm('Tem certeza que deseja excluir este curso?')">🗑️ Excluir</button>
</form>
```

---

### 📌 O que esperamos que você consiga fazer

- Cursos podem ser editados e excluídos pela interface.
- A estrutura MVC é respeitada com clareza.
- O aluno compreende o fluxo view → controller → model → banco.

---

## 📗 Parte 4B — Entenda a Teoria por Trás: O que é MVC?

### 🎯 Objetivo

Refletir conceitualmente sobre a separação de responsabilidades no padrão MVC com exemplos práticos e analogias.

---

### 1. 🧠 Mas afinal, o que é MVC?

**MVC** é um padrão que organiza aplicações em três camadas:

- **Model** → cuida dos dados e regras de negócio.
- **View** → apresenta a interface ao usuário.
- **Controller** → recebe comandos do usuário, manipula os models e atualiza a view.

---

### 🧩 Analogia Prática

Imagine um restaurante:

- **Model** → é o cozinheiro: acessa dados, prepara o prato.
- **Controller** → é o garçom: pega o pedido e entrega ao cliente.
- **View** → é o cardápio e a comida no prato: o que o usuário vê e interage.

---

### 2. 🧱 Como organizamos o projeto

```
project/
├── models/
│   ├── aluno.js           # Lógica de acesso ao banco de alunos
│   └── curso.js           # Lógica de acesso ao banco de cursos
├── controllers/
│   ├── alunoController.js # Controla ações relacionadas aos alunos
│   └── cursoController.js # Controla ações relacionadas aos cursos
├── views/
│   └── alunos/index.ejs   # Interface com formulários
└── routes/
    ├── alunos.js          # Rotas dos alunos
    └── cursos.js          # Rotas dos cursos
```

---

### 3. 🔄 Como as partes conversam entre si

**Exemplo: Cadastro de Curso**

```
Usuário preenche formulário (view)
        ↓
Controller recebe (cursoController.create)
        ↓
Model insere no banco (Curso.create)
        ↓
Banco de dados armazena e devolve resultado
        ↓
Controller redireciona para página de listagem
```

---

### 4. ✍️ Reforçando com prática

#### Desafio 1 — Análise de código

- Abra `alunoController.js` e `cursoController.js`
- Comente no código:
  - Onde o controller chama o model?
  - O que é enviado à view?

#### Desafio 2 — Criar novo endpoint

- Crie um endpoint que retorna todos os cursos em formato JSON.
- Crie a rota GET `/cursos/json`
- No controller, utilize `Curso.findAll()` e `res.json(...)`.

---

### 5. 📌 Dicas para você nunca esquecer

- 🔹 O **model** só se comunica com o banco, não manipula views.
- 🔹 O **controller** conecta a view ao model, mas não tem SQL direto.
- 🔹 A **view** apenas mostra dados, sem lógica de negócio.

---

### 📌 O que esperamos que você consiga fazer

- Compreensão teórica e prática clara do padrão MVC.
- Capacidade de modificar e estender controllers e models com autonomia.
- Clareza na divisão de responsabilidades entre camadas.

---

