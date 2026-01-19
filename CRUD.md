# Imprementando funções CRUD
Funções CRUD (Create, Read, Update e Delete) são um conjunto de função em 
que você pode listar, criar, atualizar e deleatar itens de um projeto.
Dessa forma, vamos adicionar essas funcionalidade ao projeto.

## 1. Instalação do módulo `Nodemon`
Antes de imprementar as funções CRUD, nos devemos instalar o módulo `Nodemon`. O Nodemon reinicia, automaticamente, 
o aplicativo quando são detectadas alterações em arquivos, visto que anteriomente era necessario desligar e ligar novamente 
os arquivos para que as mudanças nos scripts sejam detectadas. Com isso, para instala-lo, use o comanda a seguir no terminal, 
dentro da pasta do projeto:

- Instalação: Recomenda-se instalá-lo como dependência de desenvolvimento:
  ```bash
  npm install --save-dev nodemon
  ```
- Como usar:
  1. No arquivo package.json, adicione um script na seção "scripts". Recomendo manter o comando `start` original e criar um novo chamado `dev`:
    ```json
    "scripts": {
        "start": "node ./bin/www",
        "dev": "nodemon ./bin/www"
    },
    ```

  2. Agora para rodar o projeto, em vez de usar `npm start`, você usará o comando abaixo no seu terminal:
     ```bash
     npm run dev
     ```

## 2. Criando funções CRUD nas rotas
### Criar dados (Create)
```javascript
/* Rota para incluir dados de autor*/
router.post('/add', function(req, res) {
    //É no body que estão os dados passados na requisição.
    let nome = req.body.nome;
    let nacionalidade = req.body.nacionalidade;

    let cmd = "INSERT INTO TbAutor (NoAutor, IdNacionalidade) VALUES (?, ?);";
    db.query(cmd, [nome, nacionalidade], function(erro){
        if (erro){
            //Envia erro
            res.send(erro);
        }
        //Redireciona para outra rota.
        res.redirect('/autores/listar');    
    });
});
```
- **`router.post`**: Método POST HTTP de criação de dados.
- **`let cmd`**: Variavel que armazena o comando SQL.
- **`INSERT INTO TbAutor`**: Comando SQL para inserir dados na tabela.
- **`(NoAutor, IdNacionalidade)`**: Colunas as quais receberam os dados na tabela.
- **`VALUES`**: Os dados que serão adicionados, de acordo com a ordem das colunas listadas.
- **`(?, ?)`**: Os pontos de interrogação são usados para se referir a dados pârametros que serão passados.
- **`[nome, nacionalidade]`**: Lista com as variaveis parâmetros que vão ser adicionadas no banco, a qual a surmirão a posição do ? segundo a ordem da lista.
- **`function(erro)`**: Função que vai lidar com erros e a função POST.
- **`res.redirect`**: Função que rendireciona para outra rota.

### Listar dados (Read)
```javascript
router.get('/add', function(req, res) {
    res.render('autores-add', {resultado: {}})
};

/* Rota para obter os dados atuais do autor*/
router.get('/edit/:id', function(req, res) {
    let id = req.params.id;
    let cmd = "SELECT * FROM TbAutor WHERE IdAutor = ?;";

    db.query(cmd, [id], function(erro, listagem){
        if (erro){
            res.send(erro);
        }
    res.render('autores-add', {resultado: listagem[0]});
    });
});
```
- **`router.get`**: Método GET HTTP de listagem de dados.
- **`let cmd`**: Variavel que armazena o comando SQL.
- **`SELECT * FROM TbAutor`**: Comando SQL para selecionar dados da tabela que vão ser exibidos.
- **`WHERE IdAutor`**: Condicional para os dados que vão ou não ser exibidos.
- **`[id]`**: Lista com as variaveis parâmetros, sendo utilizada para identificar um dado espefico.
- **`res.render`**: Função que renderiza os dados no arquivo `.ejs`.

