
# Abra o MongoDB Compass.

### Clique no botão "New Connection" para se conectar ao servidor MongoDB local (geralmente, você pode usar a configuração padrão: host localhost e porta 27017).
Após se conectar, clique em "Create Database" e crie um novo banco de dados chamado "exercicio_mongodb".

### Criar 2 collections:  aluno (nome, rm, curso) e professor (nome, cpf, datanasc) .

Criando o Aplicativo Web HTML

criação de um servidor para fornecer os dados do MongoDB e configurar a API local. 
Para isso, vou usar Node.js e Express.js como exemplo.

**Passo 4: Configuração do Servidor Node.js com Express.js**

1. Certifique-se de que você tem o Node.js instalado em seu sistema. Você pode verificar a instalação digitando o seguinte comando no terminal:

   ```bash
   node -v
   ```

   Se não estiver instalado, você pode baixá-lo e instalá-lo no site oficial do Node.js: https://nodejs.org/

2. Crie uma pasta para o seu projeto e navegue até ela no terminal:

   ```bash
   mkdir exercicio-mongodb
   cd exercicio-mongodb
   ```

3. Inicialize um projeto Node.js executando o seguinte comando:

   ```bash
   npm init -y
   ```

   Isso criará um arquivo `package.json` que conterá as informações do seu projeto.

4. Instale as dependências necessárias, incluindo o Express.js e o driver MongoDB para Node.js:

   ```bash
   npm install express mongodb
   ```

5. Crie um arquivo JavaScript chamado `server.js` na sua pasta do projeto e adicione o seguinte código:

const express = require('express');
const app = express();
const { MongoClient } = require('mongodb');
const port = process.env.PORT || 3000;

// Middleware para lidar com JSON
app.use(express.json());

// Rota para buscar dados do MongoDB
app.get('/api/data', async (req, res) => {
    try {
        // Conectar ao banco de dados MongoDB
        const client = new MongoClient('mongodb://localhost:27017' );
        await client.connect();
        const db = client.db('Eteclab'); // Alterado para "Eteclab"

        // Buscar dados das coleções (aluno e professor) - nomes das coleções alterados
        const alunos = await db.collection('aluno').find().toArray(); // Alterado para "aluno"
        const professores = await db.collection('professor').find().toArray(); // Alterado para "professor"

        // Encerrar a conexão com o MongoDB
        client.close();

        // Enviar os dados como resposta
        res.json({ alunos, professores });
    } catch (error) {
        console.error('Erro ao buscar dados do MongoDB:', error);
        res.status(500).json({ error: 'Erro ao buscar dados do MongoDB' });
    }
});

// Iniciar o servidor
app.listen(port, () => {
    console.log(`Servidor rodando na porta ${port}`);
}); 


**Nota importante**: Certifique-se de que o MongoDB está em execução no mesmo host e porta que você especificou na conexão (`'mongodb://localhost:27017'` neste exemplo).

**Passo 5: Iniciar o Servidor**

Para iniciar o servidor, vá para o diretório do projeto no terminal e execute o seguinte comando:

```bash
node server.js
```

Isso iniciará o servidor na porta especificada (3000, neste exemplo).

**Passo 6: Acesse o Aplicativo Web**

Agora, com o servidor em execução, você pode acessar o aplicativo web HTML que criamos anteriormente (por exemplo, em `http://localhost:3000`) para ver os dados do MongoDB sendo exibidos na página.

O erro que você está enfrentando é devido à política de mesma origem (Same-Origin Policy) do navegador, que bloqueia solicitações feitas a partir de um domínio (ou origem) diferente do servidor onde a API está hospedada. No seu caso, o aplicativo web HTML está sendo servido a partir de `http://127.0.0.1:5500`, e ele está tentando fazer uma solicitação para `http://localhost:3000/api/data`, que é uma origem diferente (`localhost` e `127.0.0.1` são considerados origens diferentes).

Para permitir que o aplicativo web faça solicitações ao servidor Node.js em `localhost:3000`, você precisa configurar a política de mesma origem no lado do servidor para permitir as solicitações de origens específicas. Você pode fazer isso usando o middleware `cors` em seu servidor Express.js.

Aqui está como você pode configurar o middleware `cors` no seu servidor:

1. Primeiro, instale a biblioteca `cors` usando npm:

```bash
npm install cors
```

2. Em seu código do servidor, inclua e configure o middleware `cors` antes das rotas que estão sendo acessadas pelo seu aplicativo web. Altere o seu código do servidor para o seguinte:

