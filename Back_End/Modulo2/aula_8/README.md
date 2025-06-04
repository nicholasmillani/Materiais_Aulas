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
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fazendinha Virtual</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="ceu">
    <div class="sol"></div>
  </div>
  <div class="campo">
    <div class="vaca">
      <div class="corpo"></div>
      <div class="mancha uma"></div>
      <div class="mancha duas"></div>
      <div class="cabeca"></div>
    </div>
  </div>
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
/* Estilização global da página */
body {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
  overflow-x: hidden;
}

/* Seção do céu com gradiente azul para representar o topo da fazenda */
.ceu {
  width: 100%;
  height: 50vh;
  background: linear-gradient(to bottom, #87ceeb, #ffffff); /* gradiente azul para branco */
  position: relative;
  overflow: hidden;
}

/* Seção do campo verde onde os animais passeiam */
.campo {
  width: 100%;
  height: 50vh;
  background-color: #7cfc00; /* verde vibrante */
  position: relative;
  overflow: hidden;
}

/* Sol giratório com animação e movimento de flutuação */
.sol {
  position: absolute;
  top: 30px;
  right: 30px;
  width: 100px;
  height: 100px;
  background: radial-gradient(circle, #FFD700 60%, #FFA500); /* amarelo com laranja */
  border-radius: 50%; /* formato circular */
  animation: girar 6s linear infinite, flutuar 4s ease-in-out infinite alternate;
}

/* Animação para girar o sol continuamente */
@keyframes girar {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

/* Animação para flutuar o sol suavemente para cima e para baixo */
@keyframes flutuar {
  from { top: 30px; }
  to { top: 50px; }
}

/* Vaquinha com animação de caminhada horizontal contínua */
.vaca {
  position: absolute;
  bottom: 10px;
  width: 140px;
  height: 100px;
  animation: andar 8s linear infinite; /* movimento da vaquinha */
}

/* Animação da vaquinha: atravessa a tela e vira ao chegar no fim */
@keyframes andar {
  0% { left: -150px; }
  50% { left: calc(100% + 10px); transform: scaleX(1); }
  51% { transform: scaleX(-1); }
  100% { left: -150px; transform: scaleX(-1); }
}

/* Corpo principal da vaca */
.corpo {
  width: 100%;
  height: 70px;
  background: white;
  border-radius: 12px;
  position: relative;
}

/* Cabeça circular da vaca */
.cabeca {
  width: 40px;
  height: 40px;
  background: white;
  border-radius: 50%;
  position: absolute;
  left: -35px;
  top: 15px;
}

/* Manchas pretas circulares no corpo da vaca */
.mancha {
  position: absolute;
  background: black;
  border-radius: 50%;
}

/* Primeira mancha */
.mancha.uma {
  width: 20px;
  height: 20px;
  top: 15px;
  left: 25px;
}

/* Segunda mancha */
.mancha.duas {
  width: 18px;
  height: 18px;
  top: 35px;
  left: 85px;
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
