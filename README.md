
# Tutorial Super Detalhado: Configurando um Projeto Node.js com TypeScript no Codespaces
### Este tutorial vai te guiar, passo a passo, para criar um servidor web usando Node.js e TypeScript. Você vai aprender a configurar um ambiente de desenvolvimento, criar um servidor simples, conectar um banco de dados SQLite, e criar uma interface web para interagir com os dados.

### Passo 1:  Criar um Repositório no GitHub

1- Crie um repositório no GitHub:
Acesse o GitHub e faça login.
Clique no botão "New" no canto superior direito para criar um novo repositório.

2- Nomeiei o repositório como Projetin.

3- Marque a opção "Public".

4- Marque a opção "Add a README file".

5- Clique em "Create repository". 

## passo 2: Iniciar Codespaces no GitHub

1-Abrir o Codespaces:

- Dentro do seu repositório recém-criado, você verá um botão chamado "Code". Clique nele.

- Selecione a aba "Codespaces" e clique em "Create Codespace on main".

- Aguarde alguns minutos enquanto o Codespaces é configurado. Ele vai abrir uma nova janela que simula um ambiente de desenvolvimento completo na nuvem.

## Passo 3: Configurar o Ambiente de Desenvolvimento

1- Iniciar o projeto Node.js:

- Dentro do Codespaces, na parte inferior da tela, você verá um terminal. Esse terminal permite que você digite comandos que serão executados no seu ambiente.
### No terminal, digite o seguinte comando para criar o projeto básico do Node.js:

````bash
npm init -y
````
- Esse comando cria um arquivo chamado package.json que armazena as informações básicas do seu projeto.

## Passo 4: Instalar as Dependências Necessárias

1- Instalar o Express, CORS e SQLite3:

- Esses pacotes são fundamentais para o seu projeto. O Express permite que você crie um servidor web, o CORS permite que seu servidor lide com requisições de outros domínios, e o SQLite3 é o banco de dados que você vai usar.

- Para instalar esses pacotes, digite o seguinte comando no terminal: 

````bash
npm install express cors sqlite3
````
- Aguarde alguns segundos enquanto as bibliotecas são baixadas e instaladas.


## Passo 5: Instalar Ferramentas de Desenvolvimento


1- Instalar o TypeScript e outras ferramentas:

- O TypeScript é uma linguagem que melhora o JavaScript, oferecendo tipos que ajudam a prevenir erros no código. Vamos também instalar o Nodemon, que reinicia automaticamente o servidor toda vez que você faz uma mudança, e outras ferramentas que ajudam no desenvolvimento.

- Digite o seguinte comando no terminal:

````bash
npm install --save-dev typescript nodemon ts-node @types/express @types/cors @types/sqlite3
````

- Esse comando instala as ferramentas e bibliotecas necessárias para trabalhar com TypeScript em seu projeto Node.js. 

## Passo 6: Inicializar o TypeScript

1- Configurar o TypeScript:

- Agora, vamos configurar o TypeScript no seu projeto. Digite o seguinte comando no terminal:

````bash
npx tsc --init
````

- Esse comando cria um arquivo chamado tsconfig.json, que define as configurações do TypeScript para o seu projeto.

## Passo 7: Criar a Estrutura de Diretórios e Arquivos

1- Criar a pasta src e o arquivo app.ts:

- O src será o diretório onde colocaremos nosso código. Dentro dele, vamos criar o arquivo app.ts, que será o arquivo principal do nosso servidor.

- Para fazer isso, digite os seguintes comandos no terminal, um de cada vez: 

````bash
mkdir src
````

````bash
touch src/app.ts
````

- mkdir src cria a pasta src, e touch src/app.ts cria um arquivo chamado app.ts dentro dessa pasta.

## Passo 8: Configurar o tsconfig.json

1- Configurar o tsconfig.json

- Vamos ajustar algumas configurações no arquivo tsconfig.json para que o TypeScript saiba onde colocar os arquivos compilados.

- No Codespaces, à esquerda, você verá a lista de arquivos do seu projeto. Clique em tsconfig.json para abri-lo.

- Encontre a linha que diz "outDir": "./" e mude para "outDir": "./dist".

