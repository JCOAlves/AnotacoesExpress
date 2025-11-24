# Criando conexão com o Banco de Dados MySQL
O banco de dados MySQL é onde estão armazenados os dados de uma aplicação. Para realizar a conexão do banco no Express é preciso ter o MySQL instalado na máquina. Além disso, é necessario instalar o módulo MySQL.

## 1. Instalação do módulo MySQL
1. Inicialize o servidor MySQL.
2. Na pasta do projeto, instale o pacote MySQL:
   ```bash
   npm install --save mysql
   ```
3. Na pasta do projeto, crie uma subpasta chamada ```utils```.
4. Na sequência, dentro da subpasta ```utils```, crie um arquivo chamado ```db.js```:
   ```javascript
   const mysql = require('mysql');
   const db = mysql.createConnection({
       host: 'localhost', //Maquina onde se localiza o banco
       user: 'root', //Nome de usuario do MySQL
       password: '', //Senha da conta MySQL
       port: 3306, //Porta logica onde corre o MySQL 
       database: 'dbbiblioteca', //Nome do banco de dados
       multipleStatements: true
   });

   db.connect(function(erro) {
     if (erro){
        throw erro;
     }
     console.log('Conectado ao BD...');
   });
   
   global.db = db;
   module.exports = db;
   ```
   **ATENÇÃO:** Por motivos de sergurança nós não subimos para o GitHub os dados de conexão do banco, pois permite a invasoes de usuaários mal-intencionados. Com isso, criamos um arquivo `.env` para armazenar dados da conexão, a qual deve ser ignorado no commit para o GitHub.

## 2. Retornando dados do banco
Para retornamos os dados do banco utilizamos o objeto da conexão `db`.
```javascript
let express = require('express');
let router = express.Router();
let db = require('../utils/db');

/* Outras rotas definidas anteriormente... */
router.get('/autores/listar', function(req, res) {
   db.query('SELECT * FROM TbAutor', [], function(erro, listagem){
      if (erro){
         res.send(erro);
      }
      res.send(listagem);
   });
});

module.exports = router;
```
- **`let db = require('../utils/db');`**: Importação do objeto da conexão MySQL
- **`router.get`**: Método GET HTTP
- **`'/autores/listar'`**: Rota GET do Express.
- **`db.query`**: Função Express de consulta ao banco. 
- **`'SELECT * FROM TbAutor'`**: Script MySQL para seleção dos dados do banco.
- **`[]`**: Lista com os parâmetros para a consulta, não sendo necessario para funções do método GET.
- **`function(erro, listagem)`**: Função com os parâmetros `erro`, a qual guarda e retorna um erro na consulta, se tiver, e `listagem`, onde guarda uma lista JSON  com os dados do banco, podendo ser nomeada da forma que quiser.
- **`res.send(listagem)`**: Retorna uma lista com os dados do banco MySQL em formato JSON.

## 3. Exibição dos dados
Não podemos exibir os dados da aplicação de forma bruta, então nos renderizamos os dados em arquivos HTML para a visualização do usuário. Aqui podemos exibir os dados no `ejs`, que são arquivos de template que permitem incorporar código JavaScript diretamente em HTML para criar páginas web dinâmicas no lado do servidor, localizado na pasta `views`.
```javascript
/* Lembrar das declarações: let express ... let db... let router... */
router.get('/autores/listar', function(req, res) {
   let cmd = 'SELECT IdAutor, NoAutor, NoNacionalidade ';
   cmd += ' FROM TbAutor AS a INNER JOIN TbNacionalidade AS n';
   cmd += ' ON a.IdNacionalidade = n.IdNacionalidade';
   cmd += ' ORDER BY NoAutor';
   db.query(cmd, [], function(erro, listagem){
      if (erro){
         res.send(erro);
      }

      /* Criaremos a view autores-lista no próximo slide*/
      res.render('autores-lista', {resultado: listagem});
   });
});

module.exports = router;
```
```ejs
<!DOCTYPE html>
<html>
   <head>
      <title>Listagem de Autores</title>
      <link rel='stylesheet' href='/stylesheets/style.css' />
   </head>
   <body>
      <h1>Listagem de Autores</h1>
      <p>
         <% for (item of resultado) {%>
            <%=item.IdAutor%> | <%=item.NoAutor%> | <%=item.NoNacionalidade%> <br>
         <%}%>
      </p>
   </body>
</html>
```
