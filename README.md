# Guia para criação de projetos de react

Este projeto tem como objetivo apresentar um passo a passo simples e direto de como criar e configurar um ambiente react que possua todos os principais recursos necessários para desenvolvimento de um projeto, seja grande ou pequeno.

## Recursos

Este projeto emprega como recursos básicos:

* Typescript
* Eslint
* Prettier
* Integração com o github

Além de uma gama de recursos adicionais, que podem ou não serem adicionados ao projeto dependendo da necessidade:

* Styled Components e Styled System
* React Router
* Redux com Redux-Thunk
* Redux Persist
* Axios
* Next.js
* React-hook-form com Yup

## Configurações básicas

O processo a seguir mostra como configurar um projeto básico de react com verificação estática de erros de código, de erros de tipagem e de boas práticas de programação. Lembre-se de que qualquer uma das etapas pode ser personalizada para atender às suas necessidades e preferências.

### Criação do projeto

Primeiramente, crie um projeto básico de react com typescript utilizando o criador de projetos disponibilizado pela empresa que criou o React:

```bash
npx create-react-app project-name --template typescript
cd project-name
```
O comando `npx` irá baixar as bibliotecas do criador de projetos e executá-lo em seguida, então não é necessário instalá-lo globalmente. Caso você não possua esse comando instalado em seu sistema, basta executar o comando abaixo:

```bash
npm install -g npx
```

Após o comando terminar a sua execução, um diretório novo com o nome que você deu para o projeto será criado, contendo a seguinte hierarquia:

```
/project-name
+-- /.git
+-- /node_modules
+-- /public
|   +-- favicon.icon
|   +-- index.html
|   +-- logo192.png
|   +-- logo512.png
|   +-- manifest.json
|   +-- robot.txt
+-- /src
|   +-- App.css
|   +-- App.test.tsx
|   +-- App.tsx
|   +-- index.css
|   +-- index.tsx
|   +-- logo.svg
|   +-- react-app-env.d.ts
|   +-- reportWebVitals.ts
|   +-- setupTests.ts
+-- .gitignore
+-- package-lock.json
+-- package.json
+-- README.md
+-- tsconfig.json
```

Por padrão, o projeto já vem com uma demo de projeto pré montada, contando imagens, ícones, manifesto da aplicação, testes de interface e algumas outras configurações. Além disso, o criador de projeto também define os scripts de desenvolvimento e de build no arquivo **package.json**:

```json
{
    "scripts": {
        "start": "react-scripts start",
        "build": "react-scripts build",
        "test": "react-scripts test",
        "eject": "react-scripts eject"
    },
}
```

assim como uma configuração inicial de integração com o github, a qual será abordada ao final desta seção.

Como ajuste inicial, mova as dependências de typescript para a seção de dependências de desenvolvedor, dado que o objetivo desses recursos é auxiliar o processo de desenvolvimento por meio da verificação estática de tipos:

```bash
npm install --save-dev typescript @types/jest @types/node @types/react @types/react-dom
```

```json
{
    "devDependencies": {
        "@types/jest": "^26.0.20",
        "@types/node": "^12.19.15",
        "@types/react": "^16.14.2",
        "@types/react-dom": "^16.9.10",
        "typescript": "^4.1.3"
    }
}
```

### Configuração do Eslint e do Prettier

Uma vez com o projeto criado, instale o eslint e o prettier, assim como as suas dependências:

```bash
npm install --save-dev eslint prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-plugin-react eslint-config-prettier eslint-plugin-prettier
```

Como acréscimo ao projeto, instale o plugin de react hooks do eslint para adicionar regras que ajudarão na consistência de lógicas de hooks em seus componentes:

```bash
npm install --save-dev eslint-plugin-react-hooks
```

Em seguida, criei um arquivo **.eslintrc** na raíz do projeto com o que é apresentado abaixo:

```json
{
    "parser": "@typescript-eslint/parser",
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:react-hooks/recommended",
        "plugin:@typescript-eslint/recommended",
        "prettier/@typescript-eslint",
        "plugin:prettier/recommended"
    ],
    "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        },
        "project": "./tsconfig.json"
    },
    "rules": {
        "prettier/prettier": "error",
        "react/prop-types": "off",
        "@typescript-eslint/no-explicit-any": "error"
    },
    "settings": {
        "react": {
          "version": "detect"
        }
    }
}
```

Essas definições servem para instruir o seu editor de código sobre como validar o seu projeto, ajudando a evitar erros de digitação, erros de lógica ou então más práticas de programação.

Faça a mesma coisa novamente, agora com outro arquivo chamado **.prettierrc**:

```json
{
    "bracketSpacing": true,
    "jsxBracketSameLine": true,
    "singleQuote": false,
    "trailingComma": "all",
    "tabWidth": 4
}
```

Já essas definições servem para ajudar a manter um padrão estético em seu código, muito importante para a saúde e manutenção de um projeto.

### Configurações do Editor

Até este momento, definimos todas as regras de integridade e estética nas quais o novo projeto deve se basear, contudo ele ainda está limitado ao fato de que você tem que manualmente corrigir quaisquer erros que possam vir a aparecer em seu código.

É possível remediar esse problema adicionando scripts ao arquivo **package.json** que apliquem rotinas de validação em todos os arquivos:

```json
{
    "scripts": {
        "format": "prettier --write src/**/*.{j,t}s{,x}",
        "lint": "tsc --noEmit && eslint src/**/*.{j,t}s{,x}"
    },
}
```

O que por si só já elimina a necessidade de ter que encontrar os erros. Porém, a alternativa mais eficiente seria configurar o próprio editor de código para realizar não só a validação, mas também a correções quando possível, do código toda vez que algum arquivo é salvo.

Para isso, cria-se primeiro um arquivo **.editorconfig** na raíz do projeto com as seguintes configurações:

```
root = true

[*]
end_of_line = lf
indent_style = space
indent_size = 4
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

E caso o seu editor seja o VS Code, adicione um arquivo **.vscode/settings.json** com o mostrado abaixo:

```json
{
    "window.zoomLevel": 1,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "typescript",
        "typescriptreact"
    ]
}
```

### Integração com o Github

Por fim, tendo configurado todo o seu ambiente de trabalho, basta agora integrar o projeto a um repositório no Github para facilitar o controle de versões do projeto e permitir trabalho em equipe.

Dado que o criador de projeto já inicializou o Github em seu projeto, primeiramente, crie um commit para registrar as alterações realizadas no repositório:

```bash
git add *
git commit -m "first commit" -a
```

Em seguida, crie um repositório novo no site do Github e, utilizando o link de acesso fornecido, crie uma conexão entre o seu ambiente local e o servidor:

```bash
git remote add origin https://github.com/user_name/repositori_name.git
```

E por fim, defina o nome que desejar para o ramo principal do repositório e envie as alterações para o servidor:

```bash
git branch -M master
git push -u origin master
```

O repositório será então preenchido pelo conteúdo do seu repositório local e você jã pode começar a personalizar o seu projeto. Como complemento, utilize o arquivo **.gitignore** para definir quais os arquivos e diretórios no projeto não devem ser considerados pelo github e o arquivo **.gitattributes** para definir quais propriedades o github deverá implementar nos arquivos do projeto.