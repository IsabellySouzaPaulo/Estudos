ESTRUTURA

<html>
<head></head>
<body>
<header> <!-- CABECALHO -->
  <nav></nav> <!-- BARRA DE NAVEGACAO -->
</header>
<main>
  <section></section> <!-- SECOES-->
</main> <!-- CONTEUDO PRINCIPAL -->
<footer></footer> <!-- RODAPÉ-->
</body>
</html>


TAGS PRINCIPAIS

<link rel="shortcut icon" href="img/icone.png" type="image/x-icon"> <!-- ICONE DO SITE -->
<img src="img/foto.png" alt="FOTO"> <!-- ADICIONAR IMAGEM (o alt é a decricao da imagem) -->
<button class="menu"> Ola </button> <!-- BOTAO (class é o nome que vai chamar no css) -->
<h1> Oi </h1> <!-- TITULOS - Vao de h1 a h5 -->
<p> Tudo bem </p> <!-- PARAGRAFO - caso coloque "<a href="nome.html" rel="next">" dentro, direciona pra outra pagina ao clicar -->
<a href="https://google.com" target="_blank" rel="external" >Clique aqui para ir ao Google</a> <!-- LINKS (abre um link externo sem sobrepor o seu site
)-->
<br> <!-- QUEBRA LINHA -->
<hr> <!-- CRIA UMA LINHA HORIZONTAL -->


FORMATACOES

<b>Texto em negrito</b><br>
<i>Texto em itálico</i><br>
<u>Texto sublinhado</u><br>
<sub>Texto subscrito</sub><br>
<sup>Texto sobrescrito</sup><br>
<big>Texto com fonte maior do que o padrão</big><br>
<small>Texto com fonte menor do que o padrão</small><br>
<em>Texto em itálico</em><br>
<strong>Texto em negrito</strong>

LISTAS 

<!-- Lista ordenada -->
<ol>
<li>Primeiro elemento</li>
<li>Segundo elemento</li>
<li>Terceiro elemento</li>
</ol>
<!-- Lista não ordenada -->
<ul>
<li>Primeiro elemento</li>
<li>Segundo elemento</li>
<li>Terceiro elemento</li>
</ul>

FORMULARIOS

<form action="cadastro.php" method="post">
  <label for="nome">Nome:</label>
  <input type="text" id="nome" name="nome" required><br><br>
    
  <label for="cpf">CPF:</label>
  <input type="text" id="cpf" name="cpf" required><br><br>

  <label for="senha">Senha:</label>
  <input type="password" id="senha" name="senha" required><br><br>

  <!-- SELEÇÃO: Checkbox (Múltiplas escolhas - Ex: Interesses) -->
  <p>Interesses:</p>
  <input type="checkbox" id="tec" name="interesses[]" value="tecnologia">
  <label for="tec">Tecnologia</label>
  <input type="checkbox" id="esporte" name="interesses[]" value="esportes">
  <label for="esporte">Esportes</label><br><br>

  <!-- SELEÇÃO: Radio (Apenas uma escolha - Ex: Gênero) -->
  <p>Gênero:</p>
  <input type="radio" id="masc" name="genero" value="masculino">
  <label for="masc">Masculino</label>
  <input type="radio" id="fem" name="genero" value="feminino">
  <label for="fem">Feminino</label><br><br>

  <!-- SELEÇÃO: Select (Lista suspensa para poupar espaço - Ex: Estado) -->
  <label for="estado">Estado:</label>
  <select id="estado" name="estado">
    <option value="sp">São Paulo</option>
    <option value="rj">Rio de Janeiro</option>
    <option value="mg">Minas Gerais</option>
  </select><br><br>

  <button type="submit">Cadastrar</button>
</form>

