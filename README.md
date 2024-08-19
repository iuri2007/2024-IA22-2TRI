## Passo 4: Instale as Dependências


1- Ainda no terminal, instale o Express, CORS, e SQLite3 com o seguinte comando:


````bash
npm install express cors sqlite3
````


## Passo 5: Instale as Dependências de Desenvolvimento


1- Instale as dependências necessárias para o desenvolvimento com TypeScript:


````bash
npm install --save-dev typescript nodemon ts-node @types/express @types/cors @types/sqlite3
````


## Passo 6: Inicialize o TypeScript


1- Configure o TypeScript no projeto:


````bash
npx tsc --init
````


## Passo 7: Crie a Estrutura de Diretórios e Arquivos


1- Crie o diretório src e o arquivo src/app.ts:


````bash
mkdir src
touch src/app.ts
````


## Passo 8: Configure o tsconfig.json


### Abra o arquivo tsconfig.json e faça as seguintes modificações:


1- Mude a linha "outDir": "./", para "outDir": "./dist",.


2- Adicione a linha "rootDir": "./src",.


# O arquivo tsconfig.json deve ficar assim:


````bash
{
  "compilerOptions": {
    "target": "ES2017",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
````
## Passo 9: Configure o package.json


1- Abra o arquivo package.json e adicione o seguinte script na seção "scripts":


````bash
"scripts": {
  "dev": "nodemon src/app.ts"
}
````


## Passo 10: Crie o Arquivo Inicial do Servidor:


1- No arquivo src/app.ts, adicione o seguinte código:


````bash
// Importando os módulos necessários
import express from 'express';
import cors from 'cors';


// Definindo a porta em que o servidor irá rodar
const port = 3333;


// Criando uma aplicação Express
const app = express();


// Usando o middleware CORS para permitir requisições de diferentes origens
app.use(cors());


// Usando o middleware para fazer o parse do corpo das requisições em JSON
app.use(express.json());


// Definindo uma rota GET na raiz do servidor que retorna "Hello World"
app.get('/', (req, res) => {
  res.send('Hello World');
});


// Iniciando o servidor na porta especificada
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
````
## Passo 11: Inicialize o Servidor


1- No terminal, execute o servidor com o comando:


````bash
npm run dev
````


# Se tudo estiver correto, você verá a mensagem Server running on port 3333 no terminal.


## Passo 12: Teste o Servidor


1- Abra seu navegador e acesse http://localhost:3333. Você deverá ver a mensagem Hello World


## Passo 13: Configure o Banco de Dados


1- Crie um arquivo src/database.ts e adicione o seguinte código:


````bash
import { open, Database } from 'sqlite';
import sqlite3 from 'sqlite3';


// Variável para armazenar a instância do banco de dados
let instance: Database | null = null;


// Função para conectar ao banco de dados
export async function connect() {
  // Retorna a instância do banco de dados se já estiver conectada
  if (instance) return instance;


  // Abre uma conexão com o banco de dados SQLite
  const db = await open({
    filename: './src/database.sqlite',
    driver: sqlite3.Database
  });
 
  // Cria a tabela 'users' se ela não existir
  await db.exec(`
    CREATE TABLE IF NOT EXISTS users (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      name TEXT,
      email TEXT
    )
  `);


  // Armazena a instância do banco de dados
  instance = db;
  return db;
}
````


## Passo 14: Adicione o Banco de Dados ao Servidor


1- No arquivo src/app.ts, adicione as seguintes importações e código:


````typescript
// Importando o módulo connect do arquivo database.ts
import { connect } from './database';


// Definindo uma rota POST para adicionar um usuário ao banco de dados
app.post('/users', async (req, res) => {
  // Conectando ao banco de dados
  const db = await connect();
  // Extraindo os dados do corpo da requisição
  const { name, email } = req.body;


  // Inserindo um novo usuário no banco de dados
  const result = await db.run('INSERT INTO users (name, email) VALUES (?, ?)', [name, email]);
  // Obtendo o usuário inserido
  const user = await db.get('SELECT * FROM users WHERE id = ?', [result.lastID]);


  // Retornando o usuário inserido como resposta
  res.json(user);
});
````


## Passo 15: Teste a Inserção de Dados


1- Use o Postman para fazer uma requisição POST para http://localhost:3333/users com o seguinte corpo:


````bash
{
  "name": "John Doe",
  "email": "john@example.com"
}
````


2- Se tudo estiver correto, você verá a resposta com o usuário inserido:


````bash
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com"
}
````


## Passo 16: Liste os Usuários


1- Adicione a rota /users no servidor:


````bash
// Definindo uma rota GET para listar todos os usuários
app.get('/users', async (req, res) => {
  // Conectando ao banco de dados
  const db = await connect();
  // Obtendo todos os usuários do banco de dados
  const users = await db.all('SELECT * FROM users');


  // Retornando a lista de usuários como resposta
  res.json(users);
});
````


# Finalização


1- Você configurou com sucesso um projeto Node.js com TypeScript, Express, CORS e SQLite
