# Guia para criação de projetos de react

Este projeto tem como objetivo apresentar um passo a passo simples e direto de como criar e configurar um ambiente react que possua todos os principais recursos necessários para desenvolvimento de um projeto, seja grande ou pequeno.

## Recursos
---

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

## Configuraçôes básicas
---

Primeiramente, crie um projeto básico de react com typescript utilizando o criador de projetos disponibilizado pela empresa que criou o React:

```bash
npx create-react-app project-name --template typescript
```
O comando `npx` irá baixar as bibliotecas do criador de projetos e executá-lo em seguida, então não é necessário instalá-lo globalmente. Caso você não possua esse comando instalado em seu sistema, basta executar o comando abaixo:

```bash
npm install -g npx
```

Após o comando terminar a sua execução, um diretório novo com o nome que você deu para o projeto será criado, contendo a seguinte hierarquia:

```
project-name
|
+-- node_modules (pasta)
+-- public
|   +-- favicon.icon
```