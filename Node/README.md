# 📘 Estudos: Node.js, Express, APIs e Bancos de Dados

Resumo simples do curso **NIDAM2026B** (Plataforma Aprenda Mais / MEC).
Feito para quem está começando do zero.

## 📑 Índice

1. [Node.js: o básico](#1--nodejs-o-básico)
2. [APIs e HTTP](#2--apis-e-http)
3. [Express.js](#3--expressjs)
4. [Manipulação de dados e JSON](#4--manipulação-de-dados-e-json)
5. [Banco relacional: MySQL](#5--banco-relacional-mysql)
6. [Banco NoSQL: MongoDB + Mongoose](#6--banco-nosql-mongodb--mongoose)
7. [Testando com REST Client](#7--testando-com-rest-client)
8. [Cola de comandos](#8--cola-de-comandos)
9. [Mini glossário](#9--mini-glossário)

---

## 1. 🟢 Node.js: o básico

### O que é?
Plataforma que roda **JavaScript fora do navegador** (no servidor). Usa o motor **V8** do Chrome, por isso é rápido.

### Instalação
| Item | O que faz |
|---|---|
| **Node.js** | Executa o código JavaScript no servidor |
| **npm** | Gerenciador de pacotes (instala bibliotecas). Vem junto com o Node |
| **Yarn** | Alternativa ao npm |
| **VS Code** | Editor de código recomendado |

```bash
node -v    # mostra a versão do Node instalada
npm -v     # mostra a versão do npm instalada
```

**Extensões úteis do VS Code:** ESLint (acha erros), Prettier (formata o código), Debugger for Node.js.
**Editores online** para testar rápido: CodeSandbox, StackBlitz, CodePen, Replit.

### Como o Node funciona por dentro

| Peça | Explicação simples |
|---|---|
| **Single-thread** | Faz uma coisa de cada vez, em sequência |
| **Call Stack** (pilha) | Lista das funções em execução. A última que entra é a primeira que sai (LIFO) |
| **Memory Heap** | Onde ficam guardados objetos e variáveis na memória |
| **Event Loop** | "Gerente" que vê se a pilha está vazia e manda o próximo item executar |
| **Thread Pool (libuv)** | Equipe de segundo plano que faz tarefas demoradas (ler arquivo, banco de dados) |
| **Task Queue** | Fila onde o resultado (callback) espera a vez de voltar para a pilha |

**Fluxo:** função demorada (ex: `fs.readFile`) → vai para o Thread Pool → Node continua trabalhando → quando termina, o callback entra na fila → Event Loop coloca na pilha quando estiver livre.

> 💡 Por isso o Node é **assíncrono**: não fica parado esperando.

### REPL (terminal interativo do Node)
Serve para **testar código JavaScript linha a linha**.

| Comando | O que faz |
|---|---|
| `node` | Abre o REPL (aparece o `>`) |
| `.help` | Mostra ajuda |
| `.save arquivo.js` | Salva o que você digitou em um arquivo |
| `.load arquivo.js` | Carrega e executa um arquivo |
| `.exit` | Sai (ou `Ctrl + C` duas vezes) |

---

## 2. 🌐 APIs e HTTP

### O que é API?
Conjunto de regras que permite **dois sistemas conversarem**. Exemplo: app de clima pede dados a uma API e mostra na tela.

### Tipos de API

| Tipo | Para que serve |
|---|---|
| **REST** | Usa HTTP e URLs. O mais popular (web e mobile) |
| **GraphQL** | Cliente pede exatamente os dados que precisa |
| **SOAP** | Baseado em XML, muito seguro (bancos, telecom) |
| **gRPC** | Muito rápido, ideal para microsserviços |
| **WebSockets** | Conexão contínua em tempo real (chat, jogos) |

### Princípios do REST
- **Cliente-servidor**: lados independentes
- **Stateless**: cada requisição traz tudo que o servidor precisa (não guarda histórico)
- **Recursos por URL**: ex: `https://api.exemplo.com/usuarios/123`
- **Métodos HTTP** padrão
- **JSON** como formato de dados

### Modelo cliente-servidor
- **Cliente** (navegador/app) → faz a **requisição**
- **Servidor** → processa e devolve a **resposta**
- Vantagens: separação de responsabilidades, escalabilidade, tecnologias diferentes em cada lado

### Métodos HTTP (CRUD)

| Método | Ação | CRUD |
|---|---|---|
| `GET` | Buscar dados | Read |
| `POST` | Criar | Create |
| `PUT` | Atualizar tudo | Update |
| `PATCH` | Atualizar parte | Update |
| `DELETE` | Remover | Delete |

### Partes de uma requisição
- **Método + URL**: `GET /users/123`
- **Headers** (cabeçalhos): `Content-Type`, `Authorization`, `Accept`
- **Body** (corpo): dados enviados (JSON), usado em POST/PUT

### Partes de uma resposta
- **Status code**
- **Headers**: `Content-Type`, `Cache-Control`, `Date`
- **Body**: dados devolvidos

### Códigos de status

| Código | Significado |
|---|---|
| `200` OK | Deu certo |
| `201` Created | Criado com sucesso |
| `204` No Content | Sem conteúdo (ex: excluído) |
| `400` Bad Request | Dados inválidos |
| `401` Unauthorized | Não autenticado |
| `403` Forbidden | Sem permissão |
| `404` Not Found | Não encontrado |
| `500` Internal Server Error | Erro no servidor |

---

## 3. 🚂 Express.js

### O que é?
**Framework** que roda por cima do Node e **facilita criar servidores e APIs**. Criado em 2010 por TJ Holowaychuk.

### Características
- **Minimalista**: só o essencial
- **Não-opinativo**: você escolhe como organizar as pastas
- **Middlewares**: funções que processam a requisição no meio do caminho
- **Rotas flexíveis**
- **View engines** (EJS, Pug, Handlebars) para gerar HTML

| ✅ Vantagens | ⚠️ Desvantagens |
|---|---|
| Simples e flexível | Sem estrutura padrão (precisa se organizar) |
| Comunidade enorme | Muita coisa é configurada manualmente |
| Modular com middlewares | |

### Instalar
```bash
npm init -y          # cria o package.json
npm i express        # instala o Express
```

### Servidor mínimo
```js
const express = require("express");
const app = express();
const PORT = 3000;

app.get("/", (req, res) => {
  res.json({ message: "Olá, Mundo!" });
});

app.listen(PORT, () => {
  console.log(`Servidor rodando em http://localhost:${PORT}`);
});
```

| Linha | O que faz |
|---|---|
| `require("express")` | Importa o Express |
| `express()` | Cria a aplicação |
| `app.get(...)` | Define uma rota |
| `app.listen(...)` | Liga o servidor na porta |
| `res.json()` / `res.send()` | Envia a resposta |

### Estrutura de pastas comum
```
projeto/
├── server.js        # inicia o servidor
├── routes/          # rotas
├── controllers/     # lógica das rotas
├── middlewares/     # middlewares próprios
└── models/          # modelos de dados
```
> Dica: o **Express Application Generator** cria essa estrutura pronta.

### Rotas
**Formato:** `app.METODO(caminho, (req, res) => { ... })`

```js
app.get("/produtos", ...)          // listar
app.post("/produtos", ...)         // criar
app.put("/produtos/:id", ...)      // atualizar
app.delete("/produtos/:id", ...)   // excluir
```

### Tipos de caminho

| Tipo | Exemplo | Resultado |
|---|---|---|
| Simples | `/sobre` | Só `/sobre` |
| Padrão `?` | `/ab?cd` | `acd` e `abcd` |
| Padrão `*` | `/ab*cd` | `abcd`, `ab123cd`... |
| Regex | `/item/:codigo([0-9]{5})` | Só 5 dígitos |
| Parâmetro | `/produtos/:id` | `/produtos/123` |
| Vários parâmetros | `/usuarios/:usuarioId/posts/:postId` | `/usuarios/45/posts/67` |
| Opcional | `/produtos/:id?` | `/produtos` e `/produtos/5` |

### Objetos do handler `(req, res, next)`
| Objeto | Para que serve |
|---|---|
| `req` | Dados da requisição (params, query, body, headers) |
| `res` | Monta e envia a resposta |
| `next` | Passa para o próximo middleware/rota |

### Parâmetros de rota vs Query string

| | Parâmetro de rota | Query string |
|---|---|---|
| Aparência | `/usuarios/123` | `/produtos?categoria=eletronicos&precoMaximo=1000` |
| Definição | `:id` na rota | Depois do `?` |
| Leitura | `req.params.id` | `req.query.categoria` |
| Uso típico | Identificar um recurso | Filtros e opções |

```js
app.get("/usuarios/:id", (req, res) => {
  const { id } = req.params;
  res.json({ message: `Buscando o usuário com ID: ${id}` });
});

app.get("/produtos", (req, res) => {
  const { categoria, precoMaximo } = req.query;
  res.json({ categoria: categoria || "Todas", precoMaximo: precoMaximo || "Sem limite" });
});
```

### Middlewares
Função que roda **entre a requisição e a resposta**. Usos: login, logs, validação, erros.

**Regra de ouro:** chame `next()` para continuar, ou responda (`res.send/json`) para encerrar.

| Tipo | Onde vale |
|---|---|
| **De aplicação** | Todas as rotas (`app.use(...)`) |
| **De rota** | Só uma rota específica |
| **Embutido** | Já vem no Express |
| **De terceiros** | Pacotes externos (ex: morgan) |
| **De erro** | Trata erros |

**Exemplo: middleware de login numa rota**
```js
const checkAuth = (req, res, next) => {
  const autenticado = true;
  if (autenticado) next();
  else res.status(401).json({ message: "Acesso negado" });
};

app.get("/dashboard", checkAuth, (req, res) => {
  res.json({ message: "Bem-vindo ao painel" });
});
```

**Exemplo: log global**
```js
app.use((req, res, next) => {
  console.log(`${req.method} - ${req.url}`);
  next();
});
```

**Middlewares embutidos**
| Comando | O que faz |
|---|---|
| `app.use(express.json())` | Lê JSON enviado no body |
| `app.use(express.urlencoded({ extended: true }))` | Lê dados de formulário |

**Morgan (terceiros): registra cada requisição**
```bash
npm i morgan
```
```js
const morgan = require("morgan");
app.use(morgan("dev"));
// Saída: GET / 200 2.275 ms - 15
```

### View engine (EJS)
```js
app.set("view engine", "ejs");
app.get("/home", (req, res) => {
  res.render("home", { title: "Página Inicial" });
});
```

---

## 4. 📦 Manipulação de dados e JSON

### Módulos nativos do Node
| Módulo | Para que serve |
|---|---|
| `fs` | Ler e escrever arquivos |
| `http` | Criar servidor / lidar com requisições |
| `querystring` | Processar query strings |

```js
const fs = require("fs");

fs.writeFile("dados.txt", "Olá, Node.js!", (err) => {
  if (err) return console.error(err);
  console.log("Arquivo salvo!");
});

fs.readFile("dados.txt", "utf-8", (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});
```

### JSON
Formato **leve e padrão** para trocar dados entre sistemas.
```json
{ "nome": "João", "idade": 25, "hobbies": ["leitura", "esportes"] }
```

| Comando | O que faz |
|---|---|
| `JSON.stringify(obj)` | Objeto JavaScript → texto JSON |
| `JSON.parse(texto)` | Texto JSON → objeto JavaScript |

Vantagens: simples, nativo no JavaScript, funciona com outras linguagens.

---

## 5. 🐬 Banco relacional: MySQL

**Banco relacional** = dados em **tabelas** (linhas e colunas), consultados com **SQL**.

### Instalar o conector
```bash
npm i mysql2
```
`mysql2` conecta o Node ao **MySQL e MariaDB** (aceita Promises).

### Conexão
```js
const mysql = require("mysql2");

const connection = mysql.createConnection({
  host: "localhost",
  user: "seu_usuario",
  password: "sua_senha",
  database: "exemplo_db",
});

connection.connect((err) => {
  if (err) return console.error("Erro ao conectar:", err);
  console.log("Conectado ao MySQL com sucesso!");
});

module.exports = connection;
```

| Função | O que faz |
|---|---|
| `mysql.createConnection()` | Cria a conexão com os dados informados |
| `connection.connect()` | Tenta conectar |
| `connection.query(sql, valores, callback)` | Executa um comando SQL |
| `connection.end()` | Fecha a conexão |

### Estrutura do projeto
```
node-banco/
├── config/
│   └── db.js          # conexão com o banco
├── routes/
│   └── usuarios.js    # rotas CRUD
├── package.json
└── server.js          # ponto de entrada
```

### CRUD com MySQL (`routes/usuarios.js`)
| Rota | SQL | Ação |
|---|---|---|
| `GET /usuarios` | `SELECT * FROM usuarios` | Listar |
| `POST /usuarios` | `INSERT INTO usuarios (nome, idade) VALUES (?, ?)` | Criar |
| `PUT /usuarios/:id` | `UPDATE usuarios SET nome = ?, idade = ? WHERE id = ?` | Atualizar |
| `DELETE /usuarios/:id` | `DELETE FROM usuarios WHERE id = ?` | Excluir |

> 🔒 Os `?` evitam **SQL Injection** (ataque comum). Sempre passe os valores no array.

```js
router.post("/", (req, res) => {
  const { nome, idade } = req.body;
  const sql = "INSERT INTO usuarios (nome, idade) VALUES (?, ?)";
  connection.query(sql, [nome, idade], (err) => {
    if (err) return res.status(500).send("Erro ao inserir usuário");
    res.status(201).send("Usuário inserido com sucesso");
  });
});
```

### `server.js`
```js
const express = require("express");
const app = express();
const usuariosRoutes = require("./routes/usuarios");

app.use(express.json());
app.use("/usuarios", usuariosRoutes);

app.listen(3000, () => console.log("Servidor rodando em http://localhost:3000"));
```

> ⚠️ No material do curso, o `router.post` termina com `);` — o correto é `});`.

---

## 6. 🍃 Banco NoSQL: MongoDB + Mongoose

### O que é NoSQL?
"**Not Only SQL**". Bancos **sem tabelas fixas**, flexíveis e fáceis de escalar.

### MongoDB
Banco **orientado a documentos**. Guarda dados em **JSON/BSON** (BSON = JSON em binário).

**Hierarquia:** Banco de dados → Coleções → Documentos

### Comparação com SQL

| Relacional (MySQL) | MongoDB |
|---|---|
| Tabela | **Coleção** |
| Linha | **Documento** |
| Coluna | **Campo** |
| Chave primária | `_id` (gerado automático) |

### ACID vs BASE

| ACID (relacional) | BASE (NoSQL) |
|---|---|
| Atomicidade, Consistência, Isolamento, Durabilidade | **B**asic Availability: quase sempre disponível |
| Mais seguro, mais lento | **S**oft-state: estado pode mudar sozinho |
| | **E**ventual consistency: fica consistente com o tempo |

**Quando usar NoSQL:** muitos dados, alta performance, escalabilidade horizontal, estrutura que muda muito.

### Ferramentas para gerenciar o MongoDB
| Ferramenta | Observação |
|---|---|
| **MongoDB Compass** | Oficial, visual |
| **MongoDB Shell** | Oficial, linha de comando |
| **MongoDB for VS Code** | Plugin oficial (usado no curso) |
| Robo 3T, Studio 3T, NoSQLBooster, DBeaver | Alternativas |
| Mongo Playground | Simulador sem instalar |

Instalação: baixar o **MongoDB Community Server** no site oficial.

### Criando o banco (MongoDB for VS Code)
1. Clique no ícone de folha 🍃 na barra lateral
2. Clique em **Connect** e informe a URI:
```
mongodb://localhost:27017
mongodb://username:password@localhost:27017   # com senha
```
3. Botão direito em `localhost` → **Add Database** → rode:
```js
const database = 'exemplo_nosql';
const collection = 'usuario';

use(database);                        // cria/seleciona o banco
db.createCollection(collection);      // cria a coleção
```

### Mongoose
Biblioteca **ODM** (Object Data Modeling) que facilita usar MongoDB no Node.

| Recurso | O que faz |
|---|---|
| **Schema** | Define a estrutura dos dados |
| **Validação** | Regras direto no schema |
| **Middlewares** | Ações antes/depois de `save`, `find` |
| **Métodos personalizados** | Lógica própria nos modelos |
| **Population** | Relaciona documentos (parecido com JOIN) |

### Instalar e conectar
```bash
npm i mongoose
```
```js
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect("mongodb://localhost:27017/exemplo_nosql");
    console.log("Conectado ao MongoDB com sucesso!");
  } catch (error) {
    console.error("Erro ao conectar:", error);
    process.exit(1);   // encerra a aplicação se falhar
  }
};
module.exports = connectDB;
```

### Estrutura do projeto (padrão MVC simplificado)
```
node-nosql/
├── config/db.js                      # conexão
├── models/Usuario.js                 # modelo (schema)
├── routes/usuarioRoutes.js           # rotas
├── controllers/usuarioController.js  # lógica
├── package.json
└── server.js                         # ponto de entrada
```

### Model (`models/Usuario.js`)
```js
const usuarioSchema = new mongoose.Schema({
  nome:  { type: String, required: true, trim: true },
  idade: { type: Number, required: true, min: 0 },
  email: { type: String, required: true, unique: true, match: /.+\@.+\..+/ },
});
module.exports = mongoose.model("Usuario", usuarioSchema);
```

| Opção | Significado |
|---|---|
| `type` | Tipo do dado |
| `required` | Obrigatório |
| `trim` | Remove espaços nas pontas |
| `min` | Valor mínimo |
| `unique` | Não pode repetir |
| `match` | Precisa seguir um padrão (regex) |

### Comandos do Mongoose (CRUD)

| Comando | O que faz |
|---|---|
| `new Usuario(req.body)` + `.save()` | **Cria** um documento |
| `Usuario.find()` | **Busca** todos |
| `Usuario.findByIdAndUpdate(id, dados, { new: true })` | **Atualiza** e devolve o novo valor |
| `Usuario.findByIdAndDelete(id)` | **Exclui** |

### Controller (resumo)
```js
exports.criarUsuario = async (req, res) => {
  try {
    const usuario = new Usuario(req.body);
    await usuario.save();
    res.status(201).json(usuario);
  } catch (error) {
    res.status(400).json({ message: "Erro ao criar usuário", error });
  }
};
```
> `async/await` = espera a operação terminar sem travar o servidor. `try/catch` = trata erros.

### Rotas (`usuarioRoutes.js`)
```js
router.post("/", usuarioController.criarUsuario);
router.get("/", usuarioController.obterUsuarios);
router.put("/:id", usuarioController.atualizarUsuario);
router.delete("/:id", usuarioController.deletarUsuario);
```

### `server.js`
```js
const express = require("express");
const connectDB = require("./config/db");
const usuarioRoutes = require("./routes/usuarioRoutes");

const app = express();
connectDB();                          // conecta ao MongoDB
app.use(express.json());              // lê JSON
app.use("/usuarios", usuarioRoutes);  // liga as rotas

app.listen(3000, () => console.log("Servidor rodando em http://localhost:3000"));
```

---

## 7. 🧪 Testando com REST Client

Extensão do **VS Code** para testar a API sem sair do editor.

**Passos**
1. Instale a extensão **REST Client**
2. Crie a pasta `request/` e dentro dela `testes.rest`
3. Escreva as requisições
4. Clique em **Send Request** (aparece acima de cada uma)

```http
@baseURL = http://localhost:3000

### Criar (CREATE)
POST {{baseURL}}/usuarios
Content-Type: application/json

{
  "nome": "Carlos",
  "idade": 28
}

### Listar (READ)
GET {{baseURL}}/usuarios

### Atualizar (UPDATE)
PUT {{baseURL}}/usuarios/1
Content-Type: application/json

{
  "nome": "Carlos Silva",
  "idade": 29
}

### Excluir (DELETE)
DELETE {{baseURL}}/usuarios/1
```

| Símbolo | Significado |
|---|---|
| `@baseURL = ...` | Cria uma **variável** |
| `{{baseURL}}` | Usa a variável |
| `###` | Separa uma requisição da outra |

> Com MongoDB, o `email` é obrigatório no model. Inclua-o no body do POST.

---

## 8. ⚡ Cola de comandos

### Terminal
| Comando | O que faz |
|---|---|
| `node -v` | Versão do Node |
| `npm -v` | Versão do npm |
| `node` | Abre o REPL |
| `node server.js` | Executa o servidor |
| `npm init -y` | Cria `package.json` |
| `npm i express` | Instala o Express |
| `npm i mysql2` | Instala o conector MySQL |
| `npm i mongoose` | Instala o Mongoose |
| `npm i morgan` | Instala o Morgan (logs) |
| `npm i nodemon -D` | Reinicia o servidor ao salvar (usado no curso) |

### Express
| Código | O que faz |
|---|---|
| `app.get / post / put / delete` | Define rotas |
| `app.use(...)` | Aplica middleware |
| `app.listen(PORT, ...)` | Liga o servidor |
| `req.params.id` | Lê parâmetro da rota |
| `req.query.nome` | Lê query string |
| `req.body` | Lê o corpo (precisa de `express.json()`) |
| `res.send()` / `res.json()` | Responde |
| `res.status(404).json(...)` | Responde com código de status |
| `next()` | Passa para o próximo middleware |
| `express.Router()` | Cria um grupo de rotas modular |

### Mongoose
| Código | O que faz |
|---|---|
| `mongoose.connect(uri)` | Conecta ao MongoDB |
| `new mongoose.Schema({...})` | Define a estrutura |
| `mongoose.model("Nome", schema)` | Cria o modelo |
| `.save()` / `.find()` / `.findByIdAndUpdate()` / `.findByIdAndDelete()` | CRUD |

---

## 9. 📖 Mini glossário

| Termo | Significado simples |
|---|---|
| **API** | Ponte que permite sistemas conversarem |
| **Endpoint** | URL + método (ex: `GET /produtos`) |
| **Rota** | Caminho que a aplicação sabe responder |
| **Middleware** | Função que atua no meio da requisição |
| **Callback** | Função executada quando algo termina |
| **Assíncrono** | Faz outras coisas enquanto espera |
| **Framework** | Conjunto de ferramentas prontas que facilita programar |
| **JSON** | Formato de dados em texto, fácil de ler |
| **CRUD** | Create, Read, Update, Delete |
| **Schema** | "Molde" que define como o dado deve ser |
| **ODM** | Tradutor entre objetos JavaScript e MongoDB |
| **URI** | Endereço de conexão com o banco |
| **Porta (3000)** | "Porta de entrada" onde o servidor escuta |
| **localhost** | Seu próprio computador |

---

📚 *Fonte: curso NIDAM2026B, Plataforma Aprenda Mais (MEC).*
