# 🎨 Anotações de CSS

> Guia usado como referência: [Manual do CSS - freeCodeCamp](https://www.freecodecamp.org/portuguese/news/manual-do-css-um-guia-pratico-de-css-para-desenvolvedores/)

---

## ⭐ Comandos mais usados (referência rápida)

| Comando | Pra que serve |
|---|---|
| `color` | cor do texto |
| `background-color` | cor de fundo |
| `font-size` | tamanho da fonte |
| `font-family` | qual fonte usar |
| `font-weight` | espessura da fonte (negrito etc) |
| `text-align` | alinhamento do texto (left, center, right, justify) |
| `text-decoration` | sublinhado, riscado etc |
| `line-height` | altura da linha do texto |
| `width` / `height` | largura e altura do elemento |
| `margin` | espaço **fora** da borda do elemento |
| `padding` | espaço **dentro** da borda, entre ela e o conteúdo |
| `border` | borda do elemento (espessura, estilo, cor) |
| `border-radius` | arredonda os cantos |
| `box-sizing` | inclui (ou não) padding/border no cálculo de width/height |
| `display` | como o elemento se comporta no layout (block, inline, flex, grid, none) |
| `position` | como o elemento é posicionado (static, relative, absolute, fixed, sticky) |
| `top` / `right` / `bottom` / `left` | deslocamento do elemento quando `position` não é `static` |
| `z-index` | qual elemento fica na frente quando eles se sobrepõem |
| `flex-direction` | direção dos itens no Flexbox (linha ou coluna) |
| `justify-content` | alinha os itens no eixo principal do Flexbox/Grid |
| `align-items` | alinha os itens no eixo cruzado do Flexbox/Grid |
| `gap` | espaço entre os itens no Flexbox/Grid |
| `grid-template-columns` | define as colunas do Grid |
| `overflow` | o que fazer quando o conteúdo não cabe (`hidden`, `scroll`, `auto`) |
| `cursor` | ícone do mouse ao passar por cima (`pointer`, `default` etc) |
| `opacity` | transparência do elemento (0 a 1) |
| `transition` | anima a mudança suave de uma propriedade |
| `transform` | move, gira ou escala um elemento |
| `:hover` | estiliza o elemento quando o mouse passa em cima |
| `content` | insere conteúdo com `::before`/`::after` |
| `@media` | aplica estilos diferentes dependendo do tamanho da tela |

---

## 🏗️ Introdução

**Requisitos básicos:** editor de texto, navegador e conhecimento em HTML.

**Estrutura de uma regra CSS:** seletor + declaração (propriedade + valor).
```css
a, h1, h2 { color: green; }
```

**`id` (#) vs `class` (.)**
- `class`: pode se repetir várias vezes na página
- `id`: só pode ser usado **uma vez** por página, tem prioridade maior no CSS

---

## 🎯 Seletores

Definem quais elementos vão receber o estilo.

| Seletor | Exemplo | O que faz |
|---|---|---|
| Elemento | `p {}` | todos os `<p>` |
| Classe | `.nome {}` | elementos com `class="nome"` |
| Id | `#nome {}` | elemento com `id="nome"` |
| Elemento + classe | `p.nome {}` | só `<p>` com essa classe |
| Várias classes | `.nome.roger {}` | tem as duas classes |
| Agrupado | `p, .nome {}` | aplica nos dois |
| Descendente | `p span {}` | `span` dentro de `p` (qualquer nível) |
| Filho direto | `p > span {}` | `span` filho direto de `p` |
| Irmão adjacente | `p + span {}` | `span` logo depois de um `p` |

---

## 🌊 Cascata

É o processo que decide qual estilo "vence" quando várias regras miram o mesmo elemento. Leva em conta: **especificidade**, **importância** (`!important`), **herança** e **ordem no arquivo**.

---

## 🎯 Especificidade

Quando duas regras miram o mesmo elemento, **a mais específica vence**; se empatar, **vence a que vem por último**.

Ordem de força (do mais fraco pro mais forte):
1. Seletor de elemento → `p {}`
2. Classe, pseudoclasse, atributo → `.nome {}`
3. Id → `#nome {}`
4. Estilo inline → `style="..."`
5. `!important` (ganha de tudo, evite usar sem necessidade)

---

## 🧬 Herança

Algumas propriedades passam automaticamente do elemento pai pros filhos (ex: `color`, `font-family`, `font-size`, `line-height`, `text-align`). Outras não fazem sentido herdar (ex: `background-color`, `margin`, `border`).

```css
p {
  background-color: inherit; /* força herdar do pai */
}
```

Outros valores especiais: `initial` (volta ao padrão do navegador) e `unset` (herda se for herdável, senão volta ao padrão).

---

## 📥 Importar

Importa um arquivo CSS dentro de outro. Precisa estar **no topo do arquivo**, antes de qualquer outra regra.
```css
@import url(arquivo.css);
@import url(impressao.css) print;
```

---

## 🏷️ Seletores de Atributo

```css
p[id] { }                 /* tem o atributo id */
p[id="meu-id"] { }        /* valor exato */
a[href^="https"] { }      /* começa com */
a[href$=".pdf"] { }       /* termina com */
a[href*="google"] { }     /* contém */
```

---

## 🎭 Pseudoclasses

Selecionam um elemento pelo **estado** dele. Começam com `:` (um dois-pontos).

| Pseudoclasse | Quando ativa |
|---|---|
| `:hover` | mouse em cima |
| `:active` | sendo clicado |
| `:visited` | link já visitado |
| `:focus` | elemento em foco (ex: input selecionado) |
| `:first-child` / `:last-child` | primeiro/último filho |
| `:nth-child(odd)` / `:nth-child(even)` | filhos ímpares/pares |

```css
a:hover { color: red; }
ul:nth-child(odd) { background-color: #eee; }
```

---

## ✂️ Pseudoelementos

Estilizam uma **parte específica** de um elemento. Começam com `::` (dois dois-pontos).

| Pseudoelemento | O que faz |
|---|---|
| `::before` / `::after` | insere conteúdo antes/depois do elemento (usa `content:`) |
| `::first-line` | estiliza só a primeira linha |
| `::first-letter` | estiliza só a primeira letra |

```css
p::before {
  content: "★ ";
}
```

---

## 🎨 Cores

```css
color: red;                    /* nome */
color: rgb(255, 0, 0);         /* RGB */
color: rgba(255, 0, 0, 0.5);   /* RGB + transparência (alfa) */
color: #ff0000;                /* hexadecimal */
color: hsl(0, 100%, 50%);      /* matiz, saturação, brilho */
```
> 💡 `transparent` e `currentColor` (herda a cor do `color` do próprio elemento) também são valores válidos.

---

## 📏 Unidades

| Unidade | O que é |
|---|---|
| `px` | pixel — a mais usada |
| `%` | porcentagem em relação ao elemento pai |
| `em` | relativo ao `font-size` do próprio elemento |
| `rem` | relativo ao `font-size` do elemento raiz (`html`) — mais previsível que `em` |
| `vw` / `vh` | % da largura/altura da tela (viewport) |
| `vmin` / `vmax` | % do menor/maior lado da tela |
| `fr` | fração — usada no CSS Grid |

---

## 🔗 URL

Usada pra carregar recursos externos, como imagens.
```css
background-image: url(imagem.png);       /* caminho relativo */
background-image: url(../imagem.png);    /* volta uma pasta */
background-image: url(/imagem.png);      /* a partir da raiz do site */
background-image: url(https://site.com/imagem.png); /* absoluto */
```

---

## 🧮 Calc

Faz contas dentro do CSS, misturando unidades diferentes.
```css
div {
  max-width: calc(80% - 100px);
}
```
> ⚠️ Sempre deixe espaço ao redor de `+` e `-`, senão não funciona.

---

## 🖼️ Backgrounds

```css
background-color: yellow;
background-image: url(imagem.png);
background-position: top right;       /* posição da imagem */
background-repeat: no-repeat;         /* repeat | repeat-x | repeat-y | no-repeat */
background-size: cover;               /* cover | contain | auto */
background-attachment: fixed;         /* fixa o fundo ao rolar a página */

/* abreviação */
background: url(bg.png) top left no-repeat;
background: linear-gradient(#fff, #333); /* gradiente */
```

---

## 💬 Comentários

```css
/* isso é um comentário */
p { color: red; } /* pode ir no fim da linha também */
```
> ⚠️ CSS **não** tem comentário com `//` — isso vira erro de sintaxe e a linha é ignorada.

---

## 🧩 Propriedades Personalizadas (Variáveis)

```css
:root {
  --cor-primaria: yellow;
}

p {
  color: var(--cor-primaria);
}

.container {
  margin: var(--espacamento, 30px); /* 30px é o valor padrão se a variável não existir */
}
```
- `:root` = elemento raiz da página (equivale ao `html`, mas com mais prioridade)
- Variáveis seguem as regras de **escopo**: só valem dentro do elemento onde foram criadas e nos filhos dele

---

## 🔤 Fontes

```css
font-family: Helvetica, Arial, sans-serif; /* usa a 1ª disponível, senão tenta a próxima */
font-weight: bold;      /* normal | bold | 100 a 900 */
font-style: italic;     /* normal | italic | oblique */
font-size: 20px;

/* abreviação (ordem: style variant weight size family) */
font: italic bold 20px Helvetica;
```
- Fontes genéricas de reserva: `serif`, `sans-serif`, `monospace`, `cursive`, `fantasy`
- `@font-face` permite carregar uma fonte personalizada (arquivo `.woff`, `.woff2`, `.ttf` etc)

---

## ✍️ Tipografia

| Propriedade | Função |
|---|---|
| `text-transform` | `uppercase`, `lowercase`, `capitalize` |
| `text-decoration` | `underline`, `line-through`, `none` |
| `text-align` | `left`, `right`, `center`, `justify` |
| `line-height` | altura da linha de texto |
| `letter-spacing` / `word-spacing` | espaço entre letras/palavras |
| `text-indent` | recuo da primeira linha do parágrafo |
| `text-shadow` | sombra no texto (`x y desfoque cor`) |
| `white-space: nowrap` | impede quebra de linha |

---

## 📦 Box Model (Modelo de Caixa)

Todo elemento é uma caixa, formada de dentro pra fora por:

**Conteúdo → Padding (preenchimento) → Border (borda) → Margin (margem)**

- **Margem**: espaço **fora** do elemento (entre ele e os outros)
- **Border**: contorno ao redor do padding + conteúdo
- **Padding**: espaço **dentro** do elemento (entre o conteúdo e a borda)
- **Content**: onde fica o texto/imagem/vídeo

---

## 🖼️ Border

```css
border-style: solid;      /* solid | dashed | dotted | double | none */
border-width: 2px;
border-color: black;
border-radius: 8px;       /* arredonda cantos (50% = círculo) */

/* abreviação */
border: 2px solid black;
```
Dá pra estilizar cada lado separado: `border-top`, `border-right`, `border-bottom`, `border-left`.

---

## ⬜ Preenchimento (Padding)

Espaço **dentro** da borda.
```css
padding: 20px;              /* todos os lados */
padding: 20px 10px;         /* cima/baixo | esquerda/direita */
padding: 20px 10px 30px;    /* cima | lados | baixo */
padding: 20px 10px 5px 0px; /* cima → direita → baixo → esquerda (sentido horário) */
```

---

## ↔️ Margem

Espaço **fora** da borda, entre um elemento e outro. Segue a mesma lógica do `padding` (1 a 4 valores, sentido horário).

```css
margin: 0 auto; /* centraliza um bloco horizontalmente */
```
> 💡 Margem é a única que aceita valor **negativo** (aproxima elementos ou faz sobrepor).

---

## 📐 Dimensionamento da Caixa (Box Sizing)

Por padrão, `width`/`height` só definem o conteúdo — padding e border são somados por fora e aumentam o tamanho real.

```css
* {
  box-sizing: border-box; /* width/height já incluem padding e border */
}
```
Recomendado aplicar globalmente pra evitar dor de cabeça com medidas.

---

## 🖥️ Display

| Valor | Comportamento |
|---|---|
| `block` | ocupa a linha toda (ex: `div`, `p`) |
| `inline` | ocupa só o espaço do conteúdo, ignora `width`/`height` (ex: `span`, `a`) |
| `inline-block` | como `inline`, mas respeita `width`/`height` |
| `none` | some da tela (mas continua no HTML) |
| `flex` | ativa o Flexbox |
| `grid` | ativa o Grid |

---

## 📍 Posicionamento

```css
position: static;    /* padrão, segue o fluxo normal */
position: relative;  /* se move a partir da própria posição original */
position: absolute;  /* sai do fluxo, posiciona em relação ao pai mais próximo com position != static */
position: fixed;     /* fica fixo na tela, ignora o scroll */
position: sticky;    /* gruda num ponto ao rolar a página */
```
Usados junto com `top`, `right`, `bottom`, `left` pra definir a posição exata.

---

## 🌊 Float e Clear

`float` tira o elemento do fluxo normal e faz o conteúdo ao redor "molde" nele — hoje é mais usado pra colocar imagem ao lado de texto (layouts modernos usam Flexbox/Grid).

```css
img { float: left; }    /* left | right | none */
.limpa { clear: both; } /* impede que o próximo elemento flua ao lado do float */
```

---

## 🔢 Z-index

Controla qual elemento fica **na frente** quando eles se sobrepõem. Só funciona em elementos com `position` diferente de `static`.

```css
.frente { position: relative; z-index: 10; } /* fica na frente */
.fundo   { position: relative; z-index: 1; }  /* fica atrás */
```
Quanto maior o número, mais na frente.

---

## 🔲 CSS Grid

Layout em **2 dimensões** (linhas e colunas ao mesmo tempo).

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr; /* 3 colunas iguais */
  grid-template-rows: 100px auto;
  gap: 20px; /* espaço entre células */
}

.item {
  grid-column: 1 / 3; /* ocupa da linha 1 até a 3 */
}
```
- `fr` = fração do espaço disponível
- `repeat(4, 1fr)` = repete "1fr" 4 vezes
- `grid-template-areas` permite nomear áreas e organizar visualmente o layout

---

## 📏 Flexbox

Layout em **1 dimensão** (linha OU coluna).

```css
.container {
  display: flex;
  flex-direction: row;        /* row | column | row-reverse | column-reverse */
  justify-content: center;    /* alinha no eixo principal (horizontal se row) */
  align-items: center;        /* alinha no eixo cruzado (vertical se row) */
  flex-wrap: wrap;            /* permite quebrar linha */
  gap: 10px;
}
```
> 💡 Grid e Flexbox não competem: Grid é ótimo pra estrutura geral da página, Flexbox pra alinhar itens dentro de um componente.

---

## 📊 Tabelas (CSS)

```css
table {
  border-collapse: collapse; /* junta as bordas das células */
  width: 100%;
}

th, td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
}
```

---

## 🎯 Centralizando

```css
/* Horizontalmente (bloco com largura definida) */
margin: 0 auto;