### Atualizar dados (Update)
```javascript
/* Rota para alterar dados de autor*/
router.put('/edit/:id', function(req, res) {
    let id = req.params.id; //Parâmetro da rota com o identicidor (id)
    let nome = req.body.nome; //É no body que estão os dados passados na requisição.
    let nacionalidade = req.body.nacionalidade;

    let cmd = "UPDATE TbAutor SET NoAutor = ?, IdNacionalidade = ? WHERE IdAutor = ?;";
    db.query(cmd, [nome, nacionalidade, id], function(erro, listagem){
        if (erro){
            res.send(erro);
        }
        //Code 303 permite o redirecionamento entre rotas com métodos diferentes:
        //PUT → GET
        res.redirect(303, '/autores/listar');
    });
});
```
- **`router.put`**: Método PUT HTTP de atualização de dados.
- **`let cmd`**: Variavel que armazena o comando SQL.
- **`UPDATE TbAutor`**: Comando SQL que atualiza os dados da tabela.
- **`SET NoAutor`**: Condicional que diz quais colunas da tabela serão atualizadas
- **`WHERE IdAutor`**: Condicional que diz quais dados serão atualizados ou não.
- **`[nome, nacionalidade, id]`**: Lista com as variaveis parâmetros que atualizarão os dados.

### Deletar dados (Delete)
```javascript
/*Rota para excluir dados de autor*/
router.delete('/delete/:id', function(req, res) {
    let id = req.params.id; //Recupera parâmentro id da rota
    let cmd = "DELETE FROM TbAutor WHERE IdAutor = ?;";
    db.query(cmd, [id], function(erro, listagem){
        if (erro){
            res.send(erro);
        }
        res.redirect(303, '/autores/listar');
    });
});
```
- **`router.delete`**: Método DELETE HTTP de listagem de dados.
- **`let cmd`**: Variavel que armazena o comando SQL.
- **`DELETE FROM TbAutor`**: Comando SQL que deleta os dados da tabela.
- **`WHERE IdAutor;`**: Condicional que diz quais dados serão deletados ou não.
- **`[id]`**: Lista com as variaveis parâmetros, sendo utilizada para identificar um dado espefico.

## 3. Fazendo requisições API no `JavaScript`
No parte de interação e contado com o usuário, Frontend, é realizado requisições JavaScript, 
como POST, GET, PUT ou DELETE, para parte das rotas do servidor do projeto, Backend. Assim,
é utilizado a função `fetch` do JS.

### Método Fetch/POST
```javascript
async function POST(rota, objeto) { //Função assincrona para requisições HTTP
    try {
        const objetoJSON = JSON.stringify(objeto); //Converte objeto JS com os dados em formato JSON
        let resposta = await fetch(rota, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' }, //Informa o tipo de dado a ser enviador ao servidor.
            body: objetoJSON
        })
        resposta = await resposta.json(); //Converte a resposta em formato JSON
        return resposta //Retorna a resposta do método POST.
    }
    catch (erro) { //Encontra e trata erros na requisição.
        console.error(`Falha na requisição: ${erro.message || erro}`)
    }
};
```

### Método Fetch/GET
```javascript
async function GET(rota){ 
    try { 
        let resposta = await fetch(rota); // Busca os dados na rota fornecida

        // Verifica se a resposta HTTP é OK (200-299). 404/500 não rejeitam a Promise por padrão [3, 5].
        if (!resposta.ok) {
            throw new Error(`Erro HTTP! Status: ${response.status}`);
        }

        // Converte a resposta para JSON (ou .text(), .blob(), etc.)
        const dados = await resposta.json();
        return dados; // Retorna os dados buscados.

    } catch (erro) {
        console.error(`Falha na requisição: ${erro.message || erro}`);
    }
};
```

### Método Fetch/PUT
```javascript
async function PUT(rotaEspecifica, objeto) {
    try {
        const objetoJSON = JSON.stringify(objeto);
        let resposta = await fetch(rotaEspecifica, {
            method: 'PUT',
            headers: { 'Content-Type': 'application/json' },
            body: objetoJSON //JSON com novos valores de dados.
        })
        resposta = await resposta.json();
        return resposta //Retorna a resposta do método PUT.
    }
    catch (erro) {
        console.error(`Falha na requisição: ${erro.message || erro}`)
    }
};
```

### Método Fetch/DELETE
```javascript
async function DELETE(rotaEspecifica) {
    try{
        let resposta = await fetch(rotaEspecifica, {
            method: 'DELETE',
            headers: { 'Content-Type': 'application/json' }
        })
        resposta = await resposta.json();
        return resposta
    }
    catch (erro) {
        console.error(`Falha na requisição: ${erro.message || erro}`)
    }
};
```
