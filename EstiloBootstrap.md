# Estilizando com Bootstrap
Bootstrap é um framework de front-end robusto, responsivo e que auxilia no desenvolvimento rápido de 
códigos HTML, CSS e JavaScript. Com o Bootstrap você pode utilizar diversos estilos e, para isso, basta
escolher aquele que mais te agrada. 

Comece com Bootstrap acessando o site: ("Bootstrap")["https://getbootstrap.com/docs/5.3/getting-started/introduction/"].

## 1. Adicionando Bootstrap no script
Para adicionar Bootstrap no scripts, basta adicionar os scripts abaixo no html:
- **CSS**: https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css
- **JavaScript**: https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <title>Listagem de Autores</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-
    alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-
    GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
    <link rel='stylesheet' href='/stylesheets/style.css' />
</head>
<body>
   
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-
    alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-
    w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN"
    crossorigin="anonymous"></script>
</body>
</html>
```

Assim você podem adicionar estilos, estrutura e elementos pré-determinados para sua página html.
```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <title>Listagem de Autores</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-
    alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-
    GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
    <link rel='stylesheet' href='/stylesheets/style.css' />
</head>
<body>
    <!-- Tabela com estilo Bootstrap -->
    <table class="table">
        <thead>
            <tr>
                <th scope = "col">#</th>
                <th scope = "col">Autor</th>
                <th scope = "col">Nacionalidade</th>
            </tr>
        </thead>
        <tbody>
            <% for (item of resultado) {%>
                <tr>
                    <td scope = "row"><%=item.IdAutor%></td>
                    <td scope = "row"><%=item.NoAutor%></td>
                    <td scope = "row"><%=item.NoNacionalidade%></td>
                </tr>
            <%}%>
        </tbody>
    </table>
   
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-
    alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-
    w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN"
    crossorigin="anonymous"></script>
</body>
</html>
```