```javascript
const express = require('express');
const app = express();
const { MongoClient } = require('mongodb');
const cors = require('cors'); // Importe o módulo cors
const port = process.env.PORT || 3000;

// Use o middleware cors para permitir solicitações de qualquer origem
app.use(cors());

// ... Resto do código do servidor ...

// Rota para buscar dados do MongoDB
app.get('/api/data', async (req, res) => {
    // ... Resto do código para buscar dados ...
});

// Iniciar o servidor
app.listen(port, () => {
    console.log(`Servidor rodando na porta ${port}`);
});
```

Com o middleware `cors` configurado dessa forma, ele permitirá que qualquer origem acesse as rotas do seu servidor. Isso pode ser útil para desenvolvimento local, mas em um ambiente de produção, você deve configurar a política de mesma origem de acordo com os requisitos de segurança do seu aplicativo.

 
alterar o arquivo HTML para listar as coleções "aluno" e "professor" em vez de "usuarios" e "produtos". Aqui está o HTML atualizado:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Exemplo MongoDB</title>
</head>
<body>
    <h1>Lista de Alunos</h1>
    <ul id="aluno-list"></ul>

    <h1>Lista de Professores</h1>
    <ul id="professor-list"></ul>

    <script>
        // Função para buscar dados do MongoDB e exibi-los na página
        async function fetchData() {
            try {
                const response = await fetch("http://localhost:3000/api/data");
                const data = await response.json();

                // Exibindo alunos
                const alunoList = document.getElementById("aluno-list");
                data.alunos.forEach(aluno => {
                    const listItem = document.createElement("li");
                    listItem.textContent = aluno.nome;
                    alunoList.appendChild(listItem);
                });

                // Exibindo professores
                const professorList = document.getElementById("professor-list");
                data.professores.forEach(professor => {
                    const listItem = document.createElement("li");
                    listItem.textContent = professor.nome;
                    professorList.appendChild(listItem);
                });
            } catch (error) {
                console.error("Erro ao buscar dados:", error);
            }
        }

        fetchData();
    </script>
</body>
</html>
```

Neste HTML atualizado, fizemos as seguintes alterações:

1. Mudamos os títulos para "Lista de Alunos" e "Lista de Professores" para corresponder às coleções "aluno" e "professor".

2. No JavaScript, atualizamos as partes que lidam com a exibição dos dados. Agora, ele acessa `data.alunos` para listar os alunos e `data.professores` para listar os professores. Também atualizamos as propriedades dos objetos para refletir os campos reais do documento MongoDB, como `aluno.nome` e `professor.nome`.
 
Criando um novo documento (POST)
Claro, agora que você já tem a parte de consulta (GET) funcionando, vamos adicionar as operações REST completas (POST, PUT e DELETE) para interagir com a base de dados MongoDB. Abaixo estão os exemplos de como você pode implementar essas operações:

**1. Criando um novo documento (POST)**

Para criar um novo documento em uma coleção (por exemplo, "aluno" ou "professor"), você pode adicionar uma rota POST ao seu servidor Express.js. Certifique-se de que seu aplicativo web HTML tenha um formulário para enviar os dados.

```javascript
// Rota para criar um novo aluno
app.post('/api/alunos', async (req, res) => {
    try {
        const { nome, idade, curso } = req.body; // Dados do aluno a serem criados
        const client = new MongoClient('mongodb://localhost:27017');
        await client.connect();
        const db = client.db('Eteclab');

        // Inserir novo aluno na coleção "aluno"
        const result = await db.collection('aluno').insertOne({ nome, idade, curso });

        // Encerrar a conexão com o MongoDB
        client.close();

        // Enviar a resposta com o ID do novo aluno
        res.json({ message: 'Aluno criado com sucesso', _id: result.insertedId });
    } catch (error) {
        console.error('Erro ao criar aluno:', error);
        res.status(500).json({ error: 'Erro ao criar aluno' });
    }
});
```

**2. Atualizando um documento existente (PUT)**

Para atualizar um documento existente, você pode adicionar uma rota PUT ao seu servidor Express.js. Certifique-se de que seu aplicativo web HTML tenha um formulário para enviar os dados de atualização.

```javascript
// Rota para atualizar um aluno existente
app.put('/api/alunos/:id', async (req, res) => {
    try {
        const { nome, idade, curso } = req.body; // Dados de atualização do aluno
        const { id } = req.params; // ID do aluno a ser atualizado
        const client = new MongoClient('mongodb://localhost:27017');
        await client.connect();
        const db = client.db('Eteclab');

        // Atualizar aluno na coleção "aluno" com base no ID
        const result = await db.collection('aluno').updateOne({ _id: id }, { $set: { nome, idade, curso } });

        // Encerrar a conexão com o MongoDB
        client.close();

        if (result.matchedCount === 0) {
            res.status(404).json({ message: 'Aluno não encontrado' });
        } else {
            res.json({ message: 'Aluno atualizado com sucesso' });
        }
    } catch (error) {
        console.error('Erro ao atualizar aluno:', error);
        res.status(500).json({ error: 'Erro ao atualizar aluno' });
    }
});
```

**3. Excluindo um documento (DELETE)**

Para excluir um documento, você pode adicionar uma rota DELETE ao seu servidor Express.js.

```javascript
// Rota para excluir um aluno existente
app.delete('/api/alunos/:id', async (req, res) => {
    try {
        const { id } = req.params; // ID do aluno a ser excluído
        const client = new MongoClient('mongodb://localhost:27017');
        await client.connect();
        const db = client.db('Eteclab');

        // Excluir aluno na coleção "aluno" com base no ID
        const result = await db.collection('aluno').deleteOne({ _id: id });

        // Encerrar a conexão com o MongoDB
        client.close();

        if (result.deletedCount === 0) {
            res.status(404).json({ message: 'Aluno não encontrado' });
        } else {
            res.json({ message: 'Aluno excluído com sucesso' });
        }
    } catch (error) {
        console.error('Erro ao excluir aluno:', error);
        res.status(500).json({ error: 'Erro ao excluir aluno' });
    }
});
```

Lembre-se de que esses são exemplos básicos e que você deve considerar a validação de entrada e a segurança ao implementar uma API completa em um ambiente de produção. Além disso, você pode seguir um padrão de nomenclatura RESTful e adicionar mais rotas e funcionalidades conforme necessário para atender aos requisitos do seu projeto.


Refotorar para simplificar o código
Claro, é possível refatorar o código para torná-lo mais enxuto e evitar a repetição dos mesmos parâmetros dentro do bloco `try`. Vou criar uma função para conectar ao MongoDB e lidar com as operações. Aqui está o código refatorado:

```javascript
const express = require('express');
const app = express();
const { MongoClient, ObjectId } = require('mongodb');
const cors = require('cors');
const port = process.env.PORT || 3000;

