# 🎨 Web Design — Resumo do Curso

> Anotações-resumo do curso **WD2026B** (Aprenda Mais / MEC), baseado na apostila de Fernando Schütz (UTFPR/Rede e-Tec Brasil).
> Resumo completo com imagens em [`docs/resumo-ilustrado.md`](docs/resumo-ilustrado.md).

---

## 📌 1. Conceitos de Web Design

- **Web Design** = extensão do Design aplicada à criação de sites/documentos para a Web.
- Não se resume a manipular software (Photoshop etc.) — envolve **planejamento, pesquisa e comunicação**.
- **Web Semântica** (Tim Berners-Lee/W3C): usar as tags HTML pelo seu **significado** (`<h1>`, `<p>`...), separando conteúdo (HTML) de apresentação (CSS).

### Gestalt — como o cérebro percebe imagens
| Lei | Ideia central |
|---|---|
| Semelhança | Objetos parecidos são agrupados |
| Proximidade | Elementos próximos formam um grupo |
| Boa continuidade | Elementos alinhados parecem relacionados |
| Pregnância | Formas simples são mais fáceis de assimilar |
| Clausura | O olho "fecha" formas incompletas |
| Experiência passada | Reconhecemos formas por já as conhecermos |

---

## 📐 2. Planejamento e Processo de Design

1. **Briefing** → coleta de informações com o cliente (objetivo, público-alvo, identidade visual).
2. **Arquitetura da Informação** → organizar o conteúdo em hierarquia (tipo árvore/mapa do site).
3. **Wireframe** → esqueleto do site (posição dos blocos, sem cor/imagem).
4. **Diagramação** → wireframe ganha cor, imagens e proporção real.

### Tipos de menu
- Horizontais: abas, barra de menu, botões, links de texto.
- Verticais: barra lateral, botões + texto.

### Hierarquia visual / áreas privilegiadas
- Leitura ocidental: esquerda → direita, cima → baixo.
- **Canto superior esquerdo** = área mais privilegiada.
- Conteúdo pouco relevante deve ficar **abaixo da dobra** (scroll).
- Destaque de texto: negrito/itálico, tamanho maior, cor, caixa alta.

---

## 🎨 3. Layout: Cor, Tipografia e Posicionamento

### Cores
- **Primárias:** azul, amarelo, vermelho
- **Secundárias:** mistura de 2 primárias (verde, violeta, laranja)
- **Terciárias:** primária + secundária adjacente
- **Complementares:** opostas no círculo cromático (alto contraste)
- **Análogas:** vizinhas no círculo (harmônicas)
- **Acromáticas:** branco, preto, cinza
- **Monocromáticas:** variações de tom de uma mesma cor

**Esquema triádico:** 3 cores igualmente espaçadas (120°) no círculo cromático.

**Dicas de harmonização:**
- Variar luminância de uma mesma cor.
- Cuidado ao combinar cores opostas (complementares).
- Usar branco como "cor de alívio" em fundos com cores quentes.
- Cores diferenciam links visitados/não visitados.

### Tipografia
- **Serifa:** decorativa, ruim para texto corrido na tela (baixa resolução).
- **Sem serifa:** melhor legibilidade em monitores.
- Prioridade: legibilidade > estética, sempre alinhada ao conceito do site.

### Estruturação com `<div>` + CSS
- Reset de estilos (`* { margin:0; padding:0; }`) evita inconsistência entre navegadores.
- Um mesmo HTML pode virar menu horizontal ou vertical **só mudando o CSS** (ex.: `float: left` vs `display: block`).
- Boas práticas: `<h1>` para o nome do site/logo, `<ul>`/`<li>` para menus, `<div>` como contêiner.

---

## 💻 4. Estudos de Caso Completos

| Projeto | Público-alvo | Paleta | Estrutura |
|---|---|---|---|
| **OldCar** (carros antigos) | Apaixonados por carros clássicos, todas idades | Azul escuro `#000134`, marrom `#696E58`, branco, cinza `#333333` | `#top`, `#container` (`#main` + `#sidebar`), `#footer` |
| **Cat&Dog** (petshop) | Donos de cães e gatos | Verde-azulado `#008B82`, branco `#FFFFFF`, laranja `#F5A954` | `#container` → `#top`, `#content` (`#menu` + `#main`), `#footer` |

**Fluxo de desenvolvimento comum aos dois:**
1. Definir paleta de cores e público-alvo.
2. Criar protótipo/wireframe do layout.
3. Organizar pastas (`images/`, `stylesheets/`, `javascripts/`).
4. Codificar HTML semântico (estrutura primeiro, sem estilo).
5. Criar CSS separado (`stylesheet.css`) para estilizar.
6. Adicionar interatividade (ex.: jQuery para transição de imagens).

---

## 📚 Fontes principais
- WATRALL, E.; SIARTO, J. **Use a Cabeça! Web Design**. O'Reilly/Alta Books, 2009.
- NIELSEN, J.; LORANGER, N. **Usabilidade na Web**. Elsevier, 2007.
- PEDROSA, I. **Da cor à cor inexistente**. Senac Nacional, 2009.
- SCHÜTZ, F. **Web design**. UTFPR/Rede e-Tec Brasil, 2013.