- Logo abaixo, adicione a linha "rootDir": "./src".

### O arquivo deve ficar assim:

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

- Essas configurações garantem que os arquivos TypeScript sejam compilados para a pasta dist.

## Passo 9:  Configurar o package.json

1- Adicionar scripts ao package.json:

- Vamos adicionar um script que facilita o processo de iniciar o servidor.

- No Codespaces, clique no arquivo package.json para abri-lo.

- Dentro da seção "scripts", adicione a seguinte linha:

````bash
"scripts": {
  "dev": "nodemon src/app.ts"
}
````
- Esse script permite que você inicie o servidor simplesmente digitando npm run dev no terminal.

## Passo 10: Criar o Arquivo Inicial do Servidor

1- Escrever o código do servidor:

- Vamos agora adicionar o código que configura o servidor e define uma rota simples.

- Clique no arquivo src/app.ts para abri-lo e cole o seguinte código:

````bash
import express from 'express';
import cors from 'cors';

const port = 3333;  // Define a porta em que o servidor vai rodar
const app = express();  // Cria uma nova aplicação Express

app.use(cors());  // Permite que seu servidor aceite requisições de outros domínios
app.use(express.json());  // Permite que seu servidor processe dados em formato JSON

// Define uma rota que responde "Hello World" quando acessada
app.get('/', (req, res) => {
  res.send('Hello World');
});

// Inicia o servidor na porta definida
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
````

- Esse código cria um servidor que, ao ser acessado, responde com a mensagem "Hello World".

## Passo 11: Iniciar o Servidor

1- Rodar o servidor:

- Para verificar se o servidor está funcionando, volte ao terminal e digite:

````bash
npm run dev
````

- Se tudo estiver correto, você verá a mensagem Server running on port 3333 no terminal, indicando que o servidor está rodando.

## Passo 12: Testar o Servidor

1- Acessar o servidor no navegador:

- No Codespaces, você verá uma barra na parte superior com uma aba chamada "Ports". Clique nela.

- Você verá a porta 3333 listada. Clique no link ao lado dessa porta.

- Uma nova aba será aberta no seu navegador, e você deverá ver a mensagem "Hello World".

## Passo 13: Configurar o Banco de Dados

1- Criar o arquivo de conexão com o banco de dados:

- Vamos agora configurar o SQLite, que será o banco de dados usado no projeto.

- No Codespaces, crie um novo arquivo chamado src/database.ts usando o comando:

````bash
touch src/database.ts
````

- Abra o arquivo src/database.ts e adicione o seguinte código:

````typescript
import { open, Database } from 'sqlite';
import sqlite3 from 'sqlite3';

let instance: Database | null = null;

export async function connect() {
  if (instance) return instance;  // Retorna a instância existente do banco de dados, se já houver uma

  const db = await open({
    filename: './src/database.sqlite',  // Define o caminho do arquivo do banco de dados
    driver: sqlite3.Database  // Define o driver que será usado pelo SQLite
  });

  // Cria a tabela "users" se ela ainda não existir
  await db.exec(`
    CREATE TABLE IF NOT EXISTS users (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      name TEXT NOT NULL,
      email TEXT NOT NULL UNIQUE
    )
  `);

  instance = db;  // Armazena a instância do banco de dados para reutilização
  return db;  // Retorna a instância do banco de dados
}
````
- Esse código cria uma conexão com o banco de dados e cria uma tabela users para armazenar informações de usuários.

## Passo 14: Criar Rotas CRUD (Create, Read, Update, Delete)

1- Adicionar rotas para o CRUD:

- Vamos agora adicionar rotas que permitem criar, ler, atualizar e deletar usuários no banco de dados.

- Abra o arquivo src/app.ts e substitua o código pelo seguinte:

````typescript
import express from 'express';
import cors from 'cors';
import { connect } from './database';

const port = 3333;
const app = express();

app.use(cors());
app.use(express.json());

