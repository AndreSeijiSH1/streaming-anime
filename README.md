# Sekai Stream: landing page de assinatura

> Projeto de estudo: uma página de assinatura de um serviço de streaming **fictício**, feita **apenas com HTML e CSS**, sem frameworks e sem JavaScript.

🔗 **Site no ar:** [https://andreseijish1.github.io/streaming-anime/]

<!-- Coloque aqui os prints do site (salve as imagens numa pasta "prints/" do repositório) -->
| Desktop | Celular |
|---|---|
| ![Versão desktop](prints/desktop.png) | ![Versão celular](prints/celular.png) |

---

## 📌 Sobre o projeto

Este é o primeiro de uma série de sites que estou construindo para treinar os padrões de interface mais comuns da web, antes de construir o **Sekai List**, o meu projeto principal (uma plataforma de catálogo e avaliação de animes).

O layout segue o padrão de páginas de assinatura de serviços de streaming. O objetivo não era só montar a página, mas entender **por que** cada tag e cada propriedade está no lugar onde está.

**O que tem na página:**
- Header com nome do site e botões de ação
- Hero com fundo decorativo feito só com CSS
- Seletor Mensal/Anual (estrutura pronta para receber JavaScript)
- Cards de planos com destaque para o plano mais popular
- Lista de benefícios com ícones de check
- Layout responsivo para celular

## 🛠️ Tecnologias

- HTML5 semântico
- CSS3 (Flexbox, variáveis CSS, pseudo-elementos, media queries)
- Git e GitHub Pages

## 📁 Estrutura

```
├── index.html
├── style.css
└── img/
```

O `style.css` é organizado com um **índice** no topo, e cada parte da página tem a sua seção:

```
1. Variáveis → 2. Base → 3. Botões → 4. Topo → 5. Hero → 6. Planos → 7. Media Query
```

A ordem vai **do geral para o específico**, o que também ajuda a cascata do CSS: as regras gerais ficam em cima e as exceções embaixo.

---

## 📚 O que aprendi

### HTML semântico: escolher a tag pelo que o bloco **é**, não pela aparência

| Tag | Quando usar |
|---|---|
| `<section>` | Um tema da página com título próprio ("Escolha seu plano") |
| `<article>` | Conteúdo que faz sentido sozinho (cada card de plano) |
| `<div>` | Caixa sem significado, só para o layout (o container flex dos cards) |
| `<a>` | Leva para **outro lugar** ("Ver planos") |
| `<button>` | Faz uma **ação na página** (Mensal / Anual) |
| `<strong>` | Destaca um trecho importante dentro de uma frase ("**4 devices**") |
| `<small>` | Letras miúdas e avisos legais |

- **A aparência não decide a tag.** Um link pode parecer um botão (é só CSS), mas, se leva para outra página, continua sendo `<a>`.
- **Headings formam o sumário da página.** O preço não é um `<h4>` só porque é grande: ele é uma informação, não o título de uma seção. Tamanho é trabalho do CSS.
- **`aria-pressed`** informa ao leitor de tela qual opção está ativa (Mensal ou Anual), e o CSS usa o mesmo atributo para pintar o botão selecionado.

### Classes: base + variação

Um elemento pode ter **várias classes**, e recebe as regras de todas elas somadas:

```html
<a href="#" class="botao botao-destaque">Ver planos</a>
<a href="#" class="botao botao-branco">Entrar</a>
```

```css
.botao         { /* o que todo botão tem em comum: formato, padding, negrito */ }
.botao-destaque { /* só o que muda: fundo colorido, texto preto */ }
.botao-branco  { /* só o que muda: fundo transparente, borda branca */ }
```

Assim não repito código, e mudar o formato de todos os botões é questão de uma linha. É a mesma ideia de um componente com variações no React.

### Cascata, especificidade e herança

- **Herança:** `color` e `font-family` aplicados uma vez no `body` passam para todos os filhos. Os links são a exceção, porque o navegador dá a eles uma cor própria.
- **Especificidade:** uma classe (`.botao`) vence uma tag (`a`). Duas classes (`.teste-gratis .botao`) vencem uma.
- **Empate:** quando duas regras têm a mesma força, **vence a que está mais embaixo no arquivo**. Foi por isso que o hover do botão em destaque não funcionava: a regra genérica estava depois da específica.
- **Regra de ouro:** aplicar cada mudança **no menor alvo possível**. Um `text-align` no lugar errado se espalha por herança para tudo o que está dentro.

### Box model na prática

| Propriedade | Cria espaço... |
|---|---|
| `padding` | **dentro** da caixa (o fundo e a imagem cobrem esse espaço) |
| `margin` | **fora** da caixa (entre ela e as vizinhas) |
| `gap` | **entre os filhos** de um container flex ou grid |

- `* { box-sizing: border-box; }` faz o `width` incluir padding e borda, então o tamanho que eu escrevo é o tamanho que aparece.
- O navegador aplica **margens padrão** em `body`, títulos, parágrafos e listas (a *user agent stylesheet*). Muitos espaços "misteriosos" vinham daí.
- A margem padrão é em `em`, então ela **cresce junto com a fonte**: um preço com `font-size: 2.5rem` ganhava 40px de margem sem eu perceber.
- `width: fit-content` + `margin: 0 auto` deixa uma caixa do tamanho do conteúdo e centralizada (usei na pílula Mensal/Anual).

### Flexbox

```css
.topo {
    display: flex;
    justify-content: space-between; /* eixo principal: logo numa ponta, botões na outra */
    align-items: center;            /* eixo cruzado: tudo na mesma altura */
}
```

- O flex organiza só os **filhos diretos**. Para alinhar só alguns elementos, eu os agrupo numa `<div>` própria.
- O `align-items` padrão (`stretch`) deixa os cards com **a mesma altura**, mesmo com conteúdos diferentes.
- `flex-direction: column` empilha os itens (usei para os cards no celular).

### Centralizar: três ferramentas diferentes

| Para centralizar... | Use |
|---|---|
| Texto dentro de uma caixa | `text-align: center` |
| Uma caixa dentro do pai | `width: fit-content` + `margin: 0 auto` |
| Itens de um flex | `justify-content` / `align-items` |

### Pseudo-elementos: `::before` e `::after`

Os ✓ coloridos da lista de benefícios são criados pelo CSS, sem mexer no HTML:

```css
.lista-beneficios li {
    position: relative;
    padding-left: 1.2rem;   /* abre espaço para o ✓ */
}

.lista-beneficios li::before {
    content: "✓";           /* obrigatório: sem content, nada aparece */
    color: var(--cor-destaque);
    position: absolute;     /* tira o ✓ do fluxo do texto */
    left: 0;
}
```

Com o `position: absolute`, as linhas quebradas continuam alinhadas com o texto, e não embaixo do ícone. Decoração (como os ✨ do "O MAIS POPULAR") fica no CSS, porque o HTML é para conteúdo.

### Fundo decorativo feito só com CSS

```css
.hero {
    background-image:
        radial-gradient(circle at 80% 30%, rgba(90, 169, 230, 0.35), transparent 50%),
        radial-gradient(circle at 15% 85%, rgba(150, 100, 230, 0.25), transparent 45%);
    background-color: var(--cor-preto);
}
```

- O fundo é decorativo, então fica no CSS (`background-image`), e não no HTML (`<img>`).
- Várias camadas são separadas por vírgula, e **a primeira fica por cima**.
- Com gradientes, não preciso de nenhuma imagem de terceiros.

### Variáveis CSS

```css
:root {
    --cor-destaque: #5aa9e6;
    --padding-padrao: 16px;
}
```

As cores e os espaçamentos ficam definidos num lugar só. Para mudar a paleta, altero uma linha.

### Responsividade

```css
@media (max-width: 768px) {
    /* só vale em telas de até 768px */
}
```

- A media query fica no **final** do arquivo, para vencer as regras normais no empate.
- Ela depende da `<meta name="viewport">` no `<head>`: sem ela, o celular finge ter ~980px e a media query nunca é ativada.
- Fiz o projeto **desktop-first** (`max-width`, desfazendo valores grandes). No próximo, quero testar **mobile-first** (`min-width`), que é a abordagem recomendada.
- `display: none` esconde no celular o botão duplicado do header.
- `white-space: nowrap` impede que o texto dos botões quebre em várias linhas.
- **O celular revela problemas que o desktop esconde:** o `padding` do `<main>` criava uma moldura em volta do hero que quase não aparecia em telas grandes.

### Acessibilidade

- `:focus-visible` junto com `:hover`, para quem navega pelo teclado ver o mesmo efeito.
- Contraste: os botões coloridos usam texto preto, porque texto branco sobre cores claras e vivas fica abaixo do contraste recomendado (4,5:1).
- Emojis decorativos ficam fora do HTML, para o leitor de tela não ler "brilhos, O MAIS POPULAR, brilhos".

---

## 🐛 Desafios que resolvi

**A página inteira "encolhia" no celular.**
Os dois cards de planos continuavam lado a lado e, somados, ficavam mais largos que a tela. O navegador diminuía o zoom da página inteira para caber tudo. A solução foi `flex-direction: column` na media query. Para encontrar o culpado, usei um truque de depuração:

```css
* { outline: 1px solid red; } /* desenha a borda de todos os elementos sem mudar o tamanho */
```

**O hover do botão em destaque não funcionava.**
As duas regras de hover tinham a mesma especificidade, e a genérica estava depois da específica no arquivo, então ela vencia. Bastou reordenar: base primeiro, variação depois.

**A margem que "vazava" do header.**
A margem do `<h1>` atravessava o header e criava uma faixa acima dele (*margin collapse*). Um `padding` no pai impede o vazamento.

**Erros de digitação silenciosos.**
Um seletor `.pano-taxa-imposto` (sem o "l") e um caminho `..css/style.css` (sem a barra) não geram erro nenhum: simplesmente não funcionam. O F12 (aba Styles e aba Network) foi o que me ajudou a encontrar.

---

## 🔜 Próximos passos

- [ ] Deixar o header transparente, por cima do hero
- [ ] Fazer a troca Mensal/Anual funcionar com JavaScript
- [ ] Próximo projeto de estudo: uma grade de catálogo com **CSS Grid**

---

## ⚠️ Aviso

Projeto feito **exclusivamente para fins de estudo**. A marca "Sekai Stream", os planos e os preços são **fictícios**, e o site não oferece nenhum serviço real.

---

Feito por **André Seiji Shiroma** · [LinkedIn](COLOQUE-SEU-LINK) · [GitHub](https://github.com/AndreSeijiSH1)
