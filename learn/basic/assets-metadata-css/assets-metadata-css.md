[< Pagina Inicial](../../README.md#basico)

<h1 align="center">Criando 'APP' Next.js</h1>

## Recursos, Metadados e CSS

A segunda página que adicionamos atualmente não possui estilo. Vamos adicionar um pouco de CSS para estilizar a página.

O Next.js possui suporte interno para CSS e Sass. Para os propósitos deste curso, usaremos CSS.

Esta lição também abordará como o Next.js lida com recursos estáticos, como imagens e metadados da página, como tags de `título`.

## O que você aprenderá nesta lição

Nesta lição, você aprenderá:

  - Como adicionar arquivos estáticos (imagens, etc) ao Next.js.

  - Como personalizar o que está dentro do `<head>` para cada página.

  - Como criar um componente React reutilizável com estilo usando Módulos CSS.

  - Como adicionar CSS global em pages/_app.js.

  - Algumas dicas úteis para estilizar no Next.js.

>Se você está procurando uma documentação detalhada sobre o estilo Next.js., consulte a [documentação CSS](https://nextjs.org/docs/basic-features/built-in-css-support).

## Recursos

Primeiro, vamos falar sobre como o Next.js lida com **recursos estáticos**, como imagens.

O Next.js pode servir arquivos estáticos, como imagens, no diretório `public`. Os arquivos dentro de `public` podem ser referenciados a partir da raiz do aplicativo, semelhante as `pages`.

Se você abrir o `pages/index.js` no seu aplicativo e der uma olhada no `<footer>`, nos referimos à imagem do logotipo da seguinte forma:

```javascript
Powered by <img src="..." alt="..." />
```

A imagem do logotipo existe dentro do diretório `public` no nível superior do seu aplicativo.

O diretório `public` também é útil para `robots.txt`, Verificação do site do Google e outros ativos estáticos. Consulte a [documentação de vinculação de arquivos estáticos](https://nextjs.org/docs/basic-features/static-file-serving) para saber mais.

## Metadados

E se desejássemos modificar os metadados da página, como a tag HTML `<title>`?

`<title>` faz parte da tag HTML `<head>`, então vamos ver como podemos modificar a tag `<head>` em uma página Next.js.

Abra o `pages/index.js` no seu editor e veja as seguintes linhas:

```javascript
<Head>
  <title>Create Next App</title>
  <link rel="icon" href="/favicon.ico" />
</Head>
```

Observe que `<Head>` é usado em vez da `<head>` minúscula. `<Head>` é um componente React incorporado ao Next.js. Permite modificar o `<head>` de uma página.

Você pode importar o componente `Head` do módulo `next/head`.

## Adicionando `<Head>` ao `first-post.js`

Não adicionamos um `<title>` à nossa rota /`posts/first-post`. Vamos adicionar um.

Abra o arquivo `pages/posts/first-post.js`.

Primeiro, importe o `Head` do `next/head`:

```javascript
import Head from 'next/head'
```

Em seguida, adicione-o ao componente `FirstPost`. Por enquanto, adicionaremos apenas a tag `title`:

```javascript
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      …
    </>
  )
}
```

Tente acessar http://localhost:3000/posts/first-post. A guia do navegador deve agora dizer "First Post". Ao usar as ferramentas de desenvolvedor do seu navegador, você deve ver que a tag `title` é adicionada ao `<head>`.

>Para saber mais sobre o `next/head`, consulte a [documentação de referência da API](https://nextjs.org/docs/api-reference/next/head).
>
>Se você deseja personalizar o `<html>`, por exemplo, para adicionar o atributo `lang`, você pode fazer isso criando um componente de `Document` personalizado. Saiba mais na [documentação do documento personalizado](https://nextjs.org/docs/advanced-features/custom-document).

## Estilo CSS

Vamos agora falar sobre o estilo CSS.

Como você pode ver, nossa página de índice (http://localhost:3000) já possui algum estilo. Se você der uma olhada em `pages/index.js`, deverá ver o código assim:

```javascript
<style jsx>{`
  …
`}</style>
```

Isso está usando uma biblioteca chamada [styled-jsx](https://github.com/zeit/styled-jsx). É uma biblioteca "CSS-in-JS" - permite escrever CSS em um componente React e os estilos CSS terão escopo definido (outros componentes não serão afetados).

O Next.js possui suporte interno para [styled-jsx](https://github.com/zeit/styled-jsx), mas você também pode usar outras bibliotecas CSS-in-JS populares, como [componentes estilizados](https://github.com/zeit/next.js/tree/canary/examples/with-styled-components) ou [animados](https://github.com/zeit/next.js/tree/canary/examples/with-emotion).

## Escrevendo e importando CSS

O Next.js possui suporte interno para CSS e Sass, o que permite importar arquivos `.css` e `.scss`.

O uso de bibliotecas CSS populares, como o [Tailwind CSS](https://github.com/zeit/next.js/tree/canary/examples/with-tailwindcss), também é suportado.

Nesta lição, falaremos sobre como escrever e importar arquivos CSS no Next.js. Também falaremos sobre o suporte interno do Next.js. para [módulos CSS](https://github.com/css-modules/css-modules) e [Sass](https://sass-lang.com/). Vamos mergulhar!

## Layout Component

Primeiro, vamos criar um componente de **layout** que será comum em todas as páginas.

  - Crie um diretório de top-level chamado `components`.

  - Dentro, crie um arquivo chamado `layout.js` com o seguinte conteúdo:

```javascript
function Layout({ children }) {
  return <div>{children}</div>
}

export default Layout
```

Em seguida, em `pages/posts/first-post.js`.js, importe o `Layout` e faça dele o componente mais externo.

```javascript
import Head from 'next/head'
import Link from 'next/link'
import Layout from '../../components/layout'

export default function FirstPost() {
  return (
    <Layout>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </Layout>
  )
}
```

## Adicionando CSS

Agora, vamos adicionar alguns estilos para o `Layout`. Para fazer isso, usaremos [módulos CSS](https://github.com/css-modules/css-modules), que permitem importar arquivos CSS em um componente React.

Crie um arquivo chamado `layout.module.css` no diretório de `components` com o seguinte conteúdo:

```css
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
```

>**Importante**: Para usar esses Módulos CSS, o nome do arquivo CSS deve terminar com `.module.css`.

Para usar isso no layout, você precisa:

  - Importá-lo como `styles`

  - Usar `styles.<class-name>` como `className`

  - Nesse caso, o nome da classe é `container`, portanto, usaremos `styles.container`

```javascript
import styles from './layout.module.css'

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```

Se você acessar http://localhost:3000/posts/first-post agora, verá que o texto está agora dentro de um contêiner centralizado:

<h1 align="center"><img src="../../images/layout.png"></h1>

## Gera automaticamente nomes de classe exclusivos

Agora, se você der uma olhada no HTML nas ferramentas de desenvolvedor do seu navegador, perceberá que a tag `div` tem um nome de classe que se parece com `layout_container __...`.

<h1 align="center"><img src="../../images/devtools.png"></h1>

É isso que os Módulos CSS fazem: *Ele gera automaticamente nomes de classe exclusivos*. Desde que você use módulos CSS, não precisará se preocupar com colisões de nomes de classes.

Além disso, o recurso de divisão de código do Next.js. também funciona em módulos CSS. Ele garante que a quantidade mínima de CSS seja carregada para cada página. Isso resulta em tamanhos de pacote menores.

Os módulos CSS são extraídos dos pacotes configuráveis do JavaScript no momento da construção e geram arquivos `.css` carregados automaticamente pelo Next.js.

## Estilos globais

Módulos CSS são úteis para estilos em nível de componente. Mas se você deseja carregar algum CSS para ser carregado por **todas as páginas**, o Next.js também é compatível com isso.

Para carregar arquivos CSS globais, **crie um arquivo chamado** `_app.js` em `pages` e adicione o seguinte conteúdo:

```javascript
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```

Este componente do `app` é o componente de nível superior que será comum em todas as páginas diferentes. Você pode usar este componente `app` para manter o estado ao navegar entre páginas, por exemplo.

## Reinicie o servidor de desenvolvimento

**Importante**: Você precisa reiniciar o servidor de desenvolvimento ao adicionar `_app.js`. Pressione Ctrl + c para parar o servidor e executar:

```bash
npm run dev
```

## Adicionando CSS Global

No Next.js, você pode adicionar arquivos CSS globais importando-os do `_app.js`. Você **não pode** importar CSS global em nenhum outro lugar.

O motivo pelo qual o CSS global não pode ser importado fora do `_app.js` é que o CSS global afeta todos os elementos da página.

Se você navegasse da página inicial para a página `/posts/first-post`, os estilos globais da página inicial afetariam `/posts/first-post` sem querer.

Você pode colocar o arquivo CSS global em qualquer lugar e usar qualquer nome. Então, vamos fazer o seguinte:

  - Crie um diretório de `styles` de nível superior e crie um `global.css` dentro.

  - Adicione o seguinte conteúdo. Isso Redefine alguns estilos e altera a cor da marca `a`.

```css
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen, Ubuntu,
    Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  line-height: 1.6;
  font-size: 18px;
}

* {
  box-sizing: border-box;
}

a {
  color: #0070f3;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

img {
  max-width: 100%;
  display: block;
}
```

Por fim, importe-o do `_app.js`:

```javascript
import '../styles/global.css'

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```

Agora, se você acessar http://localhost:3000/posts/first-post, verá que os estilos são aplicados:

<h1 align="center"><img src="../../images/global-styles.png"></h1>

Se não funcionou: tenha certeza e reinicie o servidor de desenvolvimento ao adicionar `_app.js`.

Para resumir o que aprendemos até agora:

  - Para usar módulos CSS, importe um arquivo CSS chamado `*.module.css` de qualquer componente.

  - Para usar CSS global, importe um arquivo CSS em `pages/_app.js`.

## Layout de polimento

Até o momento, apenas adicionamos o mínimo de código React e CSS apenas para ilustrar conceitos como Módulos CSS. Antes de prosseguirmos para a próxima lição (busca de dados), vamos aprimorar o estilo e o código da página.

### Baixe sua foto do perfil

Primeiro, usaremos a foto do seu perfil para o design final.

  - Faça o download da sua foto de perfil no GitHub, Twitter, LinkedIn ou em outro lugar (ou use [este arquivo](https://github.com/zeit/next-learn-starter/blob/master/basics-final/public/images/profile.jpg)).

  - Crie um diretório `images` dentro do diretório `public`.

  - Salve a imagem como `profile.jpg` no diretório `public/images`.

  - O tamanho da imagem pode variar dentro de 400px por 400px.

  - Você pode remover o arquivo de logotipo SVG não utilizado diretamente no diretório public.

### Atualize `components/layout.module.css`

Segundo, usaremos o seguinte para `components/layout.module.css` - basta copiar e colar. Ele adiciona alguns estilos mais refinados.

```css
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}

.header {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.headerImage {
  width: 6rem;
  height: 6rem;
}

.headerHomeImage {
  width: 8rem;
  height: 8rem;
}

.backToHome {
  margin: 3rem 0 0;
}
```

### Crie `styles/utils.module.css`

Terceiro, vamos criar um conjunto de classes CSS úteis para tipografia e outras que serão úteis em vários componentes.

Vamos adicioná-lo como um módulo CSS em `styles/utils.module.css`.

```css
.heading2Xl {
  font-size: 2.5rem;
  line-height: 1.2;
  font-weight: 800;
  letter-spacing: -0.05rem;
  margin: 1rem 0;
}

.headingXl {
  font-size: 2rem;
  line-height: 1.3;
  font-weight: 800;
  letter-spacing: -0.05rem;
  margin: 1rem 0;
}

.headingLg {
  font-size: 1.5rem;
  line-height: 1.4;
  margin: 1rem 0;
}

.headingMd {
  font-size: 1.2rem;
  line-height: 1.5;
}

.borderCircle {
  border-radius: 9999px;
}

.colorInherit {
  color: inherit;
}

.padding1px {
  padding-top: 1px;
}

.list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.listItem {
  margin: 0 0 1.25rem;
}

.lightText {
  color: #999;
}
```

### Atualize `components/layout.js`

Quarto, copie o código a seguir para components/layout.js e **altere** `Your Name` na parte superior para seu nome. Aqui está o que há de novo:

`meta` tags (como `og: image`)
Booleano `home` que ajustará o tamanho do título e da imagem
Link "Back to home" na parte inferior se a `home` for `false`

```javascript
import Head from 'next/head'
import styles from './layout.module.css'
import utilStyles from '../styles/utils.module.css'
import Link from 'next/link'

const name = 'Your Name'
export const siteTitle = 'Next.js Sample Website'

export default function Layout({ children, home }) {
  return (
    <div className={styles.container}>
      <Head>
        <link rel="icon" href="/favicon.ico" />
        <meta
          name="description"
          content="Learn how to build a personal website using Next.js"
        />
        <meta
          property="og:image"
          content={`https://og-image.now.sh/${encodeURI(
            siteTitle
          )}.png?theme=light&md=0&fontSize=75px&images=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Ffront%2Fassets%2Fdesign%2Fnextjs-black-logo.svg`}
        />
        <meta name="og:title" content={siteTitle} />
        <meta name="twitter:card" content="summary_large_image" />
      </Head>
      <header className={styles.header}>
        {home ? (
          <>
            <img
              src="/images/profile.jpg"
              className={`${styles.headerHomeImage} ${utilStyles.borderCircle}`}
              alt={name}
            />
            <h1 className={utilStyles.heading2Xl}>{name}</h1>
          </>
        ) : (
          <>
            <Link href="/">
              <a>
                <img
                  src="/images/profile.jpg"
                  className={`${styles.headerImage} ${utilStyles.borderCircle}`}
                  alt={name}
                />
              </a>
            </Link>
            <h2 className={utilStyles.headingLg}>
              <Link href="/">
                <a className={utilStyles.colorInherit}>{name}</a>
              </Link>
            </h2>
          </>
        )}
      </header>
      <main>{children}</main>
      {!home && (
        <div className={styles.backToHome}>
          <Link href="/">
            <a>← Back to home</a>
          </Link>
        </div>
      )}
    </div>
  )
}
```

### Atualize `pages/index.js`

Finalmente, vamos atualizar a página inicial.

Altere o `pages/index.js` da seguinte maneira:

```javascript
import Head from 'next/head'
import Layout, { siteTitle } from '../components/layout'
import utilStyles from '../styles/utils.module.css'

export default function Home() {
  return (
    <Layout home>
      <Head>
        <title>{siteTitle}</title>
      </Head>
      <section className={utilStyles.headingMd}>
        <p>[Your Self Introduction]</p>
        <p>
          (This is a sample website - you’ll be building a site like this on{' '}
          <a href="https://nextjs.org/learn">our Next.js tutorial</a>.)
        </p>
      </section>
    </Layout>
  )
}
```

Em seguida, substitua `[Your Self Introduction]` pela sua introdução pessoal. Aqui está um exemplo com o perfil do autor:

<h1 align="center"><img src="../../images/example.png"></h1>

É isso aí! Agora temos o código de layout aprimorado para passar para as lições de busca de dados.

Antes de concluirmos esta lição, vamos falar sobre algumas técnicas úteis relacionadas ao suporte ao CSS do Next.js. na próxima página.

## Dicas de estilo

Aqui estão algumas dicas de estilo que podem ser úteis.

>Você pode apenas ler as seções a seguir. Não há necessidade de fazer alterações em nosso aplicativo!

### Usando a biblioteca `classnames` para alternar classes

`classnames` é uma biblioteca simples que permite alternar facilmente os nomes das classes. Você pode instalá-lo usando o `npm install classnames` ou `yarn add classnames`.

Dê uma olhada no [README](https://github.com/JedWatson/classnames) para obter detalhes, mas aqui está o uso básico:

  - Suponha que você deseje criar um componente `Alerta` que aceite `tipo`, que pode ser `'sucesso'` ou `'erro'`.

  - Se for `"sucesso"`, você deseja que a cor do texto seja verde. Se for um `"erro"`, você deseja que a cor do texto seja vermelha.

Você pode primeiro escrever um módulo CSS (por exemplo, `alert.module.css`) assim:

```css
.success {
  color: green;
}
.error {
  color: red;
}
```

E use `classnames` como este:

```javascript
import styles from './alert.module.css'
import cn from 'classnames'

export default function Alert({ type }) {
  return (
    <div
      className={cn({
        [styles.success]: type === 'success',
        [styles.error]: type === 'error'
      })}
    >
      {children}
    </div>
  )
}
```

### Personalizando a configuração PostCSS

Pronto para uso, sem configuração, o Next.js compila CSS usando [PostCSS](https://postcss.org/).

Para personalizar a configuração do PostCSS, você pode criar um arquivo de nível superior chamado `postcss.config.js`. Isso é útil se você estiver usando bibliotecas como o [Tailwind CSS](https://tailwindcss.com/).

Aqui está um exemplo `postcss.config.js` para usar o [Tailwind CSS](https://tailwindcss.com/) com [purgecss](https://github.com/FullHuman/purgecss), que remove o CSS não utilizado.

  - Você precisa instalar o `tailwindcss`, `@fullhuman/postcss-purgecss` e `postcss-preset-env`.

  - Você não precisa do `autoprefixer` porque o Next.js o inclui por padrão.

```javascript
module.exports = {
  plugins: [
    'tailwindcss',
    ...(process.env.NODE_ENV === 'production'
      ? [
          [
            '@fullhuman/postcss-purgecss',
            {
              content: [
                './pages/**/*.{js,jsx,ts,tsx}',
                './components/**/*.{js,jsx,ts,tsx}'
              ],
              defaultExtractor: content =>
                content.match(/[\w-/:]+(?<!:)/g) || []
            }
          ]
        ]
      : []),
    'postcss-preset-env'
  ]
}
```

Para saber mais sobre a configuração personalizada do PostCSS, consulte [nossa documentação](https://nextjs.org/docs/advanced-features/customizing-postcss-config).

### Usando Sass

Pronto, o Next.js permite importar o [Sass](https://sass-lang.com/) usando as extensões `.scss` e `.sass`. Você pode usar o Sass no nível do componente via Módulos CSS e a extensão `.module.scss` ou `.module.sass`.

Antes de poder usar o suporte integrado ao Sass do Next.js, certifique-se de instalar o sass:

```bash
npm install sass
```

### É isso nesta lição!

Para saber mais sobre os módulos CSS e suporte CSS integrados do Next.js., consulte [nossa documentação](https://nextjs.org/docs/basic-features/built-in-css-support).