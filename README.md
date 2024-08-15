# 2024-IA22-2TRI

## iniciando o projetinn


## Passo 1: Criar um Repositório no GitHub e Iniciar Codespaces

 ###  1. Crie um repositório no GitHub:

- Acesse o **GitHub** e faça login.
- Clique no botão **"New"** no canto superior direito para criar um novo.
- De o nome para o repositório como **projetinn**.
- Marque a opção **"Public"**
- Marque a opção **"Add a README file"**.
- Clique em **"Create repository"**.

### 2. Inicie o Codespaces:
- No seu repositório recém-criado, clique no botão **"Code"**.
- Clique em **"Codespaces"** e depois em **"Create Codespace on main"**.
- Espere o ambiente Codespaces carregar.

## Passo 2: Configurar o Ambiente de Desenvolvimento

###  1. Iniciar o projeto Node.js:


No terminal que se abriu automaticamente no Codespaces, digite o seguinte comando para criar o projeto:


````bash
npm init -y
````


- Esse comando cria um arquivo package.json com as configurações básicas do projeto.

###  2. Instalar dependências do projeto:


Ainda no terminal, instale as dependências necessárias:


````bash
npm install express cors sqlite3 sqlite
````


Instale as dependências de desenvolvimento para TypeScript:


````bash
npm install --save-dev typescript nodemon ts-node @types/express @types/cors
````


### 3. Configurar TypeScript:


Inicialize o TypeScript no projeto:


````bash
npx tsc --init
````

- Isso criará um arquivo **tsconfig.json** para configurar o TypeScript.

### 4. Configurar o **tsconfig.json**:

1. Abra o arquivo **tsconfig.json** e faça as seguintes modificações:

- Mude a linha "outDir": "./", para "outDir": "./dist",.

- Adicione a linha "rootDir": "./src", logo a baixo


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
### 5. Criar a estrutura de pastas e arquivos:

No terminal, crie a pasta src e um arquivo **src/app.ts&**:


````bash
 mkdir src
touch src/app.ts
````

## Passo 3:  Criar o Servidor Node.js com TypeScript

### 1. Adicionar código inicial do servidor:

- No arquivo **src/app.ts**, adicione o seguinte código:

````bash
import express from 'express';
import cors from 'cors';

const port = 3333;
const app = express();

app.use(cors());
app.use(express.json());

app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
````
### 2. Configurar o **package.json**:

- Abra o arquivo **package.json** e adicione o seguinte script dentro de **"scripts"**:

````bash
  "dev": "nodemon src/app.ts"
````
### 3. Iniciar o servidor:

- No terminal, inicie o servidor:

````bash
  npm run dev
````

- Se tudo estiver correto, você verá a mensagem Server running on port 3333 no terminal.

## Passo 4: Configurar o Banco de Dados SQLite

### 1. Criar o arquivo de banco de dados:

- Crie um arquivo src/database.ts e adicione o seguinte código:

````bash
import { open, Database } from 'sqlite';
import sqlite3 from 'sqlite3';

let instance: Database | null = null;

export async function connect() {
  if (instance) return instance;

  const db = await open({
    filename: './src/database.sqlite',
    driver: sqlite3.Database
  });

  await db.exec(`
    CREATE TABLE IF NOT EXISTS users (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      name TEXT,
      email TEXT
    )
  `);

  instance = db;
  return db;
}
````
### 2. Adicionar rotas para o banco de dados no servidor:

- No arquivo src/app.ts, substitua o código existente por este:

````bash
import express from 'express';
import cors from 'cors';
import { connect } from './database';

const port = 3333;
const app = express();

app.use(cors());
app.use(express.json());

app.get('/', (req, res) => {
  res.send('Hello World');
});

app.post('/users', async (req, res) => {
  const db = await connect();
  const { name, email } = req.body;

  const result = await db.run('INSERT INTO users (name, email) VALUES (?, ?)', [name, email]);
  const user = await db.get('SELECT * FROM users WHERE id = ?', [result.lastID]);

  res.json(user);
});

app.get('/users', async (req, res) => {
  const db = await connect();
  const users = await db.all('SELECT * FROM users');

  res.json(users);
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
````
### 3. Testar o banco de dados:

- Crie um arquivo teste.http na raiz do projeto com o seguinte conteúdo:

````bash
POST http://localhost:3333/users HTTP/1.1
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}
````

- Clique em "Send Request" acima da linha POST.

1. Se tudo estiver correto, você verá a resposta com o usuário inserido:
json

````bash
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com"
}
````

## 5. Criar Interface Web Simples:

### 1. Finalizar o código da interface web:

- No arquivo public/index.html, continue o código anterior para implementar a funcionalidade de excluir e editar usuários:

````bash
    btExcluir.addEventListener('click', async () => {
          await fetch(`/users/${user.id}`, { method: 'DELETE' })
          fetchData()
        })

        btEditar.addEventListener('click', () => {
          const newName = prompt('Novo nome:', user.name)
          const newEmail = prompt('Novo email:', user.email)

          if (newName && newEmail) {
            fetch(`/users/${user.id}`, {
              method: 'PUT',
              headers: { 'Content-Type': 'application/json' },
              body: JSON.stringify({ name: newName, email: newEmail })
            }).then(fetchData)
          }
        })

        tbody.appendChild(tr)
      })
    }

    fetchData()
  </script>
</body>
</html>
````

### 2. Servir os arquivos estáticos no servidor:

- No arquivo src/app.ts, adicione o seguinte código para servir a interface web:

````bash
    app.use(express.static('public'));
````
## O arquivo src/app.ts completo deve ficar assim:

````bash
import express from 'express';
import cors from 'cors';
import { connect } from './database';

const port = 3333;
const app = express();

app.use(cors());
app.use(express.json());
app.use(express.static('public'));

app.get('/', (req, res) => {
  res.send('Hello World');
});

app.post('/users', async (req, res) => {
  const db = await connect();
  const { name, email } = req.body;

  const result = await db.run('INSERT INTO users (name, email) VALUES (?, ?)', [name, email]);
  const user = await db.get('SELECT * FROM users WHERE id = ?', [result.lastID]);

  res.json(user);
});

app.get('/users', async (req, res) => {
  const db = await connect();
  const users = await db.all('SELECT * FROM users');

  res.json(users);
});

app.delete('/users/:id', async (req, res) => {
  const db = await connect();
  const { id } = req.params;

  await db.run('DELETE FROM users WHERE id = ?', [id]);
  res.sendStatus(204);
});

app.put('/users/:id', async (req, res) => {
  const db = await connect();
  const { id } = req.params;
  const { name, email } = req.body;

  await db.run('UPDATE users SET name = ?, email = ? WHERE id = ?', [name, email, id]);
  const updatedUser = await db.get('SELECT * FROM users WHERE id = ?', [id]);

  res.json(updatedUser);
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
````

### 3. Testar a aplicação:

- No terminal, reinicie o servidor:

````bash
 npm run dev
````
- Acesse http://localhost:3333 no navegador.
- Você verá um formulário simples que permite adicionar, editar e excluir usuários.
- Quando você adicionar um usuário, ele será exibido na tabela abaixo, e você poderá editar ou excluir esse usuário diretamente na interface.

# Fim
