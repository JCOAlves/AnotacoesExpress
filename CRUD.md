# Imprementando funções CRUD
Funções CRUD (Create, Read, Update e Delete) são um conjunto de função em 
que você pode listar, criar, atualizar e deleatar itens de um projeto.
Dessa forma, vamos adicionar essas funcionalidade ao projeto.

## 1. Instalação do módulo `Nodemon`
Antes de imprementar as funções CRUD, nos devemos instalar o módulo `Nodemon`. O Nodemon reinicia, automaticamente, 
o aplicativo quando são detectadas alterações em arquivos, visto que anteriomente era necessario desligar e ligar novamente 
os arquivos para que as mudanças nos scripts sejam detectadas. Com isso, para instala-lo, use o comanda a seguir no terminal, 
dentro da pasta do projeto:

```bash
npm install –-save nodemon
```

## 2. Criando funções CRUD nas rotas

### Criar dados (Create)
```javascript
/* Rota para incluir dados de autor*/
router.post('/add', function(req, res) {
    let nome = req.body.nome;
    let nacionalidade = req.body.nacionalidade;

    let cmd = "INSERT INTO TbAutor (NoAutor, IdNacionalidade) VALUES (?, ?);";
    db.query(cmd, [nome, nacionalidade], function(erro){
        if (erro){
            res.send(erro);
        }
        res.redirect('/autores/listar');    
    });
});
```
- **`router.post`**: Método `POST` `HTTP` de criação de dados.
- **`let cmd`**: Variavel que armazena o comando SQL.
- **`INSERT INTO TbAutor`**: Comando SQL para inserir dados na tabela.
- **`(NoAutor, IdNacionalidade)`**: Colunas as quais receberam os dados na tabela.
- **`VALUES`**: Os dados que serão adicionados, de acordo com a ordem das colunas listadas.
- **`(?, ?)`**: Os pontos de interrogação são usados para se referir a dados pârametros que serão passados.
- **`[nome, nacionalidade]`**: Lista com as variavies parâmetros que vão ser adicionadas no banco, a qual a surmirão a posição do ? segundo a ordem da lista.
- **`function(erro)`**: Função que lida com erros da função POST

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

### Atualizar dados (Update)
```javascript
/* Rota para alterar dados de autor*/
router.put('/edit/:id', function(req, res) {
    let id = req.params.id;
    let nome = req.body.nome;
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

### Deletar dados (Delete)
```javascript
/*Rota para excluir dados de autor*/
router.delete('/delete/:id', function(req, res) {
    let id = req.params.id;
    let cmd = "DELETE FROM TbAutor WHERE IdAutor = ?;";
    db.query(cmd, [id], function(erro, listagem){
        if (erro){
            res.send(erro);
        }
        res.redirect(303, '/autores/listar');
    });
});
```

## 3. Fazendo requisições API no `JavaScript`