/* Com Flexbox (o jeito mais fácil hoje em dia) */
.container {
  display: flex;
  justify-content: center; /* horizontal */
  align-items: center;     /* vertical */
}
```

---

## 📋 Listas (CSS)

```css
ul {
  list-style-type: none;   /* disc | circle | square | none */
  list-style-position: inside;
}
```

---

## 📱 Media Queries e Design Responsivo

Aplicam estilos diferentes dependendo do tamanho da tela.
```css
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }
}
```

---

## 🧪 Feature Queries

Aplicam um estilo só se o navegador **suportar** aquela funcionalidade do CSS.
```css
@supports (display: grid) {
  .container { display: grid; }
}
```

---

## 🌫️ Filtros

Aplicam efeitos visuais (tipo Photoshop) direto no CSS.
```css
img {
  filter: blur(5px);       /* desfoque */
  filter: grayscale(100%); /* preto e branco */
  filter: brightness(1.2); /* brilho */
}
```

---

## 🔄 Transformações

Move, gira, escala ou distorce um elemento **sem afetar o layout ao redor**.
```css
.box {
  transform: rotate(45deg);
  transform: scale(1.2);
  transform: translateX(20px);
}
```

---

## 🎬 Transições

Anima a mudança de um valor de propriedade de forma suave.
```css
.box {
  transition: background-color 0.3s ease;
}
.box:hover {
  background-color: blue;
}
```

---

## 🎞️ Animações

Permitem criar animações mais complexas, com várias etapas (`@keyframes`).
```css
@keyframes aparecer {
  from { opacity: 0; }
  to { opacity: 1; }
}

