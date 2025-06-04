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

Exemplo de estrutura:

```html
<div class="card card-ceu">
  <h2>Céu Azul Gradiente</h2>
  <p>Descrição explicativa...</p>
</div>
```

### 🎨 `style.css`

- Define cores de fundo, bordas, espaçamentos e sombras.
- Aplica **responsividade com CSS Grid**, ideal para layouts adaptáveis.
- Inclui diferenciação visual por meio de bordas coloridas e animações sutis.

Principais recursos aplicados:

- `display: grid` e `grid-template-columns`
- `border-radius`, `box-shadow`
- `hover` para efeito interativo

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
