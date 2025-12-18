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

## 2. Criando funções CRUD

### Listar dados (Read)
```javascript

```

### Criar dados (Create)
```javascript
/* Rota para incluir dados de autor*/
router.post('/add'

, function(req, res) {

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

### Atualizar dados (Update)
```javascript

```

### Deletar dados (Delete)
```javascript

```
