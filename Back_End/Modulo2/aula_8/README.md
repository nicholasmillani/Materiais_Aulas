# 🌾 Projeto Fazendinha Virtual com HTML e CSS

Este projeto foi desenvolvido como base para aulas introdutórias de **HTML** e **CSS**, utilizando conceitos fundamentais para a construção de um site estático. A proposta é aplicar técnicas modernas de **estruturação, estilização e organização visual** utilizando somente tecnologias nativas da web, sem bibliotecas externas ou JavaScript.

---

## 🎯 Objetivo

Capacitar os alunos a compreenderem e aplicarem:

- A estrutura de um documento HTML
- A separação entre estrutura (HTML) e estilo (CSS)
- O uso de classes e IDs para estilização
- Conceitos de grid layout e responsividade
- Estilização avançada com cores, gradientes e animações básicas em CSS

---

## 🗂️ Estrutura do Projeto

```
aula9/
├── index.html         # Página principal com os elementos da fazendinha
└── style.css          # Estilos aplicados aos elementos HTML
```

---

## 🧱 Tecnologias Utilizadas

| Tecnologia | Finalidade |
|------------|------------|
| HTML5      | Estrutura do conteúdo da página |
| CSS3       | Estilização, layout e animações |

---

## 🧾 Explicação dos Arquivos

### 📌 `index.html`

- Utiliza a semântica básica do HTML.
- Contém `div` com classes para representar seções temáticas da fazendinha:
  - Céu com gradiente
  - Campo verde
  - Elementos animados

#### 💡 Código HTML:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Fazendinha Virtual</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <section class="container">
    <div class="card card-ceu">
      <h2>Céu Azul Gradiente</h2>
      <p>Um belo céu com gradiente criado puramente em CSS, representando a parte superior da nossa fazendinha virtual.</p>
    </div>
    <div class="card card-campo">
      <h2>Campo Verde</h2>
      <p>Uma área verde vibrante que serve como base para nossa fazenda, onde nossos animais irão passear.</p>
    </div>
    <div class="card card-elementos">
      <h2>Elementos Animados</h2>
      <p>Uma vaquinha com manchas e um sol giratório, todos criados e animados usando apenas HTML e CSS, sem necessidade de JavaScript.</p>
    </div>
  </section>
</body>
</html>
```

---

### 🎨 `style.css`

- Define cores de fundo, bordas, espaçamentos e sombras.
- Aplica **responsividade com CSS Grid**, ideal para layouts adaptáveis.
- Inclui diferenciação visual por meio de bordas coloridas e animações sutis.

#### 💡 Código CSS:

```css
body {
  font-family: Arial, sans-serif;
  background-color: #111;
  margin: 0;
  padding: 2rem;
}

.container {
  display: grid;
  gap: 2rem;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  justify-items: center;
}

.card {
  background-color: #e9e9e9;
  border-radius: 10px;
  padding: 1.5rem;
  width: 90%;
  max-width: 400px;
  color: #2e4372;
  text-align: center;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  border: 2px solid transparent;
  transition: transform 0.3s ease;
}

.card:hover {
  transform: scale(1.02);
}

.card-ceu {
  border-color: #5c0f0f;
}

.card-campo {
  border-color: #5c0f0f;
}

.card-elementos {
  border-color: #3b6cb7;
}

.card h2 {
  font-size: 1.4rem;
  margin-bottom: 0.5rem;
  color: #233d6d;
}

.card p {
  font-size: 1rem;
  line-height: 1.5;
}
```

---

## 📚 Conteúdo Didático

### 📘 O que é HTML?

HTML (HyperText Markup Language) é a **linguagem de marcação** utilizada para estruturar o conteúdo da web.

Exemplo básico:

```html
<h1>Meu Título</h1>
<p>Este é um parágrafo.</p>
```

### 📗 O que é CSS?

CSS (Cascading Style Sheets) é a linguagem que define o **estilo visual** dos elementos HTML, como cor, tamanho, espaçamento e layout.

Exemplo básico:

```css
p {
  color: blue;
  font-size: 16px;
}
```

---

## 🧠 Conceitos-Chave para Estudo

| Conceito              | Descrição |
|-----------------------|-----------|
| `class`               | Agrupa elementos para aplicar estilos CSS |
| `id`                  | Identifica um elemento único |
| `display: grid`       | Organiza o layout em colunas/linhas |
| `box-shadow`          | Cria sombra ao redor de elementos |
| `border-radius`       | Arredonda cantos de uma caixa |
| `hover`               | Efeito aplicado quando o mouse passa sobre um elemento |
| `transition`          | Suaviza animações e mudanças de estilo |

---

## 🚀 Como Executar o Projeto

### Pré-requisitos

- [Visual Studio Code](https://code.visualstudio.com/)
- [Extensão Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)

### Passos

```bash
git clone https://github.com/seu-usuario/aula9.git
cd aula9
```

1. Abra o `index.html` no VSCode.
2. Clique com o botão direito e selecione **"Open with Live Server"**.
3. O site abrirá em `http://127.0.0.1:5500/`.

---

## 🧪 Atividades Sugeridas

- [ ] Criar novos cards com outros elementos da fazenda (árvore, casa, galinheiro...)
- [ ] Aplicar um gradiente diferente no céu
- [ ] Criar uma vaquinha animada com CSS
- [ ] Usar `@keyframes` para criar uma animação personalizada
- [ ] Testar responsividade alterando o tamanho da janela

---

## 👨‍🏫 Autor

Professor Cristiano Benites  
Especialista em inclusão digital, tecnologias assistivas e ensino de computação

---

## 🧾 Licença

Este projeto é de uso livre para fins educacionais.  
Créditos obrigatórios ao professor e à turma que participou da criação.

---
