
# 📘 Aula 7 – Requisições Assíncronas, Controllers, Protocolos de Rede e Testes com Postman

## 🎯 Objetivo Geral
Desenvolver habilidades práticas para compreender e testar APIs web utilizando Express.js com MVC, simulando requisições reais com o Postman. Essa aula integra os conceitos de redes de computadores (protocolo IP, HTTP), formulários HTML e rotas, e permite que os alunos interajam diretamente com os controllers `aluno`, `curso` e `professor`.

---

## 📌 Conteúdo Programático

### 1. Fundamentos de Redes de Computadores (20 min)
- Diferença entre cliente e servidor.
- O que é IP (`127.0.0.1` = `localhost`) e porta (`3000`).
- Protocolo HTTP e métodos (GET, POST, PUT, DELETE).
- Status codes: `200`, `201`, `404`, `500`.

---

### 2. Entendendo o projeto MVC (15 min)
- **Model:** Lida com o banco de dados.
- **View:** Formulários HTML (EJS).
- **Controller:** Lógica de negócio, onde ocorrem as ações de salvar, buscar, atualizar e deletar.

---

### 3. Explorando controllers existentes (20 min)

#### Aluno (`controllers/alunoController.js`)
```js
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
```

#### Curso (`controllers/cursoController.js`)
```js
exports.create = async (req, res) => {
  const { nome } = req.body;
  await Curso.create(nome);
  res.redirect('/alunos');
};
```

#### Professor (`controllers/professorController.js`)
```js
exports.create = async (req, res) => {
  const { nome, email } = req.body;
  await Professor.create(nome, email);
  res.redirect('/professores');
};
```

---

### 4. Introdução ao Postman (10 min)

#### O que é o Postman?
- Ferramenta de testes de APIs.
- Permite enviar requisições HTTP como GET, POST, PUT, DELETE.
- Permite visualizar respostas, status, headers e corpo da resposta.

#### Conceitos básicos:
- **Method**: tipo de requisição.
- **URL**: endereço da rota.
- **Body**: conteúdo da requisição.
- **Params**: valores passados na URL.
- **Headers**: informações adicionais (ex.: `Content-Type`).

---

### 5. Testes práticos com Postman (30 min)

#### Criar um aluno
- Método: POST
- URL: `http://localhost:3000/alunos`
- Headers: `Content-Type: application/json`
- Body (raw):
```json
{
  "nome": "Fernanda Lima",
  "email": "fernanda@example.com"
}
```

#### Listar alunos
- Método: GET
- URL: `http://localhost:3000/alunos`

#### Atualizar aluno
- Método: PUT
- URL: `http://localhost:3000/alunos/1`
- Body:
```json
{
  "nome": "Fernanda L. Souza"
}
```

#### Deletar aluno
- Método: DELETE
- URL: `http://localhost:3000/alunos/1`

> 🧪 Faça o mesmo com `/professores` e `/cursos`, adaptando os dados!

---

### 6. Integração com formulários HTML (15 min)

```html
<form action="/alunos" method="POST">
  <input type="text" name="nome" placeholder="Nome" required />
  <input type="email" name="email" placeholder="Email" required />
  <button type="submit">Cadastrar</button>
</form>
```

- Esse formulário envia dados via `POST` para o controller de alunos.

---

### 7. Atividade prática orientada (20 min)

**Objetivo:** Cada grupo deve criar, listar, editar e deletar dados via Postman para uma entidade (`alunos`, `professores` ou `cursos`).

1. Crie 2 entradas (POST).
2. Liste os dados (GET).
3. Atualize 1 item (PUT).
4. Apague 1 item (DELETE).
5. Tire print das requisições e organize num PDF para entrega.

---

### 8. Encerramento e Perguntas (10 min)
- Revisar diferenças entre métodos HTTP.
- Dúvidas sobre o Postman.
- Dicas para debugar: usar o console do Node.js, `console.log(req.body)`, verificar erro 404 e 500.

---

## 👨‍🏫 Professor

**Cristiano da Silva Benites**  
Especialista em Back-end, MVC, Postman, Redes e Educação Técnica

---

🎓 **Dica para o GitHub:** este README pode ser usado diretamente no repositório da aula para que os alunos o sigam passo a passo.

