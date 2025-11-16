# Rotas
Rotas são os caminhos definidos por um sistema de roteamento para direcionar as requisições do usuário para os recursos corretos de uma aplicação. 
Elas associam um endereço (URL) a uma função específica, como a exibição de uma página HTML, o processamento de um formulário ou a chamada de uma API. 
Isso permite que a aplicação responda a diferentes URLs sem que cada uma corresponda diretamente a um arquivo estático no servidor. 

## 1. Criando rotas
```JavaScript
let express = require('express');
let router = express.Router();

/* Rota principal usando a view index.ejs. */
router.get('/', function(req, res, next) {
res.render('index', { title: 'Express' });
});

/* Rota “sobre” sem usar uma view. */
router.get('/sobre', function(req, res) {
let msg = '<h2>Sobre Rotas...</h2>';
res.send(msg);
});

module.exports = router;
```

## 2. Usando parâmetros em Rotas
Para utilizamos parâmetros em rotas utlizamos ```params```.
```JavaScript
let express = require('express');
let router = express.Router();

/* Outras rotas definidas anteriormente... */

/* Rota usando 1 parâmetro enviado na URL. */
router.get('/ola/:nome'

, function(req, res) {

let msg = '<h2>Olá, ' + req.params.nome + '!</h2>';
res.send(msg);
});

module.exports = router;
```
Podemos utilizar mais de um parâmetro em uma rota.
```JavaScript
let express = require('express');
let router = express.Router();

/* Outras rotas definidas anteriormente... */
/* Rota "imc" com vários parâmetros. */
router.get('/imc'

, function(req, res) {

let peso = req.query.peso;
let estatura = req.query.estatura;

let imc = peso / Math.pow(estatura, 2);
let msg = '<h3>Seu IMC é ' + imc.toFixed(2) + '</h3>';
res.send(msg);
});

module.exports = router;
```