.box {
  animation: aparecer 1s ease-in;
}
```

---

## 🧹 Normalizando o CSS

Navegadores têm estilos padrão diferentes entre si (margens, tamanhos de fonte etc). Usar um **reset/normalize** no início do projeto deixa tudo consistente entre navegadores. Bibliotecas comuns: `normalize.css`, `reset.css`.

---

## 🐞 Lidando com Erros

O CSS **não trava** com erro: se uma linha tem um erro de sintaxe, o navegador simplesmente **ignora aquela linha** e segue para a próxima. Por isso é fácil ter um estilo "sumindo" sem aviso nenhum — sempre confira o DevTools do navegador (aba Elements/Inspecionar) pra ver o que está sendo aplicado de fato.

---

## 🏭 Prefixos dos Fabricantes

Prefixos usados no passado pra recursos ainda não padronizados em todos os navegadores.
```css
.box {
  -webkit-transform: rotate(45deg); /* Chrome/Safari */
  -moz-transform: rotate(45deg);    /* Firefox */
  transform: rotate(45deg);         /* padrão (sempre por último) */
}
```
Hoje em dia a maioria dos recursos não precisa mais disso — confira no [caniuse.com](https://caniuse.com/) antes de usar.

---

## 🖨️ CSS para Impressão

Estilos aplicados só quando a página é impressa.
```css
@media print {
  nav, footer {
    display: none; /* esconde elementos que não fazem sentido no papel */
  }
}
```

---

## ✏️ Estilizando Elementos, Textos e Listas (resumo rápido)

### Texto e cores
```css
color: red;
font-size: 16px;
font-family: Arial, sans-serif;
text-align: center;
font-weight: bold;
```

### Fundo e bordas
```css
background-color: blue;
border: 2px solid black;
border-radius: 8px;
```

### Box Model (espaçamento e tamanho)
```css
width: 300px;
height: 150px;
padding: 20px;
margin: 15px;
```

### Layout básico
```css
display: block;
display: inline;
display: flex;
justify-content: center; /* alinha horizontalmente no flex */
align-items: center;     /* alinha verticalmente no flex */
```
