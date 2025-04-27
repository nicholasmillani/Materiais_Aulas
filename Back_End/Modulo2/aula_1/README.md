
# 🌐 Mini Site MVC com Node.js, Express e EJS — Com Navegação Funcional - Boilerplate.

Este projeto demonstra um site básico com **Node.js**, **Express** e **EJS**, utilizando a arquitetura **MVC (Model-View-Controller)**. Inclui **navegação funcional** entre as páginas **Início**, **Sobre** e **Contato**, com os códigos **totalmente comentados** para fins didáticos.

---

## 🧰 Etapa 1: Clonar o repositório e instalar as dependências

```bash
git clone https://github.com/afonsobrandaointeli/mvc-boilerplate.git

npm install
npm init -y
npm install express ejs
```

---

## 🎯 Objetivos

- Aplicar a arquitetura MVC.
- Separar responsabilidades: controle, visual e lógica.
- Reutilizar componentes com `partials` no EJS.
- Navegar entre páginas com HTML completo e estilizado.
- Utilizar CSS básico para layout visual agradável.

---

## 🗂️ Estrutura de Diretórios

```
MVC-basico/
├── app.js
├── package.json
├── controllers/
│   ├── homeController.js
│   ├── aboutController.js
│   └── contactController.js
├── routes/
│   └── index.js
├── views/
│   ├── partials/
│   │   ├── header.ejs
│   │   └── footer.ejs
│   ├── pages/
│   │   ├── home.ejs
│   │   ├── about.ejs
│   │   └── contact.ejs
├── public/
│   └── css/
│       └── style.css
```

---

## 🔧 `app.js` — Configuração do Servidor

```js
const express = require('express');
const app = express();
const path = require('path');
const routes = require('./routes');

// Configura o mecanismo de views para EJS
app.set('view engine', 'ejs');

// Define onde ficam as views
app.set('views', path.join(__dirname, 'views'));

// Define a pasta pública com CSS e outros arquivos estáticos
app.use(express.static(path.join(__dirname, 'public')));

// Usa o arquivo de rotas
app.use('/', routes);

// Inicia o servidor na porta 3000
app.listen(3000, () => {
  console.log('Servidor rodando em http://localhost:3000');
});
```

---

## 🌐 `routes/index.js` — Arquivo de Rotas

```js
const express = require('express');
const router = express.Router();
const home = require('../controllers/homeController');
const about = require('../controllers/aboutController');
const contact = require('../controllers/contactController');

// Rota principal
router.get('/', home.index);

// Rota da página "Sobre"
router.get('/sobre', about.index);

// Rota da página "Contato"
router.get('/contato', contact.index);

module.exports = router;
```

---

## 🎮 Controllers

### `controllers/homeController.js`

```js
// Controlador da rota /
exports.index = (req, res) => {
  res.render('pages/home', {
    titulo: 'Página Inicial',
    mensagem: 'Bem-vindo ao nosso mini site MVC!'
  });
};
```

### `controllers/aboutController.js`

```js
// Controlador da rota /sobre
exports.index = (req, res) => {
  res.render('pages/about', {
    titulo: 'Sobre',
    descricao: 'Esta é a página sobre do mini site MVC.'
  });
};
```

### `controllers/contactController.js`

```js
// Controlador da rota /contato
exports.index = (req, res) => {
  res.render('pages/contact', {
    titulo: 'Contato',
    mensagem: 'Preencha o formulário abaixo para entrar em contato.'
  });
};
```

---

## 🖼️ Views (com layout manual e includes corrigidos)

### `views/partials/header.ejs`

```html
<!-- Cabeçalho com menu de navegação -->
<header>
  <h2>Mini Site MVC</h2>
  <nav>
    <a href="/">🏠 Início</a> |
    <a href="/sobre">ℹ️ Sobre</a> |
    <a href="/contato">📬 Contato</a>
  </nav>
</header>
```

### `views/partials/footer.ejs`

```html
<!-- Rodapé fixo -->
<footer>
  <p>&copy; 2025 - Projeto Educacional com Node.js</p>
</footer>
```

### `views/pages/home.ejs`

```ejs
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title><%= titulo %></title>
  <link rel="stylesheet" href="/css/style.css">
</head>
<body>
  <%- include('../partials/header') %>

  <main class="container">
    <h1><%= mensagem %></h1>
    <p>Este site foi desenvolvido para explicar MVC na prática.</p>
  </main>

  <%- include('../partials/footer') %>
</body>
</html>
```

### `views/pages/about.ejs`

```ejs
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title><%= titulo %></title>
  <link rel="stylesheet" href="/css/style.css">
</head>
<body>
  <%- include('../partials/header') %>

  <main class="container">
    <h1><%= titulo %></h1>
    <p><%= descricao %></p>
  </main>

  <%- include('../partials/footer') %>
</body>
</html>
```

### `views/pages/contact.ejs`

```ejs
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title><%= titulo %></title>
  <link rel="stylesheet" href="/css/style.css">
</head>
<body>
  <%- include('../partials/header') %>

  <main class="container">
    <h1><%= titulo %></h1>
    <p><%= mensagem %></p>

    <form>
      <input type="text" placeholder="Seu nome" required>
      <input type="email" placeholder="Seu email" required>
      <textarea placeholder="Sua mensagem" required></textarea>
      <button type="submit">Enviar</button>
    </form>
  </main>

  <%- include('../partials/footer') %>
</body>
</html>
```

---

## 🎨 CSS — `public/css/style.css`

```css
body {
  font-family: 'Segoe UI', sans-serif;
  background-color: #f8f9fa;
  margin: 0;
  padding: 0;
}

header, footer {
  background-color: #2c3e50;
  color: white;
  padding: 15px;
  text-align: center;
}

nav a {
  color: white;
  margin: 0 10px;
  text-decoration: none;
  font-weight: bold;
}

.container {
  max-width: 900px;
  margin: 30px auto;
  padding: 20px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
}

form input, form textarea {
  width: 100%;
  margin-bottom: 10px;
  padding: 10px;
}
```

---

## ▶️ Executar o Projeto

```bash
node app.js
```

Abra no navegador:

```
http://localhost:3000
```

Você poderá navegar pelas páginas:
- `http://localhost:3000/` — Início
- `http://localhost:3000/sobre` — Sobre
- `http://localhost:3000/contato` — Contato

---

## ✅ Análise final

Com este projeto, você:

- Aprende a aplicar arquitetura MVC de forma clara.
- Separa controle, visual e navegação de maneira escalável.
- Utiliza partials reutilizáveis.
- Estiliza com CSS de forma limpa e objetiva.

**👨‍🏫 Ideal para você praticar conceitos de Node.js com EJS!**

---

NAs próximas aulas:
- ✅ Adicione banco de dados (PostgreSQL).
- ✅ Incluir rotas POST com tratamento de formulários.
- ✅ Aplique autenticação de usuários forma simples e teórica.

Boa prática e bons estudos!
