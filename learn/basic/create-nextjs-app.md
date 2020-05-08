[< Pagina Inicial](../../README.md#basico)
<h1 align="center">Criando 'APP' Next.js</h1>

## Introdução

Para criar um aplicativo Web completo com o React do zero, há muitos detalhes importantes que você precisa considerar:

  - O código deve ser empacotado usando um empacotador como o webpack e transformado usando um compilador como o Babel.

  - Você precisa fazer otimizações de produção, como divisão de código.

  - Convém pré-renderizar estaticamente algumas páginas para um melhor desempenho e SEO. Você também pode usar a renderização server-side ou client-side.

  - Pode ser necessário escrever um código de server-side para conectar seu aplicativo React ao seu banco de dados.

Uma framework pode resolver esses problemas. Mas essa framework deve ter o nível certo de abstração - caso contrário, não será muito útil. Ela também precisa ter uma excelente "Experiência do desenvolvedor", garantindo que você e sua equipe tenham uma experiência incrível ao escrever o código.

## Next.js: O Framework do React

Digite Next.js, o Framework do React. O Next.js fornece uma solução para todos os problemas acima. Mas o mais importante é que você e sua equipe estão chegando no chamado 'pit of success' ao criar aplicativos com React.

O Next.js é o melhor da classe "Experiência do desenvolvedor" e de muitos recursos internos; uma amostra deles são:

  - Um sistema intuitivo de roteamento baseado em página (com suporte para rotas dinâmicas)

  - A pré-renderização, a geração estática (SSG) e a renderização server-side (SSR) são suportadas por página base.

  - Divisão automática de código para carregamento de página mais rápido

  - Client-side com pré-busca otimizada

  - Suporte integrado a CSS e Sass e suporte para qualquer biblioteca CSS-in-JS

  - Ambiente de desenvolvimento que suporta substituição de Hot Modules

  - Rotas de API para criar pontos de extremidade de API com funções Serverless.

  - Totalmente extensível

O Next.js é usado em dezenas de milhares de sites e aplicativos da Web voltados para produção, incluindo muitas das maiores marcas do mundo.

## Sobre esse Tutorial

Este curso interativo gratuito orientará você sobre como começar a usar o Next.js.

Neste tutorial, você aprenderá os conceitos básicos do Next.js criando um aplicativo de blog muito simples. Aqui está um exemplo do resultado final:

https://next-learn-starter.now.sh

([source](https://github.com/zeit/next-learn-starter/tree/master/demo))

Vamos começar!

> Este tutorial pressupõe conhecimentos básicos de JavaScript e React. Se você nunca escreveu o código em React, deve primeiro ler o tutorial oficial do React.
> Se você está procurando a documentação do Next.js, [clique aqui](https://nextjs.org/docs/getting-started).

## Setup

Primeiro, verifique se seu ambiente de desenvolvimento está pronto.

 - Se você não possui o *Node.js* instalado, [instale-o aqui](https://nodejs.org/en/). Você precisará do Node.js. versão *10.13* ou posterior.

 - Você usará seu próprio editor de texto e terminal para este tutorial.

Se você estiver no Windows, recomendamos o download do [Git for Windows](https://gitforwindows.org/) e use o Git Bash que vem com ele, que suporta os comandos específicos do UNIX neste tutorial.

## Crie um aplicativo Next.js.

Para criar um aplicativo Next.js, abra seu terminal, vá no diretório em que você deseja criar o aplicativo e execute o seguinte comando:

```bash
npm init next-app nextjs-blog --example "https://github.com/zeit/next-learn-starter/tree/master/learn-starter"
```

De baixo dos panos, isso usa a ferramenta chamada `create-next-app`, que inicia um aplicativo Next.js. para você. Ele usa [esse modelo](https://github.com/zeit/next-learn-starter/tree/master/learn-starter) através do sinalizador `--example`.

## Execute o servidor de desenvolvimento

Agora você tem um novo diretório chamado `nextjs-blog`. Vamos entrar nele:

```bash
cd nextjs-blog
```

Em seguida, execute o seguinte comando:

```bash
npm run dev
```

Isso inicia o "servidor de desenvolvimento" do aplicativo em Next.js. (mais sobre isso posteriormente) na porta 3000.

Vamos verificar se está funcionando. Abra http://localhost:3000 no seu navegador.

Pergunta: Que texto você vê na página?

- [ ] Welcome to Next.js!
- [ ] Hello Next.js!

## Bem-vindo ao Next.js

Você deverá ver uma página como essa ao acessar http://localhost:3000. Esta é a página de modelo inicial que mostra algumas informações úteis sobre o Next.js.

![initial page](../../images/welcome-to-nextjs.png)

A ajuda está disponível: se você ficar preso, poderá entrar em contato com a comunidade nas [Discussões do GitHub](https://github.com/zeit/next.js/discussions).

Vamos tentar editar esta página a seguir!

## Editando a página

Vamos tentar editar a página inicial.

  - Verifique se o servidor de desenvolvimento Next.js. ainda está em execução.

  - Abra o `pages/index.js` com seu editor de texto.

  - Localize o texto que diz "*Welcome to*" sob a tag `<h1>` e altere-o para "*Learn*".

  - Salve o arquivo.

Assim que você salva o arquivo, o navegador recarrega e atualiza automaticamente a página com o novo texto:

![Learn Next.Js!](../../images/learn-nextjs.png)

O servidor de desenvolvimento Next.js. possui o recurso Hot Reloading. Quando você faz alterações nos arquivos, o Next.js aplica automaticamente as alterações no navegador. Nenhuma atualização necessária! Isso ajudará você a interagir rapidamente no seu aplicativo.

## Em seguida: Criando páginas

Bom trabalho! É isso na primeira lição.

Na próxima lição, falaremos sobre a criação de mais páginas e a navegação entre páginas.

> Você deve manter o servidor de desenvolvimento em execução, mas se quiser reiniciá-lo, pressione `Ctrl+C` para parar o servidor.

<h1 align="center">
<a href="../../README.md#basico">
  <img src="../../images/next-arrow.svg" alt="next-arrow" width="40px">
</a>
</h1>

<a href="../../README.md#basico/README.md#basico" align="center">< Pagina Inicial</a>
