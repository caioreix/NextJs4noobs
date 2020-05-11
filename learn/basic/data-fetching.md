[< Pagina Inicial](../../README.md#basico)
<h1 align="center">Pré-renderização e busca de dados</h1>

Gostaríamos de criar um blog (eis o [resultado desejado](https://next-learn-starter.now.sh/)), mas até agora não adicionamos conteúdo ao blog. Nesta lição, aprenderemos como buscar dados de blogs externos em nosso aplicativo. Armazenaremos o conteúdo do blog no sistema de arquivos, mas funcionará se o conteúdo for armazenado em outro local (por exemplo, banco de dados ou Headless CMS).

## O que você aprenderá nesta lição

Nesta lição, você aprenderá sobre:

  - Recurso de pré-renderização do Next.js.

  - As duas formas de pré-renderização: geração estática e renderização no servidor.

  - Geração estática com e sem dados.

  - `getStaticProps` e como usá-lo para importar dados externos do blog para a página de index.

  - Algumas informações úteis sobre `getStaticProps`.

## Pré-renderização

Antes de falarmos sobre a busca de dados, vamos falar sobre um dos conceitos mais importantes no Next.js: **pré-renderização**.

Por padrão, o Next.js pré-renderiza todas as páginas. Isso significa que o Next.js gera HTML para cada página antecipadamente, em vez de fazer tudo pelo JavaScript do cliente. A pré-renderização pode resultar em melhor desempenho e SEO.

Cada HTML gerado é associado ao código JavaScript mínimo necessário para essa página. Quando uma página é carregada pelo navegador, seu código JavaScript é executado e torna a página totalmente interativa. (Esse processo é chamado de **hydration**.)

### Check That Pre-rendering Is Happening

Você pode verificar se a pré-renderização está ocorrendo, executando as seguintes etapas:

  - Desative o JavaScript no seu navegador (veja como no [Chrome](https://developers.google.com/web/tools/chrome-devtools/javascript/disable)) e…

  - Tente acessar [esta página](https://next-learn-starter.now.sh/) (o resultado final deste tutorial).

Você deve ver que seu aplicativo é renderizado sem JavaScript. Isso porque o Next.js pré-renderizou o aplicativo em HTML estático, permitindo que você veja a interface do aplicativo sem executar o JavaScript.

>**Nota**: Você também pode tentar as etapas acima no `localhost`, mas o CSS não será carregado se você desativar o JavaScript.

Se o seu aplicativo for um React.js simples (sem o Next.js.), não haverá pré-renderização, portanto, você não poderá vê-lo se desativar o JavaScript. Por exemplo:

  - Ative o JavaScript no seu navegador e [confira esta página](https://create-react-app.now-examples.now.sh/). Este é um aplicativo simples do React.js criado com o [Create React App](https://create-react-app.dev/).

  - Agora, desative o JavaScript e acesse a [mesma página](https://create-react-app.now-examples.now.sh/) novamente.

  - Você não verá mais o aplicativo. Em vez disso, ele dirá "You need to enable JavaScript to run this app." Isso ocorre porque o aplicativo não é pré-renderizado em HTML estático.

### Resumo: pré-renderização vs sem pré-renderização

Aqui está um rápido resumo gráfico:

<h1 align="center"><img src="../../images/pre-rendering.png"></h1>

<h1 align="center"><img src="../../images/no-pre-rendering.png"></h1>

Em seguida, vamos falar sobre **duas formas** de pré-renderização em Next.js.

## Duas formas de pré-renderização

O Next.js possui duas formas de pré-renderização: geração estática e renderização no servidor. A diferença está quando ele gera o HTML para uma página.

  - **Static Generation** é o método de pré-renderização que gera o HTML no **momento da construção**. O HTML pré-renderizado é *reutilizado* em cada solicitação.

  - **A renderização Server-side** é o método de pré-renderização que gera o HTML em **cada solicitação**.

<h1 align="center"><img src="../../images/static-generation.png"></h1>

<h1 align="center"><img src="../../images/server-side-rendering.png"></h1>

>No modo de desenvolvimento (quando você executa o `npm run dev` ou `yarn dev`), todas as páginas são renderizadas previamente em cada solicitação - mesmo para páginas que usam Static Generation.

### Suporte por página

É importante ressaltar que o Next.js permite **escolher** qual formulário de pré-renderização usar para cada página. Você pode criar um aplicativo Next.js "híbrido" usando a Geração estática para a maioria das páginas e usando a renderização no servidor para outras.

<h1 align="center"><img src="../../images/per-page-basis.png"></h1>

### Quando usar a geração estática vs Renderização Server-side

Recomendamos o uso de **geração estática** (com e sem dados) sempre que possível, porque sua página pode ser construída uma vez e veiculada pela CDN, o que torna muito mais rápido do que um servidor renderizar a página em todas as solicitações.

Você pode usar a geração estática para muitos tipos de páginas, incluindo:

  - Páginas de marketing
  - Postagens no blog
  - Listagens de produtos de comércio eletrônico
  - Ajuda e documentação

Você deve se perguntar: "Posso renderizar previamente esta página **antes** da solicitação do usuário?" Se a resposta for sim, você deve escolher Geração estática.

Por outro lado, a Geração estática **não** é uma boa ideia se você não puder pré-renderizar uma página antes da solicitação do usuário. Talvez sua página mostre dados atualizados com frequência e o conteúdo da página seja alterado a cada solicitação.

Nesse caso, você pode usar a **renderização Server-Side**. Será mais lento, mas a página pré-renderizada estará sempre atualizada. Ou você pode pular a pré-renderização e usar o JavaScript do lado do cliente para preencher os dados.

### Vamos nos concentrar na geração estática

Nesta lição, focaremos na geração estática. Na próxima página, falaremos sobre geração estática **com e sem dados**.

## Geração estática **com e sem dados**.

A geração estática pode ser feita com e sem **dados**.

Até o momento, todas as páginas que criamos não exigem a obtenção de dados externos. Essas páginas serão geradas estaticamente automaticamente quando o aplicativo for criado para produção.

<h1 align="center"><img src="../../images/static-generation-without-data.png"></h1>

No entanto, em algumas páginas, talvez você não consiga renderizar o HTML sem buscar primeiro alguns dados externos. Talvez você precise acessar o sistema de arquivos, buscar a API externa ou consultar seu banco de dados no momento da construção. O Next.js suporta este caso - Geração estática **com dados** - prontos para uso.

<h1 align="center"><img src="../../images/static-generation-with-data.png"></h1>

### Geração estática com dados usando `getStaticProps`

Como funciona? Bem, no Next.js, quando você exporta um componente da página, também pode exportar uma função assíncrona chamada getStaticProps. Se você fizer isso, então:

  - `getStaticProps` é executado em tempo de construção na produção e…

  - Dentro da função, você pode buscar dados externos e transmiti-los como os props da página.

```javascript
export default function Home(props) { ... }

export async function getStaticProps() {
  // Get external data from the file system, API, DB, etc.
  const data = ...

  // The value of the `props` key will be
  //  passed to the `Home` component
  return {
    props: ...
  }
}
```

Essencialmente, o `getStaticProps` permite que você diga ao Next.js: “Ei, esta página possui algumas dependências de dados - portanto, quando você renderizar esta página no momento da criação, resolva-as primeiro!”

>Nota: No modo de desenvolvimento, `getStaticProps` é executado em cada solicitação.

### Vamos usar getStaticProps

É mais fácil aprender fazendo isso; portanto, a partir da próxima página, usaremos `getStaticProps` para implementar nosso blog.

## Dados do blog

Agora, adicionaremos dados de blog ao nosso aplicativo usando o sistema de arquivos. Cada postagem do blog será um arquivo de markdown.

  - Crie um novo diretório de nível superior chamado `posts` (não é o mesmo que `pages/posts`).

  - Dentro, crie dois arquivos: `pre-rendering.md` e `ssg-ssr.md`.

Copie o seguinte para `pre-rendering.md`:

```markdown
---
title: 'Two Forms of Pre-rendering'
date: '2020-01-01'
---

Next.js has two forms of pre-rendering: **Static Generation** and **Server-side Rendering**. The difference is in **when** it generates the HTML for a page.

- **Static Generation** is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then _reused_ on each request.
- **Server-side Rendering** is the pre-rendering method that generates the HTML on **each request**.

Importantly, Next.js lets you **choose** which pre-rendering form to use for each page. You can create a "hybrid" Next.js app by using Static Generation for most pages and using Server-side Rendering for others.
```

Copie o seguinte para `ssg-ssr.md`:

```markdown
---
title: 'When to Use Static Generation v.s. Server-side Rendering'
date: '2020-01-02'
---

We recommend using **Static Generation** (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

You can use Static Generation for many types of pages, including:

- Marketing pages
- Blog posts
- E-commerce product listings
- Help and documentation

You should ask yourself: "Can I pre-render this page **ahead** of a user's request?" If the answer is yes, then you should choose Static Generation.

On the other hand, Static Generation is **not** a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.

In that case, you can use **Server-Side Rendering**. It will be slower, but the pre-rendered page will always be up-to-date. Or you can skip pre-rendering and use client-side JavaScript to populate data.
```

>Você deve ter notado que cada arquivo de remarcação possui uma seção de metadados na parte superior, contendo `title` e `date`. Isso se chama YAML Front Matter, que pode ser analisado usando uma biblioteca chamada [gray-matter](https://github.com/jonschlinkert/gray-matter).

### Analisando os dados do blog em `getStaticProps`

Agora, vamos atualizar nossa página de índice (pages/index.js) usando esses dados. Gostaríamos de:

  - Analisar cada arquivo de remarcação e obter o `title`, a `date` e o nome do arquivo (que serão usados como `id` para o URL da postagem).

  - Listar os dados na página de índice, classificados por data.

Para fazer isso na pré-renderização, precisamos implementar o `getStaticProps`.

<h1 align="center"><img src="../../images/index-page.png"></h1>

Vamos fazer na próxima página!

## Implementar `getStaticProps`

Primeiro, instale a gray-matter, que permite analisar os metadados em cada arquivo de markdown.

```bash
npm install gray-matter
```

Em seguida, criaremos uma biblioteca simples para buscar dados do sistema de arquivos.

  - Crie um diretório de nível superior chamado `lib` e…

  - Crie um arquivo chamado `posts.js` dentro com o seguinte conteúdo:

```javascript
import fs from 'fs'
import path from 'path'
import matter from 'gray-matter'

const postsDirectory = path.join(process.cwd(), 'posts')

export function getSortedPostsData() {
  // Get file names under /posts
  const fileNames = fs.readdirSync(postsDirectory)
  const allPostsData = fileNames.map(fileName => {
    // Remove ".md" from file name to get id
    const id = fileName.replace(/\.md$/, '')

    // Read markdown file as string
    const fullPath = path.join(postsDirectory, fileName)
    const fileContents = fs.readFileSync(fullPath, 'utf8')

    // Use gray-matter to parse the post metadata section
    const matterResult = matter(fileContents)

    // Combine the data with the id
    return {
      id,
      ...matterResult.data
    }
  })
  // Sort posts by date
  return allPostsData.sort((a, b) => {
    if (a.date < b.date) {
      return 1
    } else {
      return -1
    }
  })
}
```

E em `pages/index.js`, importe esta função:

```javascript
import { getSortedPostsData } from '../lib/posts'
```

Em seguida, chame esta função em `getStaticProps`. Você precisa retornar o resultado dentro da chave `props`:

```javascript
export async function getStaticProps() {
  const allPostsData = getSortedPostsData()
  return {
    props: {
      allPostsData
    }
  }
}
```

Depois que isso for configurado, o suporte `allPostsData` será passado para o componente `Home`. Para ver isso, modifique a definição do componente para obter `{allPostsData}`:

```javascript
export default function Home ({ allPostsData }) { ... }
```

Para exibir os dados, adicione outra tag `<section>` na parte inferior deste componente:

```javascript
export default function Home({ allPostsData }) {
  return (
    <Layout home>
      <Head>…</Head>
      <section className={utilStyles.headingMd}>…</section>
      <section className={`${utilStyles.headingMd} ${utilStyles.padding1px}`}>
        <h2 className={utilStyles.headingLg}>Blog</h2>
        <ul className={utilStyles.list}>
          {allPostsData.map(({ id, date, title }) => (
            <li className={utilStyles.listItem} key={id}>
              {title}
              <br />
              {id}
              <br />
              {date}
            </li>
          ))}
        </ul>
      </section>
    </Layout>
  )
}
```

Agora você deve ver os dados do blog se acessar http://localhost:3000.

<h1 align="center"><img src="../../images/blog-data.png"></h1>

Parabéns! Buscamos com êxito dados externos (do sistema de arquivos) e renderizamos previamente a página de índice com esses dados.

<h1 align="center"><img src="../../images/index-page (1).png"></h1>

Vamos falar sobre algumas dicas para usar `getStaticProps` na próxima página.

## Detalhes de `getStaticProps`

Você pode obter informações detalhadas sobre `getStaticProps` em [nossa documentação](https://nextjs.org/docs/basic-features/data-fetching). Mas aqui estão algumas informações essenciais que você deve saber sobre `getStaticProps`.

## Buscar API externa ou banco de dados de consulta

Em nossa lib/posts.js, implementamos `getSortedPostsData`, que busca dados do sistema de arquivos. Mas você pode buscar os dados de outras fontes, como um terminal de API externo, e funcionará perfeitamente:

```javascript
import fetch from 'node-fetch'

export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from an external API endpoint
  const res = await fetch('..')
  return res.json()
}
```

Você também pode consultar o banco de dados diretamente:

```javascript
import someDatabaseSDK from 'someDatabaseSDK'

const databaseClient = someDatabaseSDK.createClient(...)

export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from a database
  return databaseClient.query('SELECT posts...')
}
```

Isso é possível porque o `getStaticProps` é executado **apenas no server-side**. Ele nunca será executado no client-side. Ele nem será incluído no pacote JS do navegador. Isso significa que você pode escrever código como consultas diretas ao banco de dados sem que elas sejam enviadas para os navegadores.

### Desenvolvimento vs Produção

  - No desenvolvimento (`npm run dev` or `yarn dev`), `getStaticProps` é executado em todas as solicitações.

`- In production, getStaticProps runs at build time.

Como ele deve ser executado no tempo de compilação, você não poderá usar dados disponíveis apenas durante o tempo de solicitação, como parâmetros de consulta ou cabeçalhos HTTP.

### Permitido apenas em uma página

`getStaticProps` pode ser exportado apenas de uma **página**. Você não pode exportá-lo de arquivos que não são de página.

Um dos motivos dessa restrição é que o React precisa ter todos os dados necessários antes da renderização da página.

### E se eu precisar buscar dados no momento da solicitação?

A geração estática **não** é uma boa ideia se você não puder pré-renderizar uma página antes da solicitação do usuário. Talvez sua página mostre dados atualizados com frequência e o conteúdo da página seja alterado a cada solicitação.

Em casos como esse, você pode tentar a **renderização Server-side** ou ignorar a pré-renderização. Vamos falar sobre essas estratégias antes de prosseguirmos para a próxima lição.

## Buscando dados no momento da solicitação

Se você precisar buscar dados no momento da solicitação, e não no tempo de compilação, tente a **renderização Server-side**:

<h1 align="center"><img src="../../images/server-side-rendering-with-data.png"></h1>

Para usar a renderização do lado do servidor, você precisa exportar `getServerSideProps` em vez de `getStaticProps` da sua página.

### Using `getServerSideProps`

Aqui está o código inicial para `getServerSideProps`. Não é necessário para o exemplo do nosso blog, por isso não o implementaremos.

```javascript
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    }
  }
}
```

Como `getServerSideProps` é chamado no momento da solicitação, seu parâmetro (`context`) contém parâmetros específicos da solicitação. Você pode aprender mais em [nossa documentação](https://nextjs.org/docs/basic-features/data-fetching#getserversideprops-server-side-rendering).

Você deve usar `getServerSideProps` apenas se precisar renderizar previamente uma página cujos dados devem ser buscados no momento da solicitação. O tempo para o primeiro byte (TTFB) será mais lento que o `getStaticProps` porque o servidor deve calcular o resultado em cada solicitação e o resultado não pode ser armazenado em cache por uma CDN sem configuração extra.

### Renderização Client-side

Se você **não** precisar pré-renderizar os dados, também poderá usar a seguinte estratégia (denominada **Renderização no Client-side**):

  - Gera estaticamente (pré-renderiza) partes da página que não requerem dados externos.

  - Quando a página for carregada, busque dados externos do cliente usando JavaScript e preencha as partes restantes.

<h1 align="center"><img src="../../images/client-side-rendering.png"></h1>

Essa abordagem funciona bem para as páginas do painel do usuário, por exemplo. Como um painel é uma página privada específica do usuário, o SEO não é relevante e a página não precisa ser pré-renderizada. Os dados são atualizados com freqüência, o que requer a busca de dados no momento da solicitação.

### SWR

A equipe por trás do Next.js criou um gancho React para busca de dados chamado [SWR](https://swr.now.sh/). É altamente recomendável que você esteja buscando dados no lado do cliente. Ele lida com armazenamento em cache, revalidação, rastreamento de foco, busca em intervalos e muito mais. Não abordaremos os detalhes aqui, mas aqui está um exemplo de uso:

```javascript
import useSWR from 'swr'

function Profile() {
  const { data, error } = useSWR('/api/user', fetch)

  if (error) return <div>failed to load</div>
  if (!data) return <div>loading...</div>
  return <div>hello {data.name}!</div>
}
```

Check out the [SWR documentation](https://swr.now.sh/) to learn more.

### É isso aí!

Na próxima lição, criaremos páginas para cada postagem de blog usando **rotas dinâmicas**.

>Novamente, você pode obter informações detalhadas sobre `getStaticProps` e `getServerSideProps` em [nossa documentação](https://nextjs.org/docs/basic-features/data-fetching).