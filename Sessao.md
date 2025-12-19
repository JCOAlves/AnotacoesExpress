# Utilizando a sessão (`session`) no Express
Uma sessão é um mecanismo para manter o estado de um usuário entre múltiplas requisições HTTP, armazenando dados temporários no
servidor e usando um cookie com ID de sessão no navegador para identificá-lo, permitindo recursos como login, carrinhos de
compras e preferências personalizadas, usando o middleware `express-session` para gerenciar isso de forma segura e eficiente. 

## 1. Instalando o express-session
1. No terminal do seu projeto, instale o pacote: 
    ```bash
    npm install express-session
    ```

## 2. Instalando o express-session
2. Importe o módulo e configure o middleware antes das suas rotas no arquivo principal (`App.js`):
    ```javascript
    const express = require('express');
    const session = require('express-session'); // Importa o módulo
    const app = express();

    // Configuração do middleware de sessão
    app.use(session({
        secret: 'uma-chave-secreta-muito-segura', // Essencial para assinar o cookie
        resave: true, // Salva a sessão mesmo se não modificada
        saveUninitialized: true, // Salva sessão para usuários não logados
        cookie: { secure: true } // Em produção, use secure: true e HTTPOnly
    }));

    // Exemplo de rota para usar a sessão
    app.get('/', (req, res) => {
        // Se a sessão 'views' não existir, inicia com 0, senão, incrementa
        req.session.views = (req.session.views || 0) + 1;
        res.send(`Você visitou esta página ${req.session.views} vezes.`);
    });

    // Exemplo de rota para fazer logout (destruir a sessão)
    app.get('/logout', (req, res) => {
        req.session.destroy(() => { // Destrói a sessão
            res.redirect('/'); // Redireciona para a home
        });
    });

    app.listen(3000, () => {
        console.log('Servidor rodando na porta 3000');
    });'

    ```

Como funciona
- Cookie: O express-session gera um ID único e o envia para o navegador do usuário em um cookie.
- Requisições subsequentes: O navegador envia o cookie de volta com cada requisição.
- req.session: O middleware usa o ID para carregar os dados da sessão (armazenados no servidor) no objeto req.session para você usar nas suas rotas.
- Segurança: Em produção, configure cookie: { secure: true, httpOnly: true, sameSite: 'strict' } para proteger contra ataques. 
Com esses passos, você implementa sessões para rastrear usuários e gerenciar o estado do login em sua aplicação Express. 


