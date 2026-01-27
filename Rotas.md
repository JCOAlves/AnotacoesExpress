# Rotas e requisições
Rotas são os caminhos definidos por um sistema de roteamento para direcionar as requisições do usuário para os recursos corretos de uma aplicação. 
Elas associam um endereço (URL) a uma função específica, como a exibição de uma página HTML, o processamento de um formulário ou a chamada de uma API. 
Isso permite que a aplicação responda a diferentes URLs sem que cada uma corresponda diretamente a um arquivo estático no servidor. 

## 1. Criando rotas
Para criar rotas no express usamos o objeto `router` para gerar a rota, que conta como parâmetros 
o nome da rota e um função inline que trata e gerencia as requsições (`req`) e respostas (`res`) à rota.
```JavaScript
const express = require('express');
const router = express.Router();

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
Para utilizamos parâmetros em rotas utilizamos ```params``` para acessar parâmentros na rota da requisição (`req`).
```JavaScript
const express = require('express');
const router = express.Router();

/* Outras rotas definidas anteriormente... */

/* Rota usando 1 parâmetro enviado na URL. */
router.get('/ola/:nome', function(req, res) {
  let msg = '<h2>Olá, ' + req.params.nome + '!</h2>';
  res.send(msg);
});

/*Podemos adicionar mais de um parâmetro na rota, mas é necessario separar os parâmetros por barras.*/
router.get('/ola/:nome/:sobrenome', function(req, res) {
  let msg = '<h2>Olá, ' + req.params.nome + " " + req.params.sobrenome + '!</h2>';
  res.send(msg);
});

module.exports = router;
```
Também podemos utilizar o ```query``` para acessar mais de uma parâmetro na URL, desde que comece com "?" e os parâmetros sejam separados por "&".
```JavaScript
const express = require('express');
const router = express.Router();

/* Outras rotas definidas anteriormente... */
/* Rota "imc" com vários parâmetros: localhost:3000/imc/?peso=88&estatura=1.82 */
router.get('/imc', function(req, res) {
  let peso = req.query.peso;
  let estatura = req.query.estatura;

  let imc = peso / Math.pow(estatura, 2);
  let msg = '<h3>Seu IMC é ' + imc.toFixed(2) + '</h3>';
  res.send(msg);
  //Resposta: "Seu IMC é 26.57"
});

module.exports = router;
```
## 3. Enviando resposta de requisições
No Express, o objeto de resposta (`res`) oferece diversos métodos para enviar dados de volta ao cliente e finalizar o ciclo de requisição-resposta.
Entre os principais métodos de resposta estão:

- `res.send([body])`: Envia uma resposta HTTP de diversos tipos. O corpo (body) pode ser uma string, um objeto, um Array, um Buffer ou um Boolean. O Express define automaticamente o `Content-Type` (ex: `text/html` para strings, `application/json` para objetos).
- `res.json([body])`: Envia uma resposta JSON. É o método ideal para APIs REST. Ele converte objetos ou arrays JavaScript para uma string JSON (usando `JSON.stringify`) e define o `Content-Type` como `application/json`.
- `res.type(type)`: Define o campo `Content-Type` do cabeçalho HTTP para o tipo MIME fornecido. 
- `res.sendFile(path [, options] [, fn])`: Transfere um arquivo como um octet stream, definindo o Content-Type com base na extensão do arquivo.
- `res.download()`: Solicita que um arquivo seja baixado pelo navegador do cliente. 
- `res.status(code)`: Define o código de status HTTP da resposta (ex: 200, 404, 500). É um método encadeável, o que significa que pode ser usado antes de outro método de resposta.
*Ex:* `res.status(404).send('Not Found')`.
- `res.sendStatus(statusCode)`: Define o status code HTTP e envia a string correspondente ao código como corpo da resposta (ex: `res.sendStatus(200)` envia "OK").
- `res.render(view [, locals] [, callback])`: Renderiza uma view (template) e envia o HTML gerado para o cliente. Geralmente usado com motores de template como EJS, Pug ou Handlebars.
- `res.redirect([status,] path)`: Redireciona a requisição para uma URL específica. Por padrão, o status é 302 (Found).
- `res.end()`: Finaliza o processo de resposta manualmente sem enviar nenhum dado no corpo, útil para respostas vazias. 

```js
app.get('/api/user', (req, res) => {
  // Define status 200 e envia JSON
  res.status(200).json({ id: 1, name: 'João' });
});

app.get('/error', (req, res) => {
  // Define status 404 e envia mensagem
  res.status(404).send('Página não encontrada');
});

app.get('/error', (req, res) => {
  // Define status 500 e envia "Internal Server Error"
  res.sendStatus(500);
});

app.get('/perfil', (req, res) => {
  // Renderiza template engine (EJS, Pug, etc)
  res.render('perfil', { usuario: 'Maria' });
});

app.get('/imagem', (req, res) => {
  // Envia arquivo
  res.sendFile(__dirname + '/foto.png');
});

app.get('/antiga-rota', (req, res) => {
  // Redireciona para uma noba rota
  res.redirect('/nova-rota');
});
```







