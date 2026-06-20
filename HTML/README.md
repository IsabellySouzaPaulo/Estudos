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

```html
<!-- Lista ordenada (1, 2, 3...) -->
<ol>
  <li>Primeiro elemento</li>
  <li>Segundo elemento</li>
  <li>Terceiro elemento</li>
</ol>

<!-- Lista não ordenada (bullets •) -->
<ul>
  <li>Primeiro elemento</li>
  <li>Segundo elemento</li>
  <li>Terceiro elemento</li>
</ul>
```

---

## 📝 Formulários

```html
<form action="cadastro.php" method="post">

  <!-- Texto -->
  <label for="nome">Nome:</label>
  <input type="text" id="nome" name="nome" required><br><br>

  <!-- CPF -->
  <label for="cpf">CPF:</label>
  <input type="text" id="cpf" name="cpf" required><br><br>

  <!-- Senha -->
  <label for="senha">Senha:</label>
  <input type="password" id="senha" name="senha" required><br><br>

  <!-- Checkbox (múltiplas escolhas) -->
  <p>Interesses:</p>
  <input type="checkbox" id="tec" name="interesses[]" value="tecnologia">
  <label for="tec">Tecnologia</label>

  <input type="checkbox" id="esporte" name="interesses[]" value="esportes">
  <label for="esporte">Esportes</label><br><br>

  <!-- Radio (apenas uma escolha) -->
  <p>Gênero:</p>
  <input type="radio" id="masc" name="genero" value="masculino">
  <label for="masc">Masculino</label>

  <input type="radio" id="fem" name="genero" value="feminino">
  <label for="fem">Feminino</label><br><br>

  <!-- Select (lista suspensa) -->
  <label for="estado">Estado:</label>
  <select id="estado" name="estado">
    <option value="sp">São Paulo</option>
    <option value="rj">Rio de Janeiro</option>
    <option value="mg">Minas Gerais</option>
  </select><br><br>

  <button type="submit">Cadastrar</button>

</form>
```

### Tipos de `<input>` mais usados

| Tipo | Uso |
|------|-----|
| `type="text"` | Texto livre |
| `type="password"` | Senha (oculta o texto) |
| `type="email"` | E-mail (valida formato) |
| `type="number"` | Apenas números |
| `type="date"` | Seletor de data |
| `type="checkbox"` | Múltiplas escolhas |
| `type="radio"` | Uma escolha |
| `type="file"` | Upload de arquivo |
| `type="hidden"` | Campo invisível |
| `type="submit"` | Botão de envio |
