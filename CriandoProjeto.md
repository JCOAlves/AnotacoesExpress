# Criando um projeto Express
## 1. Verificando o ambiente do projeto
Para iniciar um projeto Express é necessario primeiro verificar se o ```Node.js``` e o ```NPM``` (Node Package Manager) 
estão instalados e sua versão.

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
   
2. Agora, vamos instalar o Express:
   ```bash
   npm install --global --save express express-generator
   ```
   
3. Vamos criar um projeto chamado biblioteca. Precisamos informar que view-engine utilizada no projeto será a ejs. Digite o comando e observe o resultado::
   ```bash
   npx express --view=ejs biblioteca
   ```
   
4. Entre na pasta criada e instale as dependências:
   ```bash
   cd biblioteca
   ```
   ```bash
   npm install
   ```
   
5. Caso seja informado que existe alguma vulnerabilidade, execute o comando:
   ```bash
   npm audit fix --force
   ```
   caso apresente algum erro no comando:
   ```bash
   npx audit fix --force
   ```
   
7. Inicie o seu primeiro projeto Node.js, digitando o comando abaixo:
   ```bash
   npm start
   ```
8. Por fim, abra um browser e acesse o endereço ```localhost```, utilizando a porta ```3000```: ```localhost:3000```.
