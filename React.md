# Imprementando React com Express
React.js (ou React) é uma biblioteca JavaScript de código aberto usada para construir interfaces de usuário (UIs) interativas e reativas, dividindo-as em componentes reutilizáveis que gerenciam seu próprio estado, facilitando a criação de aplicações web complexas e de página única (SPAs) de forma eficiente. Assm, usar Express.js com React.js permite criar aplicações full-stack robustas, onde o Express atua como backend (API RESTful) gerenciando dados e regras de negócio, enquanto o React gerencia a interface do usuário no frontend. 

## 1. Criando um *Frontend* com React.js
Atualmente, o para criar uma interface de usuário se usa `Vite`, uma ferramenta de construção (build tool), 
extremamente rápida, criada para acelerar o desenvolvimento de aplicações `frontend`.

1. No terminal digite o comando:
    ```bash
    npm create vite@latest
    ```
2. No terminal aparecerá opções de ferramentas Frontend, incluindo o React.js, e linguagens de programação. Selecione a linguagem que preferir.
Se você quiser pular as perguntas e criar tudo em um único comando, use: 
    ```bash
    npm create vite@latest meu-app -- --template react-swc
    ```
3. Depois de terminal de configurar, digite no terminal:
    ```bash
    npm install react-router-dom
    ```
    Esse comando instala a bibliotec React responsavel pelo roteamento.

Ao abrir o projeto Frontend, você verá uma estrutura organizada:
- `index.html`: Fica na raiz (não na pasta public), o que permite ao Vite tratá-lo como parte do grafo de dependências.
- `src/main.jsx`: O ponto de entrada onde o React é montado no DOM.
- `vite.config.js`: Onde você adiciona plugins (como suporte para Tailwind ou aliases de pastas).

Para rodar o frontend digite no terminal: `npm run dev`. E assim o frontend será exercutado no endereço `localhost:5137`, onde `5137` é a porta padrão a aplicação.

## 2. Utilizando o **CORS**
Como você deve ter visto, o frontend React está rodando na porta `5137` enquando o backend na `3000`. Por padrão
ss navegadores utilizam o **CORS** (Cross-Origin Resource Sharing), que é um mecanismo de segurança dos navegadores que bloqueia requisições entre domínios diferentes, como no caso do frontend e backend. No Express, usa-se o pacote `cors` como middleware para configurar cabeçalhos HTTP (`Access-Control-Allow-Origin`) que autorizam essas origens, permitindo o compartilhamento de recursos. Para isso:

1. No terminal, instale o pacote com o comando:
    ```bash
    npm install cors
    ```

2. Uso básico (Permitir todas as origens - Geralmente para desenvolvimento):
    ```js
    const express = require('express');
    const cors = require('cors');
    const app = express();

    app.use(cors()); // Habilita CORS para todas as rotas e origens
    ```

3. Uso configurado (Permitir origens específicas - Produção):
    ```js
    const corsOptions = {
        origin: 'http://meuapp.com', // Apenas esta origem pode acessar
        optionsSuccessStatus: 200
    };
    app.use(cors(corsOptions));
    ```
4. **CORS** em uma única rota:
    ```js
    app.get('/api/data', cors(corsOptions), (req, res) => {
        res.json({ msg: 'Esta rota tem CORS específico' });
    });
    ```