app.use(express.json());
app.use(cors());

// Função para conectar ao MongoDB
async function connectDB() {
    const client = new MongoClient('mongodb://localhost:27017');
    await client.connect();
    const db = client.db('Eteclab');
    return { client, db };
}

// Rota para criar um novo aluno
app.post('/api/alunos', async (req, res) => {
    try {
        const { nome, idade, curso } = req.body;
        const { db, client } = await connectDB();
        const result = await db.collection('aluno').insertOne({ nome, idade, curso });
        client.close();
        res.json({ message: 'Aluno criado com sucesso', _id: result.insertedId });
    } catch (error) {
        console.error('Erro ao criar aluno:', error);
        res.status(500).json({ error: 'Erro ao criar aluno' });
    }
});

// Rota para atualizar um aluno existente
app.put('/api/alunos/:id', async (req, res) => {
    try {
        const { nome, idade, curso } = req.body;
        const { id } = req.params;
        const { db, client } = await connectDB();
        const result = await db.collection('aluno').updateOne({ _id: new ObjectId(id) }, { $set: { nome, idade, curso } });
        client.close();
        if (result.matchedCount === 0) {
            res.status(404).json({ message: 'Aluno não encontrado' });
        } else {
            res.json({ message: 'Aluno atualizado com sucesso' });
        }
    } catch (error) {
        console.error('Erro ao atualizar aluno:', error);
        res.status(500).json({ error: 'Erro ao atualizar aluno' });
    }
});

// Rota para excluir um aluno existente
app.delete('/api/alunos/:id', async (req, res) => {
    try {
        const { id } = req.params;
        const { db, client } = await connectDB();
        const result = await db.collection('aluno').deleteOne({ _id: new ObjectId(id) });
        client.close();
        if (result.deletedCount === 0) {
            res.status(404).json({ message: 'Aluno não encontrado' });
        } else {
            res.json({ message: 'Aluno excluído com sucesso' });
        }
    } catch (error) {
        console.error('Erro ao excluir aluno:', error);
        res.status(500).json({ error: 'Erro ao excluir aluno' });
    }
});

app.listen(port, () => {
    console.log(`Servidor rodando na porta ${port}`);
});
```

Neste código refatorado, a função `connectDB` é responsável por estabelecer a conexão com o MongoDB e retornar o objeto de banco de dados e cliente MongoDB. Isso ajuda a evitar a repetição de código em cada rota e torna o código mais modular e legível. Certifique-se de que o servidor Node.js esteja em execução e pronto para lidar com essas operações REST.
