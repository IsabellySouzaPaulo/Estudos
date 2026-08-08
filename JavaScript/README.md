# ⚡ Anotações de JavaScript

> Bibliotecas úteis: [GSAP](https://gsap.com/docs/v3/Installation/) · [SoapUI Script Library](https://www.soapui.org/docs/scripting-and-properties/script-library/)

---

## 💬 Saída de texto

```js
alert("Olá");                          // mostra um popup com botão OK
window.prompt("Qual seu nome?");       // popup com campo pro usuário digitar
document.write("texto");               // escreve direto no documento
document.writeln("texto");             // escreve e pula linha
console.log("texto");                  // mostra no console (DevTools)
```

---

## 📦 Variáveis

```js
let nome = "Isa";        // pode ser reatribuída
var n = 6;                // forma antiga, evite usar
const numero = 321328;    // não pode ser reatribuída

var nome = window.prompt("Qual seu nome?"); // guarda o que o usuário digitar
```
> 💡 Prefira `let` e `const` — `var` é a forma antiga do JS e tem comportamento menos previsível.

---

## 🔤 Tipos de dados

| Tipo | Exemplo |
|---|---|
| string | `"Isa"` |
| inteiro/número | `42` |
| boolean | `let ativo = true;` |
| array (lista) | `let frutas = ["maçã", "banana"];` |
| object | `let pessoa = { nome: "Isa", idade: 99 };` |

---

## 🔗 Concatenação

```js
window.alert("Olá " + nome);        // usando +
window.alert(`Meu nome é ${nome}`); // template string (crase + ${})
```
> 💡 Template string (` `` `) é mais legível que ficar somando `+` toda hora.

---

## 🔢 Formatação de números e texto

```js
var s = "Isabelly";
var n1 = 2321.5;

s.length;              // conta a quantidade de caracteres
s.toUpperCase();       // TUDO MAIÚSCULO
s.toLowerCase();       // tudo minúsculo
n1.toFixed(2);         // formata com 2 casas decimais → "2321.50"

Number("42");              // converte texto pra número
Number.parseInt("42");     // converte pra número inteiro
Number.parseFloat("4.5");  // converte pra número com casas decimais
```

---

## ➕ Operadores

**Aritméticos**
```js
+  -  *  /  %(resto)  **(potência)
```

**Atribuição**
```js
=   += (soma e atribui)   -=   *=   /=   %=
```

**Relacionais**
```js
>   <   >=   <=
==  // igual (só compara o valor)
=== // estritamente igual (valor E tipo)
!=  // diferente
!== // estritamente diferente
```

**Lógicos**
```js
&&  // E (AND) — as duas condições precisam ser verdadeiras
||  // OU (OR) — pelo menos uma precisa ser verdadeira
!   // NÃO (NOT) — inverte o valor
```

**Ternário** (if/else resumido em uma linha)
```js
condição ? valorSeVerdadeiro : valorSeFalso;

// exemplo
let idade = 20;
let status = idade >= 18 ? "maior" : "menor";
```

---

## 🌳 DOM e Eventos

DOM = a estrutura da página em forma de objetos que o JS consegue acessar e manipular.

**Eventos mais usados** ([lista completa MDN](https://developer.mozilla.org/pt-BR/docs/Web/API/Event))

| Evento | Quando dispara |
|---|---|
| `click` | ao clicar no elemento |
| `mouseenter` | quando o mouse entra na área |
| `mouseout` | quando o mouse sai da área |
| `mousemove` | enquanto o mouse se move em cima |
| `mousedown` | ao pressionar o botão do mouse |
| `mouseup` | ao soltar o botão do mouse |

### Exemplo prático (HTML + JS)
```html
<div id="area" onclick="clicar()" onmouseenter="entrar()" onmouseout="sair()">
  Interaja...
</div>

<script>
  var a = window.document.getElementById('area');

  function clicar() {
    a.innerText = 'Clicou!';
    a.style.background = 'red';
  }
  function entrar() {
    a.innerText = 'Entrou!';
  }
  function sair() {
    a.innerText = 'Saiu!';
    a.style.background = 'green';
  }
</script>
```

### Exemplo: somar dois valores de inputs
```html
<h1>Somando Valores</h1>
<input type="number" id="txtn1"> +
<input type="number" id="txtn2">
<input type="button" value="Somar" onclick="somar()">
<div id="res">Resultado</div>

<script>
  function somar() {
    var tn1 = window.document.getElementById('txtn1');
    var tn2 = window.document.querySelector('input#txtn2'); // outra forma de selecionar
    var res = window.document.getElementById('res');

    var n1 = Number(tn1.value);
    var n2 = Number(tn2.value);
    var s = n1 + n2;

    res.innerHTML = `A soma entre ${n1} e ${n2} é igual a <strong>${s}</strong>`;
  }
</script>
```
> 💡 `getElementById` busca por id. `querySelector` busca com sintaxe de CSS (`'input#txtn2'`, `'.classe'` etc) — mais flexível.

---

## 🔀 Condições

```js
if (condição) {
  // ...
} else {
  // ...
}

switch (variavel) {
  case 'valor1':
    // ...
    break;
  case 'valor2':
    // ...
    break;
  default:
    // ...
}
```
> ⚠️ Não esquecer o `break` em cada `case`, senão o código continua executando os próximos casos.

---

## 🔁 Repetições (loops)

```js
// Enquanto (testa antes de rodar)
var c = 1;
while (c <= 10) {
  // ...
  c++; // c++ = incrementa 1 (c = c + 1). Também existe c-- (decrementa 1)
}

// Faça-enquanto (roda pelo menos 1 vez, testa depois)
do {
  // ...
} while (condição);

// Para (mais usado quando já se sabe quantas vezes repetir)
for (var c = 1; c <= 10; c++) {
  // ...
}
```
- **`while`**: início e incremento ficam fora do parênteses, soltos no código
- **`for`**: início, teste e incremento ficam juntos: `for(início; teste; incremento)`

---

## 📚 Arrays

Array é uma **lista de valores**, cada um com uma posição (índice), começando do **0**.

```js
let num = [5, 8, 4];
//          0  1  2   ← índices

num[0];           // 5   (acessa o valor da posição 0)
num[3] = 6;        // adiciona/altera a posição 3 → [5, 8, 4, 6]
num.push(7);       // adiciona no final → [5, 8, 4, 6, 7]
num.length;        // quantidade de itens no array (5)
num.sort();        // ordena os itens
```

### Outros métodos úteis de array
```js
let frutas = ["banana", "maçã", "uva"];

frutas.pop();               // remove o último item
frutas.shift();             // remove o primeiro item
frutas.unshift("pera");     // adiciona no início
frutas.indexOf("maçã");     // retorna a posição do item (1)
frutas.includes("uva");     // true/false, se o item existe no array
frutas.join(", ");          // transforma em texto: "banana, maçã, uva"
frutas.reverse();           // inverte a ordem do array
```

### Percorrendo um array
```js
let numeros = [10, 20, 30];

// com for tradicional
for (let i = 0; i < numeros.length; i++) {
  console.log(numeros[i]);
}

// com forEach (mais usado)
numeros.forEach(function(item) {
  console.log(item);
});

// com for...of (mais moderno e legível)
for (let item of numeros) {
  console.log(item);
}
```

### Array de objetos (muito comum na prática)
```js
let alunos = [
  { nome: "Isa", nota: 9 },
  { nome: "João", nota: 7 }
];

alunos[0].nome;    // "Isa"
alunos.forEach(function(aluno) {
  console.log(aluno.nome + " tirou " + aluno.nota);
});
```

---

## 🧩 Objetos

```js
let amigo = {
  nome: 'José',
  sexo: 'M',
  peso: 85.4,
  engordar(p) { /* ... */ }   // objeto pode ter função dentro (método)
};

amigo.nome;         // "José" — acessa propriedade com ponto
amigo.peso;         // 85.4
amigo.engordar(2);  // chama o método
```

---

## 🔧 Funções

```js
function parimpar(n) {
  if (n % 2 == 0) {
    return 'Par!';
  } else {
    return 'Ímpar!';
  }
}

console.log(parimpar(223)); // "Ímpar!"
```
- `function nome(parâmetros) { }` — declara a função
- `return` — devolve um valor e encerra a função

---

## 📍 Próximos tópicos pra estudar

Continuação natural do conteúdo (aprofundar quando for revisar):

- **Functions** (funções anônimas, arrow functions `=>`, callbacks)
- **Objetos** (mais a fundo — métodos, `this`, protótipos)
- **Modularização** (dividir o código em vários arquivos com `import`/`export`)
- **RegEx** (expressões regulares — validar formatos de texto, tipo e-mail)
- **JSON** (formato de dados usado pra troca de informação entre sistemas)
- **AJAX** (fazer requisições sem recarregar a página)
- **NodeJS** (rodar JavaScript fora do navegador, no servidor)
