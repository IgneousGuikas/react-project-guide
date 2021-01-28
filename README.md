# Guia para criação de projetos de react

Este projeto tem como objetivo apresentar um passo a passo simples e direto de como criar e configurar um ambiente react que possua todos os principais recursos necessários para desenvolvimento de um projeto, seja grande ou pequeno.

## Sumário

1. [Recursos](#Recursos)
2. [Configurações básicas](#Configurações-básicas)
    1. [Criação do projeto](#Criação-do-projeto)
    2. [Configuração do Eslint e do Prettier](#Configuração-do-Eslint-e-do-Prettier)
    3. [Configurações do Editor](#Configurações-do-Editor)
    4. [Integração com o Github](#Integração-com-o-Github)
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

O que por si só já elimina a necessidade de ter que encontrar os erros. Porém, a alternativa mais eficiente seria configurar o próprio editor de código para realizar não só a validação, mas também a correção quando possível, do código toda vez que algum arquivo é salvo.

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

E caso o seu editor seja o VS Code, lembre de instalar as extensões do **Eslint** e do **Prettier** adicione um arquivo **.vscode/settings.json** com o mostrado abaixo:

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
    return <h2>This is a page for product with ID: {match.params.id}</h2>;
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

Dentre os vários desafios que um projeto de React acarreta, gerenciamento de estados é certamente um dos que abrange uma grande gama de possibilidades, envolvendo diversos métodos, diversos conceitos e diversas ferramentas.

Projetos simples com uma hierarquia rasa de componentes normalmente exigem pouco esforço nesse quesito, podendo se sustentar apenas com um componente pai que centralize os estados do projeto e funções de callback sendo passadas para os componentes filhos como props. Já para casos em que a hierarquia do projeto é mais profunda ou que a lógica da aplicação se torne mais complexa, como no caso de um projeto com React Router ou até mesmo com um framework de Next.js, tal estratégia torna-se muito convolucionada, uma vez que a transmissão de informações passa a ter muitos intermediários, exigindo soluções mais sofisticadas como a API de contextos da biblioteca de React, que, como o nome já diz, permite atribuir um contexto sobre um conjunto de dados a uma certa parcela da hierarquia de componentes, tornando-se diretamente acessível de qualquer componente dentro dela. Porém, à medida que o projeto começa a escalar, até mesmo essa abordagem pode vir a se tornar excessivamente complexa. Excesso de informações em um mesmo contexto, gerenciamento de requisições, code-spliting e permanência de dados são problemas comuns nessas situações.

Tendo tudo isso em mente, a biblioteca tratada por esta seção, **Redux**, assim como outras duas bibliotecas auxiliares, **Redux-Thunk** e **Redux-Persist**, procuram solucionar vários dos problemas mencionados, talvez que de uma forma um pouco mais exigente do ponto de vista conceitual, porém com uma maior robusteza e versatilidade em comparação a outras soluções.

### Redux

Trata-se de um framework para gerenciamento de estados globais que serve para qualquer tipo de aplicação com multiplos arquivos, não apenas para projetos de front-end. Após a implementação, os dados os quais deseja-se que sejam d caráter global, assim como as lógicas necessárias para se acessar e alterar esses dados, são embuidos uma camada de processamento separada, paralela à de execução da aplicação, criando assim uma fonte única de informações acessível ao projeto como um todo. No caso de um projeto de React, isso consiste em permitir que qualquer componente, seja pequeno ou grande, tenha acesso a estados que armazenem dados ou transmitam ações para outros componentes isolados, resolvendo dessa forma o problema da hierarquia na aplicação.

Em complemento a isso, a lógica do Redux não se limita um único padrão de implementação, possibilitando diversas alternativas, incluindo abordagens extremamente modulares que permitem separar as funcionalidades da aplicação ao mesmo tempo que elas continuam acessíveis ao projeto como um todo.

```bash
npm install --save react-redux
```

O exeplo abaixo demonstra uma dessas formas modulares de se implementar o Redux a um projeto de React. A funcionalidade escolhida foi a de um contador com valor ajustável de acréscimo e será reutilizada posteriormente nas próximas seções.

```
/pages
+-- /page1
|   +-- index.tsx
+-- index.tsx
/store
+-- /modules
|   +-- /func1
|       +-- actions.ts
|       +-- interface.ts
|       +-- reducer.ts
|       +-- selector.ts
|   +-- index.ts
|   +-- interface.ts
+-- index.ts
```

A base de uma funcionalidade de Redux se resume a 3 componentes distintos: *states*, *reducers* e *actions*. Um *state* é a estrutura que contém qualquer dado que faça parte da lógica da funcionalidade, sendo em geral uma lista ou um objeto. Como uma de suas premissas básicas, o Redux deve atuar como uma fonte única da verdade à aplicação e, portanto, todo e qualquer *state* definido deve ser imutável, o que em outras palavras significa que os *states* nunca devem ser alterados diretamente, apenas substituídos por novos *states*. Já um *reducer* consiste em uma função cuja tarefa é realizar alterações no *state* correspondente de acordo com as especificações de uma *action*, que por sua vez nada mais é do que um objeto que deve conter um elemento "type" para indicar o tipo de alteração que o *state* deve sofrer pela ação do *reducer* e, opcionalmente, um elemento "payload" para caso tais alterações necessitem de dados extras ou novos dados. *Actions* podem ser passadas manulamente para os *reducers*, porém é mais prático definir funções, normalmente denominadas de *creators*, que apenas montem os objetos nos formatos certos e repassem os dados de payload caso sejam necessários.

```typescript
// /store/modules/func1/interface.ts

import { types } from "./actions";
import { TAction } from "../interface";

export type TFunc1State = {
    count: number;
    step: number;
}

export type TFunc1Actions =
    | TAction<
        types.ADD
        | types.SUB
        | types.RESET
    >
    | TAction<types.SET_STEP, number>;
```

```typescript
// /store/modules/func1/actions.ts

import { TAction } from "../interface";

export enum types {
    ADD = "func1/add",
    SUB = "func1/sub",
    RESET = "func1/reset",
    SET_STEP = "func1/set/step"
}

export const addCount = (): TAction<types.ADD> => ({ type: types.ADD });

export const subCount = (): TAction<types.SUB> => ({ type: types.SUB });

export const resetCount = (): TAction<types.RESET> => ({ type: types.RESET });

export const setStepCount = (newStep: number): TAction<types.SET_STEP, number> => ({
    type: types.SET_STEP,
    payload: newStep
});
```

```typescript
// /store/modules/func1/reducer.ts

import { types } from "./actions";
import { TFunc1State, TFunc1Actions } from "./interface";

const initialState: TState = {
    count: 0,
    step: 1,
};

const reducer = (state = initialState, action: TFunc1Actions): TState => {
    switch(action.type) {
        case types.ADD:
            return {
                ...state,
                count: state.count + state.step
            };
        case types.SUB:
            return {
                ...state,
                count: state.count - state.step
            };
        case types.SET_STEP:
            return {
                ...state,
                step: action.payload
            };
        case types.RESET:
            return { ...initialState };
        default:
            return { ...state };
    }
}

export default reducer;
```

A lógica da funcionalidade em si já está contida nesses três arquivos acima. O arquivo **actions.ts** define os tipos de ações que podem ser efetuadas, no caso somar ou subtrair o valor de *step* uma vez do valor da contagem, definir o valor do *step* e reiniciar o *state* da funcionalidade para os valores iniciais, assim como também declara os *creators* que facilitarão o envio das *actions* para o *reducer*. O arquivo **reducer.ts**, por sua vez, declara o valor inicial do *state* e define as operações respectivas a cada tipo de *action* e, por fim, o arquivo **interface.ts** ajuda a organizar as estruturas envolvidas na funcionalidade.

```typescript
// /store/modules/index.ts

import { combineReducers } from "redux";

import func1 from "./func1/reducer";

const rootReducer = combineReducers({
    func1,
});

export default rootReducer;
```

```typescript
// /store/modules/interface.ts

import { Action } from "redux";

import rootReducer from "./";

import { TFunc1Actions } from "./func1/interface";

export type TStore = ReturnType<typeof rootReducer>;

export type TActions = TFunc1Actions;

export type TAction<TType, TPayload = void> = {
    type: TType;
    payload: TPayload;
}
```

```typescript
// /store/index.ts

import { createStore } from "redux";

import rootReducer from "./modules";
import { TActions, TStore } from "./modules/interface";

const appReducer = (state: TStore, action: TAction) => rootReducer(state, action);

export default createStore(appReducer);
```

```typescript
// /pages/index.tsx

import { FC, ReactElement } from "react";
import { Provider } from "react-redux";
import {
    BrowserRouter as Router,
    Switch,
    Route,
} from "react-router-dom";

import store from "../store";

import Page1 from "./page1";

const App: FC = ():ReactElement => {
    return (
        <Provider store={store}>
            <Router>
                <Switch>
                    <Route path="/" exact component={Page1} />
                </Switch>
            </Router>
        </Provider>
    );
}
```

```typescript
// /store/modules/func1/selectors.ts

import { TFunc1State } from "./interface";
import { TStore } from "../interface";

export const getState = (state: TStore): TFunc1State => state?.func1 ?? {};
```

```typescript
// /pages/page1/index.tsx

import { FC, ReactElement } from "react";
import { useDispatch, useSelector } from "react-redux";

import {
    addCount,
    subCount,
    resetCount,
    setStepCount,
} from "../store/modules/func1/actions";
import {  }
```