
# Tutorial Passo a Passo: Configurando um Projeto Node.js com TypeScript no Codespaces
### Objetivo: Você vai aprender a criar um servidor Node.js com TypeScript que realiza operações de banco de dados e possui uma interface web simples para interagir com os dados.

### Passo 1: Criar um Repositório no GitHub e Iniciar Codespaces

1- Crie um repositório no GitHub:
Acesse o GitHub e faça login.
Clique no botão "New" no canto superior direito para criar um novo repositório.

2- Nomeiei o repositório como Projetin.

3- Marque a opção "Public".

4- Marque a opção "Add a README file".

5- Clique em "Create repository". 

## passo 2: Inicie o Codespaces:

No seu repositório recém-criado, clique no botão "Code".
Clique em "Codespaces" e depois em "Create Codespace on main".
Espere o ambiente Codespaces carregar.

## Passo 3: Configurar o Ambiente de Desenvolvimento

Iniciar o projeto Node.js:
No terminal que se abriu automaticamente no Codespaces, digite o seguinte comando para criar o projeto:

````bash
npm init -y
````

Esse comando cria um arquivo package.json com as configurações básicas do projeto.
Instalar dependências do projeto:
Ainda no terminal, instale as dependências necessárias:

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
2- Editando um usuário Adicione a rota /users/:id ao servidor. adicione ao app.ts copiar código

````bash
app.put('/users/:id', async (req, res) => {
  const db = await connect();
  const { name, email } = req.body;
  const { id } = req.params;

  await db.run('UPDATE users SET name = ?, email = ? WHERE id = ?', [name, email, id]);
  const user = await db.get('SELECT * FROM users WHERE id = ?', [id]);

  res.json(user);
});
````
3- Deletando um usuário Adicione a rota /users/:id ao servidor. adicione ao app.ts copiar código

````bash
app.delete('/users/:id', async (req, res) => {
  const db = await connect();
  const { id } = req.params;

  await db.run('DELETE FROM users WHERE id = ?', [id]);

  res.json({ message: 'User deleted' });
});
````
## Passo 17: Criar a Pasta Public e Adicionar Arquivos

1- Criar a Pasta Public e dentro dela um arquivo  **index.html**

# Dentro de ìndex.html adicione: 

````html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <form>
    <input type="text" name="name" placeholder="Nome">
    <input type="email" name="email" placeholder="Email">
    <button type="submit">Cadastrar</button>
  </form>

  <table>
    <thead>
      <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Email</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody>
      <!--  -->
    </tbody>
  </table>

  <script>
    // 
    const form = document.querySelector('form')

    form.addEventListener('submit', async (event) => {
      event.preventDefault()

      const name = form.name.value
      const email = form.email.value

      await fetch('/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name, email })
      })

      form.reset()
      fetchData()
    })

    // 
    const tbody = document.querySelector('tbody')

    async function fetchData() {
      const resp = await fetch('/users')
      const data = await resp.json()

      tbody.innerHTML = ''

      data.forEach(user => {
        const tr = document.createElement('tr')
        tr.innerHTML = `
          <td>${user.id}</td>
          <td>${user.name}</td>
          <td>${user.email}</td>
          <td>
            <button class="excluir">excluir</button>
            <button class="editar">editar</button>
          </td>
        `

        const btExcluir = tr.querySelector('button.excluir')
        const btEditar = tr.querySelector('button.editar')

        btExcluir.addEventListener('click', async () => {
          await fetch(`/users/${user.id}`, { method: 'DELETE' })
          tr.remove()
        })

        btEditar.addEventListener('click', async () => {
          const name = prompt('Novo nome:', user.name)
          const email = prompt('Novo email:', user.email)

          await fetch(`/users/${user.id}`, {
            method: 'PUT',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ name, email })
          })

          fetchData()
        })

        tbody.appendChild(tr)
      })
    }

    fetchData()
  </script>
</body>

</html>
````

# Finalizou :)

# Parabéns!. Você configurou com sucesso um projeto Node.js com TypeScript, Express, CORS e SQLite
