# Agrupando rotas no Express
Vimos que as rotas do Express podem ser passadas em um único arquivo, mas não é uma pratica interessante. 
Visto isso, é uma boa pratica dividir e agrupa rotas em arquivos separados, segundo as suas funcionalidades.

## 1. Agrupando rotas espeficas
Uma boa forma de agrupar rotas é de acordo com sua funcionalidade e os tipos de dados que manipulam. 
Um exemplo disso são rotas sobre autores de livros, em que listam e registram autores, manipulando assim os métodos
`GET` e `POST`. Assim, as rotas são arquivadas em um único arquivo. E esses arquivos são guardados em uma subpasta no projeto, 
podendo ser chamada de `Routes`.

```javascript
let express = require('express');
let db = require('../utils/db')
let router = express.Router();

//Lista autores
router.get('/listar', function(req, res) {
  let cmd = 'SELECT IdAutor, NoAutor, NoNacionalidade';
  cmd += ' FROM TbAutor AS a INNER JOIN TbNacionalidade AS n';
  cmd += ' ON a.IdNacionalidade = n.IdNacionalidade ORDER BY NoAutor';
  db.query(cmd, [], function(erro, listagem){
    if (erro){
      res.send(erro);
    }
    res.render('autores-lista', {resultado: listagem});
  });
});

//Adicionar autores
router.get('/add', function(req, res) {
  res.render('autores-add')
});

module.exports = router;
```

## 2. Configurando rotas no arquivo `App.js`
Para configurar as rotas no arquivo `App.js` nos importamos as rotas do arquivos com as rotas.
```javascript
let indexRouter = require('./routes/index');
let usersRouter = require('./routes/users');
let autoresRouter = require('./routes/autores');

//Chamamento das rotas.
app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/autores', autoresRouter);

//Chamamos as rotas a partir da rota principal, que pode ser a primeira rota que aparece no arquivo.
```
Chamamos as rotas a partir da rota principal, que pode ser a primeira rota que aparece no arquivo.

## 3. Modulerizando funções nas rotas
Também é uma boa pratica colocar as funções das rotas em arquivos separados. 
A exportação e importação de funções para rotas no Express pode ser feita usando 
a sintaxe de módulos do Node.js (`require` e `module.exports`) ou módulos ES (`import` e `export`). 

### Abordagem 1: Módulos Node.js (CommonJS)
Esta é a abordagem mais comum em projetos Express legados e funciona perfeitamente.

1. Crie um arquivo para suas funções/controladores (ex: controladores/usuarioController.js):
   ```javascript
   // controladores/usuarioController.js

   // Exemplo de função de controle para listar usuários
   exports.listarUsuarios = (req, res) => {
   // Lógica para buscar usuários do banco de dados, etc.
     res.status(200).send('Lista de usuários');
   };

   // Exemplo de função de controle para obter um único usuário
   exports.obterUsuario = (req, res) => {
     const userId = req.params.id;
     // Lógica para buscar um usuário específico
     res.status(200).send(`Detalhes do usuário ${userId}`);
   };
   ```
   
2. Importe e use as funções no seu arquivo de rotas (ex: app.js ou rotas/usuarioRotas.js):
   ```javascript
   // app.js (ou arquivo principal)

   const express = require('express');
   const app = express();
   const usuarioController = require('./controladores/usuarioController');

   // Define as rotas usando as funções importadas
   app.get('/usuarios', usuarioController.listarUsuarios);
   app.get('/usuarios/:id', usuarioController.obterUsuario);

   app.listen(3000, () => {
     console.log('Servidor rodando na porta 3000');
   });
   ```

### Abordagem 2: Módulos ES (ES Modules)
Esta abordagem é a sintaxe moderna do JavaScript. Para usá-la, você precisa garantir que seu projeto esteja 
configurado para suportar módulos ES (adicionando `"type"`: `"module"` no seu package.json ou usando a extensão de arquivo `.mjs`).

1. Crie o arquivo de funções usando export (ex: controladores/usuarioController.mjs):
   ```javascript
   // controladores/usuarioController.mjs

   // Exemplo de função de controle para listar usuários
   export const listarUsuarios = (req, res) => {
     res.status(200).send('Lista de usuários');
   };

   // Exemplo de função de controle para obter um único usuário
   export const obterUsuario = (req, res) => {
     const userId = req.params.id;
     res.status(200).send(`Detalhes do usuário ${userId}`);
   };
   ```

2. Importe e use as funções no seu arquivo de rotas usando import (ex: app.mjs):
   ```javascript
   // app.mjs

   import express from 'express';
   import { listarUsuarios, obterUsuario } from './controladores/usuarioController.mjs';

   const app = express();

   // Define as rotas usando as funções importadas
   app.get('/usuarios', listarUsuarios);
   app.get('/usuarios/:id', obterUsuario);

   app.listen(3000, () => {
      console.log('Servidor rodando na porta 3000');
   });
   ```

### Prática Recomendada: Usando `express.Router`
Em aplicações maiores, é comum modularizar ainda mais usando o objeto Router do Express.

1. Arquivo de rotas dedicado (ex: rotas/usuarioRotas.js):
   ```javascript
   // rotas/usuarioRotas.js

   const express = require('express');
   const router = express.Router();
   const usuarioController = require('../controladores/usuarioController'); // Usando CommonJS

   // Define as rotas específicas para usuários
   router.get('/', usuarioController.listarUsuarios);
   router.get('/:id', usuarioController.obterUsuario);

   module.exports = router; // Exporta o objeto router configurado
   ```

2. Arquivo principal (app.js) usando o roteador:
   ```javascript
   // app.js

   const express = require('express');
   const app = express();
   const usuarioRotas = require('./rotas/usuarioRotas');

   // Aplica todas as rotas do arquivo usuarioRotas sob o prefixo '/api/usuarios'
   app.use('/api/usuarios', usuarioRotas);

   app.listen(3000, () => {
     console.log('Servidor rodando na porta 3000');
   });
   ```




