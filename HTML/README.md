# 📝 Anotações de HTML

---

## 🏗️ Estrutura Básica

```html
<!DOCTYPE html>
<html>
<head></head>
<body>

  <header> <!-- Cabeçalho -->
    <nav></nav> <!-- Barra de navegação -->
  </header>

  <main> <!-- Conteúdo principal -->
    <section></section> <!-- Seções -->
  </main>

  <footer></footer> <!-- Rodapé -->

</body>
</html>
```

---

## 🏷️ Tags Principais

| Tag | Descrição |
|-----|-----------|
| `<h1>` até `<h5>` | Títulos (h1 é o maior, h5 o menor) |
| `<p>` | Parágrafo |
| `<a>` | Link |
| `<img>` | Imagem |
| `<button>` | Botão |
| `<br>` | Quebra de linha |
| `<hr>` | Linha horizontal |

### Exemplos

```html
<!-- Ícone do site (vai no <head>) -->
<link rel="shortcut icon" href="img/icone.png" type="image/x-icon">

<!-- Imagem (alt = descrição da imagem) -->
<img src="img/foto.png" alt="Descrição da foto">

<!-- Botão (class chama o estilo no CSS) -->
<button class="menu">Olá</button>

<!-- Título -->
<h1>Título principal</h1>

<!-- Parágrafo -->
<p>Texto do parágrafo</p>

<!-- Link externo (abre em nova aba, sem sair do site) -->
<a href="https://google.com" target="_blank" rel="external">
  Clique aqui para ir ao Google
</a>

<!-- Link interno (vai pra outra página ao clicar) -->
<a href="outra-pagina.html" rel="next">Ir para outra página</a>
```

---

## ✏️ Formatações de Texto

```html
<b>Negrito</b>
<strong>Negrito semântico</strong>
<i>Itálico</i>
<em>Itálico semântico</em>
<u>Sublinhado</u>
<sub>Subscrito</sub>   <!-- ex: H₂O -->
<sup>Sobrescrito</sup> <!-- ex: x² -->
<big>Texto maior que o padrão</big>
<small>Texto menor que o padrão</small>
```

> 💡 **Dica:** Prefira `<strong>` a `<b>` e `<em>` a `<i>` — as versões semânticas têm significado pra leitores de tela e SEO.

---

## 📋 Listas

### Ordenada (`<ol>`)
```html
<ol type="A" reversed>
  <li>Item A</li>
  <li>Item B</li>
  <li>Item C</li>
</ol>
```
- `type`: `1` (padrão), `A`/`a` (letras), `I`/`i` (romanos)
- `reversed`: inverte a ordem da contagem

### Não ordenada (`<ul>`)
```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```
- `type`: `disc` (padrão), `circle`, `square`

### Lista de descrição
```html
<dl>
  <dt>Título 1</dt>
  <dd>Descrição do título 1.</dd>
</dl>
```

### Lista aninhada
```html
<ul>
  <li>Item 1</li>
  <li>Item 2
    <ul>
      <li>Item 2.1</li>
      <li>Item 2.2</li>
    </ul>
  </li>
</ul>
```

---

## 📊 Tabelas

```html
<table border="1">
  <caption>Horário Semanal</caption>
  <thead>
    <tr>
      <th>Dia</th>
      <th>Matéria</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td colspan="2">Horários sujeitos a alterações</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>Segunda</td>
      <td>Matemática</td>
    </tr>
  </tbody>
</table>
```

- `<tr>`: linha | `<th>`: cabeçalho (negrito/centralizado) | `<td>`: célula de dado
- `<thead>` / `<tbody>` / `<tfoot>`: agrupam cabeçalho, corpo e rodapé (o `<tfoot>` fica antes do `<tbody>` no código, mas renderiza embaixo)
- `<caption>`: legenda da tabela

### Atributos (usar de preferência via CSS)

| Atributo | Função |
|---|---|
| `cellpadding` | espaço interno da célula |
| `cellspacing` | espaço entre células |
| `colspan` | célula ocupa várias colunas |
| `rowspan` | célula ocupa várias linhas |
| `align` / `valign` | alinhamento horizontal/vertical |
| `bgcolor` | cor de fundo |

---

## 📝 Formulários

```html
<form action="cadastro.php" method="post" enctype="multipart/form-data">

  <fieldset>
    <legend>Dados Pessoais</legend>

    <label for="nome">Nome:</label>
    <input type="text" id="nome" name="nome" required><br>

    <label for="senha">Senha:</label>
    <input type="password" id="senha" name="senha" required><br>

    <label for="foto">Foto:</label>
    <input type="file" id="foto" name="foto" accept="image/*"><br>

    <p>Interesses:</p>
    <input type="checkbox" id="tec" name="interesses[]" value="tecnologia">
    <label for="tec">Tecnologia</label><br>

    <p>Gênero:</p>
    <input type="radio" id="masc" name="genero" value="masculino" checked>
    <label for="masc">Masculino</label><br>

    <label for="estado">Estado:</label>
    <select id="estado" name="estado">
      <optgroup label="Sudeste">
        <option value="sp">São Paulo</option>
        <option value="rj">Rio de Janeiro</option>
      </optgroup>
    </select><br>

    <label for="bio">Bio:</label>
    <textarea id="bio" name="bio" rows="4" cols="30" placeholder="Fale sobre você"></textarea>

  </fieldset>

  <button type="submit">Cadastrar</button>
</form>
```

### `<form>` — atributos principais

| Atributo | Função |
|---|---|
| `action` | URL/servidor que recebe os dados |
| `method="get"` | dados visíveis na URL (padrão) |
| `method="post"` | dados no corpo da requisição, não aparecem na URL |
| `enctype="multipart/form-data"` | obrigatório quando o form tem upload de arquivo |
| `autocomplete="on/off"` | ativa/desativa preenchimento automático |
| `novalidate` | desativa a validação automática do HTML5 |

### Tipos de `<input>`

| Tipo | Uso |
|------|-----|
| `text` | Texto livre |
| `password` | Senha (oculta o texto) |
| `email` / `url` | Valida formato de e-mail/URL |
| `number` | Apenas números |
| `date` / `time` / `datetime-local` | Data e/ou hora |
| `range` | Controle deslizante |
| `color` | Seletor de cor |
| `checkbox` | Múltiplas escolhas |
| `radio` | Uma escolha entre várias (mesmo `name`) |
| `file` | Upload de arquivo (`accept`, `multiple`) |
| `hidden` | Campo invisível ao usuário |
| `submit` | Envia o formulário |
| `reset` | Limpa os campos para o valor inicial |

### Outros campos

- **`<textarea>`**: `rows`, `cols`, `placeholder`, `maxlength`, `readonly`, `disabled`, `required`
- **`<select>` / `<option>`**: `required`, `multiple`, `size`
- **`<optgroup label="...">`**: agrupa opções relacionadas dentro do `<select>`
- **`<fieldset>` + `<legend>`**: agrupa campos relacionados e dá um título ao grupo (melhora acessibilidade)
