
# 📘 API MVC com Postman – Criação de Registros

Este repositório demonstra como interagir com uma API construída no padrão MVC utilizando o Postman. A proposta é realizar **operações de criação** para os recursos `curso`, `professor` e `aluno`, em um banco de dados inicialmente vazio.

---

## 🎯 Objetivos

- Compreender o envio de dados via JSON para a API.
- Utilizar requisições HTTP do tipo `POST`.
- Registrar dados reais no banco de dados utilizando Postman.
- Refletir sobre o fluxo de dados entre cliente e servidor no modelo MVC.

---

## 🧰 Pré-requisitos

- Projeto em Node.js já rodando com `node app.js`.
- Middleware JSON habilitado no `app.js`:
  ```js
  app.use(express.json());
  ```
- Banco de dados configurado e vazio.
- Postman instalado.

---

## 🚀 Inicializando o servidor

1. Acesse o terminal e navegue até a pasta do projeto.
2. Execute:
   ```bash
   node app.js
   ```
3. Confirme que o servidor está ativo:
   ```
   Servidor rodando em http://localhost:3000
   ```

---

## 🛠️ Configuração no Postman

- Selecione o método `POST`.
- Em **Headers**, adicione:
  ```
  Content-Type: application/json
  Accept: application/json
  ```
- Em **Body > raw**, selecione o tipo `JSON`.

---

## 📚 Criar Curso (POST `/cursos`)

**URL:** `http://localhost:3000/cursos`

```json
{
  "nome": "Engenharia da Computação",
  "descricao": "Curso criado via Postman"
}
```

---

## 👨‍🏫 Criar Professor (POST `/professores`)

**URL:** `http://localhost:3000/professores`

```json
{
  "nome": "Dra. Fernanda Lima",
  "email": "fernanda@faculdade.com"
}
```

---

## 👨‍🎓 Criar Aluno (POST `/alunos`)

**URL:** `http://localhost:3000/alunos`

> Consulte previamente os cursos com `GET /cursos` para obter um `curso_id` válido.

### Com curso:
```json
{
  "nome": "Carlos Souza",
  "email": "carlos@email.com",
  "curso_id": 1
}
```

### Sem curso:
```json
{
  "nome": "Aluno sem curso",
  "email": "aluno@email.com"
}
```

---

## ✅ Conclusão

Após seguir os passos acima, você será capaz de:

- Registrar cursos, professores e alunos no banco via Postman.
- Utilizar corretamente o método HTTP `POST` com JSON.
- Compreender o comportamento de APIs RESTful integradas ao padrão MVC.

---

> ⚠️ Este guia cobre apenas a criação de dados. As operações de leitura, atualização e exclusão serão abordadas em etapas futuras.
