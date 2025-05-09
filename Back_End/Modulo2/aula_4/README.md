
# 🧠 Parte 3 - Relacionamento entre Tabelas, Chaves Estrangeiras e JOINs no CRUD

## 🎯 Objetivo

Nesta etapa, o projeto evolui com a criação de relacionamentos entre tabelas por meio de **chaves estrangeiras**, aplicação de **JOINs SQL** e **cadastro dinâmico de dados relacionados**. O foco é consolidar os seguintes conceitos:

- Relacionamento 1:N entre tabelas
- Criação e uso de chave estrangeira
- Consultas com `JOIN` (LEFT JOIN)
- Integração de dados relacionados na interface e backend

---
📌  Pré-requisito: Parte 2 da aula anterior funcionando corretamente.

---

## 1. 🗃️ Criação da Tabela `curso` e relacionamento com `aluno`

Atualizamos o arquivo `init.sql` com:

```sql
CREATE TABLE IF NOT EXISTS curso (
  id SERIAL PRIMARY KEY,
  nome TEXT NOT NULL
);

ALTER TABLE aluno
ADD COLUMN curso_id INTEGER,
ADD CONSTRAINT fk_curso FOREIGN KEY (curso_id) REFERENCES curso(id) ON DELETE SET NULL;
```

🔧 **Explicação**:
- Criamos a tabela `curso` para representar os cursos disponíveis.
- A coluna `curso_id` em `aluno` cria o vínculo entre as tabelas (1 curso → muitos alunos).
- O `ON DELETE SET NULL` garante que, se o curso for excluído, o aluno permanece no banco com valor nulo no campo `curso_id`.

---

## 2. ✅ Novos Arquivos e Atualizações na Estrutura do Projeto

Adições à estrutura do projeto:

```
📂models
┣ 📜 curso.js       → NOVO: model para manipular cursos
📂controllers
┣ 📜 cursoController.js → NOVO: lógica de criação de cursos
📂routes
┣ 📜 cursos.js      → NOVO: rota para cadastro de cursos
```

📌 **Comentário**: Essa modularização segue o padrão MVC, mantendo a organização clara com base em responsabilidades. Agora temos um módulo completo também para `curso`.

---

## 3. 📁 Atualização no model `aluno.js`

```js
async create(data) {
  const query = 'INSERT INTO aluno (nome, email, curso_id) VALUES ($1, $2, $3)';
  const values = [data.nome, data.email, data.curso_id || null];
  return db.query(query, values);
}
```

📌 **Comentário**: adicionamos `curso_id` ao insert de aluno.

```js
async findAllComCurso() {
  const query = \`
    SELECT aluno.id, aluno.nome, aluno.email, curso.nome AS curso
    FROM aluno
    LEFT JOIN curso ON aluno.curso_id = curso.id
    ORDER BY aluno.id ASC
  \`;
  const result = await db.query(query);
  return result.rows;
}
```

📌 **Comentário**: novo método com `LEFT JOIN` para exibir nome do curso junto ao aluno, mesmo que não esteja vinculado.

```js
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

📌 **Comentário**: novo método que permite listar alunos filtrados por curso específico.

---

## 4. 📁 Novo model `curso.js`

```js
async findAll() {
  const result = await db.query('SELECT * FROM curso ORDER BY nome ASC');
  return result.rows;
}

async create(nome) {
  const query = 'INSERT INTO curso (nome) VALUES ($1) RETURNING *';
  const result = await db.query(query, [nome]);
  return result.rows[0];
}
```

📌 **Comentário**: implementa listagem e criação de cursos.

---

## 5. 📁 Atualização no controller `alunoController.js`

```js
const cursos = await Curso.findAll();
```

📌 **Comentário**: adicionamos a busca de todos os cursos para popular o `select` do formulário de aluno.

```js
exports.byCurso = async (req, res) => {
  const { curso_id } = req.params;
  const alunos = await Aluno.findByCurso(curso_id);
  res.json(alunos);
};
```

📌 **Comentário**: nova rota para retornar apenas os alunos de um curso específico (ex: API REST filtrável).

---

## 6. 📁 Novo controller `cursoController.js`

```js
exports.create = async (req, res) => {
  const { nome } = req.body;
  const curso = await Curso.create(nome);
  res.redirect('/alunos');
};
```

📌 **Comentário**: responsável por cadastrar cursos a partir da interface HTML.

---

## 7. 📁 Novas rotas

**alunos.js**
```js
router.get('/curso/:curso_id', controller.byCurso);
```

📌 **Comentário**: rota adicional para listar alunos de um curso específico (útil para AJAX futuramente).

**cursos.js**
```js
router.post('/', controller.create);
```

📌 **Comentário**: rota para criar cursos a partir do formulário.

---

## 8. 📄 Atualização da view `views/alunos/index.ejs`

Adicionamos no formulário de aluno:

```html
<select name="curso_id">
  <option value="">Selecione um curso</option>
  <% cursos.forEach(curso => { %>
    <option value="<%= curso.id %>"><%= curso.nome %></option>
  <% }) %>
</select>
```

📌 **Comentário**: nova interface para associar um curso ao aluno na criação ou edição.

Também criamos um novo formulário para adicionar cursos:

```html
<h2>Cadastrar novo curso</h2>
<form action="/cursos" method="POST">
  <input name="nome" placeholder="Nome do curso" required>
  <button type="submit">Adicionar Curso</button>
</form>
```

📌 **Comentário**: isso permite ao usuário criar cursos diretamente pela interface, facilitando o preenchimento dinâmico da lista no `select`.

---

## ▶️ Como Rodar o Projeto Localmente

1. Rode o script para recriar o banco:
```bash
npm run init-db
```

2. Inicie o servidor:
```bash
node app.js
```

3. Acesse:
```
http://localhost:3000
```
