# Guia para criação de projetos de react

Este projeto tem como objetivo apresentar um passo a passo simples e direto de como criar e configurar um ambiente react que possua todos os principais recursos necessários para desenvolvimento de um projeto, seja grande ou pequeno.

## Sumário

1. [Recursos](#Recursos)
2. [Configurações básicas](#Configurações-básicas)
2.1. [Criação do projeto](#Criação-do-projeto)
2.2. [Configuração do Eslint e do Prettier](#Configuração-do-Eslint-e-do-Prettier)
2.3. [Configurações do Editor](#Configurações-do-Editor)
2.4. [Integração com o Github](#Integração-com-o-Github)
3. [Integração com Styled Components e Styled System](#Integração-com-Styled-Components-e-Styled-System)
4. [Integração com React Router](#Integração-com-React-Router)
5. [Integração com Redux, Redux-Thunk e Redux-Persist](#Integração-com-Redux,-Redux-Thunk-e-Redux-Persist)

## Recursos

Este projeto emprega como recursos básicos:

* Typescript
* Eslint
* Prettier
* Integração com o github

Além de uma gama de recursos adicionais, que podem ou não serem adicionados ao projeto dependendo da necessidade:

* Styled Components e Styled System
* React Router
* Redux / Redux-Thunk / Redux Persist
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

## Integração com Styled Components e Styled System

Existem diversas formas diferentes de implementar estilos em componentes de React, desde arquivos básicos de css até bibliotecas especializadas. Dentre as diferentes opções, a biblioteca **Styled Componentes** se destaca entre as demais por empregar os mesmos recursos de SCSS, como sintáxe de CSS básica e composição relativa de estilos, ao mesmo tempo que permiti o uso das props fornecidas às tags de HTML na composição dos estilos, possibilitando uma alta responsividade e interatividade com os componentes.

```bash
npm install --save styled-components
npm install --save-dev @types/styled-components
```

O exemplo abaixo demonstra alguns dos recursos que essa biblioteca tem a oferecer:

```typescript
import React, { FC, ReactElement } from "react";

import styled, { css, ThemeProvider, CSSProperties } from "styled-components";

interface DefaultTheme {
    breakpoints: number[];
    variants: Record<string,{
        color: string;
        border: string;
        backgroundColor: string;
        fontSize: string;
    }>
}

enum VARIANTS {
    PRIMARY: "primary",
    SECONDARY: "secondary"
}

const theme: DefaultTheme = {
    breakpoints: [300, 600, 900, 1200],
    variants: {
        [VARIANTS.PRIMARY]: {
            color: black;
            border: "1px solid black",
            backgroundColor: "white",
            fontSize: "14px"
        },
        [VARIANTS.SECONDARY]: {
            color: white;
            border: "2px solid grey",
            backgroundColor: "black",
            fontSize: "16px"
        },
    }
}

type TButtonProps = {
    disabled: boolean;
    variant: VARIANT;
}

const Button = styled.button<CSSProperties & TButtonProps>`
    width: max-content;
    height: 40px;
    padding: 40px;
    display: flex;
    justify-content: center;
    align-items: center;

    ${({ disabled, variant, theme }) => {
        if(disabled) return css`
            background-color: #DDDDDD;
            border: 2px solid grey;
        `;

        const { backgroundColor, border } = theme.variants[variant]

        return css`
            background-color: ${backgroundColor};
            border: ${border};
        `;
    }}

    span {
        color: ${({ disabled, variant, theme }) => disabled
            ? "grey"
            : theme.variants[variant].color};
        font-family: sans-serif;
        font-size: ${({ variant, theme }) => theme.variants[variant].fontSize};
    }

    @media only screen and (max-width: ${({ theme }) => theme.breakpoints[1]}px) {
        width: 100%;
    }
`;

const App: FC = (): ReactElement => {
    return (
        <ThemeProvider theme={theme}>
            <Button disabled variant="primary">
                <span>Click me</span>
            </Button>
        </ThemeProvider>
    );
}
```

Em complemento a essa ferramenta, existe também uma outra biblioteca chamada **Styled System**, que facilita a implementação de atributos básicos de CSS, permitindo passá-los como props ao componente ao invés defini-los manualmente.

```bash
npm install --save styled-system
```

```typescript
import React, { FC, ReactElement } from "react";

import styled, { CSSProperties } from "styled-components";
import { Properties } from "csstype";

import {
    background,
    border,
    flexBox,
    layout,
    space,
    typography
} from "styled-system";

const Button = styled.button<>`
    ${background}
    ${border}
    ${flexBox}
    ${layout}
    ${space}

    span {
        ${typography}
    }
`;

const App: FC = (): ReactElement => {
    return (
        <Button
            backgroundColor="white"
            width="max-content"
            height="40px"
            border="1px solid black"
            padding="40px"
            display="flex"
            justify-content="center"
            align-items="center"
            color="black"
            fontFamily="sans-serif"
            fontSize="14px">
            <span>Click me</span>
        </Button>
    );
}
```

## Integração com React Router

A estrutura básica de um projeto de React é conhecida como SPA ou Single Page Application, o que em outras palavras significa que todo o projeto está contido em uma única página de HTML que é servida ao cliente no browser com um único grande bundle de javascript, independente de qual rota a url da aplicação apresente. Para o caso de aplicações pequenas, tal estrutura não é um problema, pois uma simples state-machine já seria o suficiente para separar os seus conteúdos, mas em muitos dos casos são necessárias mais do que algumas páginas para comportar o conteúdo do site como um todo, como por exemplo um site de blog no qual cada entrada da sua timeline renderia uma página isolada, determinada por parâmetros na url.

A biblioteca **React Router** vem com a proposta de remediar esse problema, implementando uma lógica que associa conteúdos específicos da aplicação com valores de rota na url, para então realizar o roteamento entre esses conteúdos dependendo da correspondência vingente na url. O projeto em si continua sendo uma SPA, porém esse processo de roteamento junto às alteração da url dão a impressão de que há de fato uma navegação ocorrendo o site, além de facilitar a separação dos conteúdos.

```bash
npm install --save react-router-dom
npm install --save-dev @types/react-router-dom
```

```typescript
import React, { FC, ReactElement } from "react";
import {
    BrowserRouter as Router,
    Switch,
    Route,
    Link
    RouteComponentProps
} from "react-router-dom";


const Home: FC = (): ReactElement => {
    return <h2>Home</h2>;
}


type TParams = {
    id: string;
}

const Product: FC = ({ match }: RouteComponentProps<TParams>): ReactElement => {
    return <h2>This is a page for product with ID: {match.params.id} </h2>;
}


const App: FC = ():ReactElement => {
    return (
        <Router>
            <div>
                <nav>
                    <ul>
                        <li>
                            <Link to="/">Home</Link>
                        </li>
                        <li>
                            <Link to="/product/1">Produto 1</Link>
                        </li>
                        <li>
                            <Link to="/product/2">Produto 2</Link>
                        </li>
                    </ul>
                </nav>

                <Switch>
                    <Route path="/" exact component={Home} />
                    <Route path="/product/:id" component={Product} />
                </Switch>
            </div>
        </Router>
    );
}
```

## Integração com Redux, Redux-Thunk e Redux-Persist