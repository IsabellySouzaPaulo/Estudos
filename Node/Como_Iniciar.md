```markdown
# Como Iniciar um Backend Node.js do Zero

Este documento serve como um passo a passo prático para configurar a estrutura base de um servidor backend com Node.js, Express e MongoDB em futuros projetos.

---

## 1. Estrutura de Pastas Inicial

No diretório raiz do teu projeto, certifica-te de organizar o projeto separando o servidor da interface:

seu-projeto/
├── backend/
└── frontend/

```

---

## 2. Passo a Passo no Terminal

### Passo 1: Entrar na pasta do backend

Abre o terminal integrado no teu editor (VS Code) e navega até à pasta do servidor:

```bash
cd backend

```

### Passo 2: Inicializar o projeto Node.js

Cria o ficheiro de configuração `package.json` de forma automática:

```bash
npm init -y

```

### Passo 3: Instalar as dependências essenciais

Instala os pacotes fundamentais para gerir o servidor, rotas e base de dados:

```bash
npm install express mongoose dotenv cors

```

* **`express`**: Framework leve para gerir servidores HTTP e rotas.
* **`mongoose`**: ODM para modelar os dados e comunicar com o MongoDB.
* **`dotenv`**: Para carregar variáveis de ambiente protegidas a partir de um ficheiro `.env`.
* **`cors`**: Permite que o frontend faça requisições ao backend sem bloqueios de segurança do navegador.

---

## 3. Criar o Ficheiro Principal (`server.js`)

Cria um ficheiro chamado `server.js` dentro da pasta `backend` com a seguinte estrutura base:

```javascript
const express = require('express');
const cors = require('cors');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 3000;

// Configuração de Middlewares
app.use(express.json()); // Permite receber dados em formato JSON nas requisições
app.use(cors());         // Libera o acesso para o frontend

// Rota de Teste Inicial
app.get('/', (req, res) => {
    res.json({ status: "Sucesso", mensagem: "O servidor está a funcionar perfeitamente!" });
});

// Ligar o Servidor à Porta Definida
app.listen(PORT, () => {
    console.log(`🚀 Servidor rodando na porta ${PORT}`);
});

```

---

## 4. Automatizar o Arranque no `package.json`

Para conseguires iniciar o servidor de forma simples usando `npm start`, abre o teu ficheiro `package.json` e certifica-te de que a linha de `"scripts"` inclui o comando de arranque:

```json
"scripts": {
  "start": "node server.js"
}

```

---

## 5. Como Executar o Servidor

Sempre que quiseres ligar o teu backend no dia a dia de desenvolvimento:

```bash
npm start

```

```

```