// Rota para criar um novo usuário
app.post('/users', async (req, res) => {
  const { name, email } = req.body;
  const db = await connect();

  try {
    const result = await db.run(
      `INSERT INTO users (name, email) VALUES (?, ?)`,
      [name, email]
    );

    res.status(201).json({ id: result.lastID, name, email });
  } catch (error) {
    res.status(500).json({ error: 'Erro ao criar usuário' });
  }
});

// Rota para listar todos os usuários
app.get('/users', async (req, res) => {
  const db = await connect();
  const users = await db.all(`SELECT * FROM users`);
  res.json(users);
});

// Rota para atualizar um usuário existente
app.put('/users/:id', async (req, res) => {
  const { id } = req.params;
  const { name, email } = req.body;
  const db = await connect();

  try {
    await db.run(
      `UPDATE users SET name = ?, email = ? WHERE id = ?`,
      [name, email, id]
    );
    res.status(200).json({ id, name, email });
  } catch (error) {
    res.status(500).json({ error: 'Erro ao atualizar usuário' });
  }
});

// Rota para deletar um usuário
app.delete('/users/:id', async (req, res) => {
  const { id } = req.params;
  const db = await connect();

  try {
    await db.run(`DELETE FROM users WHERE id = ?`, [id]);
    res.status(200).json({ message: 'Usuário deletado com sucesso' });
  } catch (error) {
    res.status(500).json({ error: 'Erro ao deletar usuário' });
  }
});

// Inicia o servidor
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
````
- Esse código adiciona as rotas que permitem criar (POST /users), listar (GET /users), atualizar (PUT /users/:id), e deletar (DELETE /users/:id) usuários.

## Passo 15: Testar as Rotas do CRUD

1- Testar as rotas com o Postman ou Insomnia:

- Para testar as rotas, você pode usar ferramentas como Postman ou Insomnia. Estas ferramentas permitem que você envie requisições HTTP e veja as respostas do servidor.

- A URL base será http://localhost:3333/users e você pode testar as seguintes rotas:

- POST /users com um JSON no corpo da requisição, por exemplo: { "name": "João", "email": "joao@email.com" }.

- GET /users para listar todos os usuários.

- PUT /users/:id para atualizar um usuário existente.

- DELETE /users/:id para deletar um usuário.

## Passo 16: Criar a Interface Web

1- Criar o arquivo HTML para a interface web:

- Agora, vamos criar uma interface web simples para interagir com as rotas do servidor.

- Crie um arquivo chamado index.html na raiz do projeto:

````html
touch index.html
````

- Abra o arquivo index.html e adicione o seguinte código:

````html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gerenciamento de Usuários</title>
  <style>
    table {
      width: 100%;
      border-collapse: collapse;
    }
    table, th, td {
      border: 1px solid black;
    }
    th, td {
      padding: 8px;
      text-align: left;
    }
  </style>
</head>
<body>
  <h1>Gerenciamento de Usuários</h1>
  <form id="userForm">
    <input type="text" id="name" placeholder="Nome" required>
    <input type="email" id="email" placeholder="Email" required>
    <button type="submit">Adicionar Usuário</button>
  </form>
  <h2>Lista de Usuários</h2>
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Nome</th>
        <th>Email</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody id="userList"></tbody>
  </table>
  <script>
    const form = document.getElementById('userForm')
    const tbody = document.getElementById('userList')

    form.onsubmit = async (e) => {
      e.preventDefault()

      const name = document.getElementById('name').value
      const email = document.getElementById('email').value

      await fetch('/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name, email })
      })

      fetchData()
    }

    async function fetchData() {
      const response = await fetch('/users')
      const users = await response.json()
      tbody.innerHTML = ''

      users.forEach(user => {
        const tr = document.createElement('tr')
        tr.innerHTML = `
          <td>${user.id}</td>
          <td>${user.name}</td>
          <td>${user.email}</td>
          <td>
            <button class="excluir">Excluir</button>
            <button class="editar">Editar</button>
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

- Esse código cria um formulário para adicionar novos usuários e uma tabela para exibir, editar e excluir usuários existentes.

## Conclusão

### Parabéns!

- Você configurou com sucesso um projeto Node.js com TypeScript, Express, CORS e SQLite. Agora, você tem um servidor funcional que pode ser acessado via interface web para gerenciar uma lista de usuários.