# Criando um projeto Express
O Express é um framework web para Node.js que facilita e acelera a criação de aplicativos e APIs back-end, 
oferecendo recursos como roteamento, tratamento de requisições HTTP e middleware. Ele funciona como uma camada de 
abstração sobre o Node.js, que é o ambiente de runtime que executa JavaScript no servidor.

## 1. Verificando o ambiente do projeto
Para iniciar um projeto Express é necessario primeiro verificar se o ```Node.js``` e o ```NPM``` (Node Package Manager) 
estão instalados e sua versão, de preferência a mais recente.

**Verificando instalação do Node.js:**
  ```bash
  node --version
  ```

**Verificação da instalção do NPM:**
  ```bash
  npm --version
  ```
## 2. Criando um projeto Express
1. No prompt, digite o comando abaixo para acessar a pasta onde será criado o projeto:
   ```bash
   cd desktop\projetos-pabd
   ```
   
2. Agora, para criar o projeto Express, digite os comandos inline:
   ```bash
   mkdir projeto && cd projeto
   ```
   - `mkdir projeto`: Comando para criar o diretorio do projeto.
   - `cd projeto`: Entra no diretorio recem criado.
   
3. Depois de  entra no diretorio do projeto, digite:
   ```bash
   npm init -y
   ```
   Este comando cria o arquivo `package.json` com todas as configurações padrão (**y**es). Sem o `-y`, o terminal ficaria te fazendo perguntas como "Qual o nome do autor?" ou "Qual a licença?".
   
4. Depois de criar o arquivo `package.json`, instale o express no projeto como o comando:
   ```bash
   npm install express
   ```
   Para verificar se o express está instalado, digite o comando:
   ```bash
   npm list express
   ```
   
5. Depois de instado o express, no package.json, configure:
   - `"type": "module"`: Ativa o suporte nativo a ES Modules, permitindo que você use `import/export`.
   - `"dev": "node --watch index.js"`: Cria um script de automação. O parâmetro `--watch` (nativo do Node.js recente) monitora mudanças no código e reinicia o servidor sozinho.
   ```json
   {
      "name": "projeto",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
         "test": "echo \"Error: no test specified\" && exit 1",
         "dev": "node --watch index.js"
      },
      "keywords": [],
      "author": "",
      "license": "ISC",
      "type": "module",
      "dependencies": {
         "express": "^5.2.1"
      }
   }
   ```

6. Crie os arquivos iniciais do projeto:
   - `index.js`: Onde seu servidor nasce.
   - `.env`: Onde você guardará as chaves secretas.
   - `.gitignore`: Fundamental para dizer ao Git: "Não suba a pasta `node_modules` nem o meu `.env` para o GitHub".
   Você pode criar cada por vez, ou pode usar o comando:
   ```bash
   touch index.js .env .gitignore
   ```
   O qual cria os arquivos básicos vazios.
   > **Dica**: Se você estiver no **Windows PowerShell**, o comando `touch` pode não funcionar nativamente. Nesse caso, você pode substituir `touch index.js .env .gitignore` por `New-Item index.js, .env, .gitignore`.

7. Para rodar o projeto, execute o comando:
   ```bash
   npm run dev
   ```
