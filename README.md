# Aula de Introdução a vueRouter e components

## requisitos do ambiente virtual
É necessário para fazer este tutorial que em sua máquina esteja intalado:

- [Visual Studio Code](https://code.visualstudio.com)
- [Extensões Recomendadas](#extensões)
- [Nodejs](https://nodejs.org/)

Caso não tenha algun destes requisitos em sua máquina, vá para a [configuração do abiente virtual](#configuração-do-ambiente-virtual) no fim desta página.

# 1-Criação do Projeto

- Crie uma pasta para o projeto
- Após criada a pasta abra-a no VS code
- Agora inicie um projeto em vue:
  ```shell
  npm init vue@latest .
  ```
  - IMPORTANTE! Não esquedo do ponto no final
  - Aceite as seguintes especificações:

![especificações de projeto](./imagensreadme/especificacoes.png)

- Rode o seguinte comando para instalar as dependêcias:
  ```shell
  npm install
  ```
- vá para a pasta src em assets e exclua todo o conteúdo do arquivo ``main.css``

![assets](imagensreadme/assets.png "assets")

- Exclua todos os arquivos dentro de ``components`` e também o arquivo ``AboutView.vue`` dentro de ``views``
- Após isso exclua o seguinte código do ``index.js`` dentro de routers 

```js
{
      path: '/about',
      name: 'about',
      // route level code-splitting
      // this generates a separate chunk (About.[hash].js) for this route
      // which is lazy-loaded when the route is visited.
      component: () => import('../views/AboutView.vue')
    }
```
- deixando apenas:

```js
import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView
    },
  ]
})

export default router

```

- E Também exclua o conteúdo de ``App.vue`` e , deixando apenas:
```html
<script setup>

</script>

<template>
 
</template>

<style scoped>

</style>

```

- faça o mesmo com ``HomeView.vue`` dentro de views

- Agora execute o seguite comando para o projetos rodar em uma porta local:

```shell
npm run dev
```
- IMPORTANTE! A partir de agora abra outro terminal e mantenha este rodando para ver o andamento da aplicação

Pronto seu projeto foi criado

# 2-Components

Um ``component`` é como o nome sugere um componente de uma página. O grande diferencial de um component do ``Vue`` para uma ``div`` qualquer é o armazenamento de script, CSS e reutilizabilidade. 

Por exemplo um botão que se repete diversas vezes em diversas páginas, normalmente você teria que recria o botão em todas as páginas com o CSS e script, porém com o ``Vue`` você cria este botão apenas uma vez e apenas o importa para as páginas que deseja.

Para ver isso na prática criaremos dois ``components``, um menu de navegação e um formulário.

- Crie um arquivo chamado ``MenuNav.vue`` em components
- Deixe-o como o ``App.vue``
- Agora crie um ``header`` a seu gosto no ``template``
- Estilize-o como desejar
- Agora Dentro de ``App.vue`` importe o component
  ```html
  <script setup>
    import MenuNav from './components/MenuNav.vue'
  </script>
  ```
- Agora adicione-o ao app: 
    ```html
    <template>
        <header>
            <MenuNav />
        </header>
    </template>
    ``` 
  - Perceba que agora o menu é exibido mesmo não tendo sido criado no ``App.vue``
  - É desta maneira para todos os components que desejar adicionar  
- Agora para fazer o formulário crie o arquivo ``FormUser.vue``
- E adicione o código:
```html
<template>
    <form>
        <label for="nome">nome</label>
        <input type="text" name="nome">
        <label for="email">email</label>
        <input type="email" name="email">
        <label for="idade">idade</label>
        <input type="number" name="idade">
    </form>
</template>

<style>
form {
    display: flex;
    flex-direction: column;
}
input {
    max-width: 15%;
}
</style>
```
- Agora Importe-o para ``HomeView.vue``:
```html
<script setup>
import FormUser from '../components/FormUser.vue';
</script>
```
- E aplique-o dentro da View:

```html
<template>
  <main>
    <h1>Pagina inicial</h1>
    <FormUser />
  </main>
</template>
```
- Perceba que o formulário não é exibido. Arrumaremos isto no Próximo passo

# 3-VueRouter

VueRouter é a maneira de fazer uma applição com mais de uma página no vue, para isso nós utilizamos as ``views``, que são diferentes telas de visualização

Um exemplo é a ``HomeView.vue`` que ja utilizamos anteriormente

Agora criaremos outra view para utilizarmos e aprensetaremos as views para o usuário

- Para exibir a view que ja existe utilizaremos o ``RouterView`` para isso vamos importa-lo no app:
```html
<script setup>
import { RouterView } from 'vue-router'
...
</script>
```
- Agora vamos apresentar a view para o usuário da mesma maneira que fizemos com o component no app:
```html
<template>
    ...
 <RouterView />
</template>
```

- Perceba que agora o Formulário é exibido. Isso acontece porque o alocamos na view ``HomeView.vue`` que antes não era exibida
- Agora criaremos uma segunda view para que possamos alternar entre elas
- Crie outro um arquivo chamado ``PicanhaView.vue`` dentro da pasta ``views`` e adicione a seguinte lista:
```html
<template>
    <h1>PICANHA</h1>
    <h2>preços</h2>
    <ul>
        <li>20</li>
        <li>50</li>
        <li>30</li>
        <li>70</li>
    </ul>
</template>

```
- Agora importe e aloque o component ``FormUser`` da mesma maneira que fizemos anteriormente na Home
- Agora iremos permitir que o usuário alterne entre as ``views``
- Para isso vamos utilizar o ``RouterLink`` e component ``MenuNav`` que criamos anteriormente
- Antes disso vá em ``index.js`` na pasta router e adicione o seguinte código:
```js
routes: [
    ...
    {
      path: '/picanha',
      name: 'picanha',
      component: () => import('../views/PicanhaView.vue')
    }
  ]
```
- Aqui estamos definindo que a url ``http://localhost:5173/picanha`` irá exibir a view ``PicanhaView.vue``
- Agora importe o ``RouterLink`` no component ``MenuNav.vue``:
```html
<script setup>
import { RouterLink } from 'vue-router'
</script>
```
- Aloque-os da seguinte maneira:
```html

<template>
    <RouterLink to="/">home</RouterLink>
    <RouterLink to="/picanha">picanha</RouterLink>    
</template>
```
- Aqui o ``RouterLink`` funciona semelhante a uma tag ``a``, desta maneira dizemos que o ``home`` irá alterar o url para ``http://localhost:5173/`` e picanha irá altera-la para ``http://localhost:5173/picanha``
- Perceba que agora é possível alternar entre as views

Assim concluímos este tutorial. Qualquer contribuição/sugetão é bem vinda.
 
# configuração  do ambiente virtual

A configuração só precisa ser feita uma vez em cada computador

## instalação do VS code
Para instalar o vs code siga os seguintes passos:

**No Ubuntu/Mint e derivados:**

Digite o seguinte código no terminal do linux:

```shell
sudo apt install code
```

**No Manjaro:**

Digite o seguinte código no terminal do linux:

```shell
yay -Syu visual-studio-code-bin
```

**No Windows**

1. Acesse o site do [VS code](https://code.visualstudio.com)
2. Clique em "Dowload para Windows" ou "Dowload for Windows" dependendo do navegador
3. Após instalar o arquivo de setup rode-o, aceite oque tiver que aceitar e pronto. Seu VS code está pronto para o uso

## Extensões
As extensões recomendadas para o uso do vueJs são:

- [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Portuguese (Brazil) Language Pack](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-pt-BR)
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=code-spell-checker.code-spell-checker)
- [Brazilian Portuguese - Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker-portuguese-brazilian)
- [vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons)

Para instalar as extensões basta pesquisa-las na aba de extensões do VS code ou clicar nos links e instalar

## Instalização ou atualização do Nodejs

O [Vue.js](https://vuejs.org) é um framework de [Nodejs](https://nodejs.org/), portanto necessita que ele esteja instalado e atualizado.

Para intalar ou atualizar:

**No Linux**

Recomenda-se utilizar o `nvm`,  O nvm é gerenciador de versões do NodeJs, desenvolvido para ser instalado utilizando a conta de um usuário final.

Para instalar ou atualizar o `nvm` utilize o seguite código no terminal (Não faz difereça ser no do linux ou do VS code):

~~~shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
~~~

Após isso feche o terminal, abra um novo e instale a versão LTS do node:

~~~shell
nvm install --lts
~~~

Pronto o node está instalado e atulizado pronto para o uso.

**No Windows/Mac**

Para instalar o node no Windows acesse o site do [Nodejs](https://nodejs.org/en) e siga estes passos:

1. Vá para a aba de Dowload
2. Altere a versão que deseja instalar para LTS, e selecione as configuraçõe da máquina como na imagem:
   ![imagem de instalção do node](./imagensreadme/node.png)
3. Após a instalação do arquivo rode-o, aceite oque tiver que aceitar e pronto. Seu node está instalado e pronto para o uso.