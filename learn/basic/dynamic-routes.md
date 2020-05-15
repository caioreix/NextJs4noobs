[< Pagina Inicial](../../README.md#basico)
<h1 align="center">Rotas dinâmicas</h1>

Nós preenchemos a página de índice com os dados do blog, mas ainda não criamos páginas de blog individuais (eis o [resultado](https://next-learn-starter.now.sh/) desejado). Queremos que o URL dessas páginas dependa dos dados do blog, o que significa que precisamos usar rotas dinâmicas.

## O que você aprenderá nesta lição

Nesta lição, você aprenderá:

  - Como gerar estaticamente páginas com rotas dinâmicas usando `getStaticPaths`.

  - Como escrever `getStaticProps` para buscar os dados de cada postagem do blog.

  - Como renderizar markdown usando `remark`.

  - Como imprimir sequências de datas bonitas.

  - Como vincular a uma página com rotas dinâmicas.

  - Algumas informações úteis sobre rotas dinâmicas.

O caminho da página depende de dados externos

Na lição anterior, abordamos o caso em que o conteúdo da página depende de dados externos. Usamos `getStaticProps` para buscar os dados necessários para renderizar a página de índice.

Nesta lição, falaremos sobre o caso em que cada **caminho da página** depende de dados externos. O Next.js permite gerar estaticamente páginas com caminhos que dependem de dados externos. Isso habilita **URLs dinâmicos** no Next.js.

<h1 align="center"><img src="../../images/page-path-external-data.png"></h1>

### Como gerar estaticamente páginas com rotas dinâmicas

No nosso caso, queremos criar páginas dinâmicas para postagens do blog:

Queremos que cada postagem tenha o caminho `/posts/<id>`, onde `<id>` é o nome do arquivo de remarcação no diretório de `posts` de nível superior.
Como temos `ssg-ssr.md` e `pre-rendering.md`, gostaríamos que os caminhos fossem `/posts/ssg-ssr` e `/posts/pre-rendering`.

### Visão geral das etapas

Podemos fazer isso executando as seguintes etapas. **Você ainda não precisa fazer essas alterações**. Faremos tudo na próxima página.

Primeiro, criaremos uma página chamada `[id].js` em `pages/posts`. As páginas que começam com `[` e terminam com `]` são páginas dinâmicas no Next.js.

Em `pages/posts/[id].js`, escreveremos um código que renderizará uma página de postagem, assim como outras páginas que criamos.

```javascript
import Layout from '../../components/layout'

export default function Post() {
  return <Layout>...</Layout>
}
```

Agora, aqui está o que há de novo: exportaremos uma função assíncrona chamada `getStaticPaths` desta página. Nesta função, precisamos retornar uma lista de **possíveis valores** para `id`.

```javascript
import Layout from '../../components/layout'

export default function Post() {
  return <Layout>...</Layout>
}

export async function getStaticPaths() {
  // Return a list of possible value for id
}
```

Por fim, precisamos implementar `getStaticProps` novamente - desta vez, para buscar os dados necessários para a postagem do blog com um determinado `id`. `getStaticProps` recebe `parâmetros`, que contém o `id`.

```javascript
import Layout from '../../components/layout'

export default function Post() {
  return <Layout>...</Layout>
}

export async function getStaticPaths() {
  // Return a list of possible value for id
}

export async function getStaticProps({ params }) {
  // Fetch necessary data for the blog post using params.id
}
```

Aqui está um resumo gráfico do que acabamos de falar:

<h1 align="center"><img src="../../images/how-to-dynamic-routes.png"></h1>

Vamos tentar isso na próxima página!

## Implementando getStaticPaths

Primeiro, vamos configurar os arquivos:

Crie um arquivo chamado `[id].js` dentro do diretório `pages/posts`.

Além disso, remova `first-post.js` dentro do diretório de `pages/posts` - não usaremos mais isso.

Em seguida, adicione isso em `pages/ posts/[id].js`. Vamos preencher ... mais tarde.

```javascript
import Layout from '../../components/layout'

export default function Post() {
  return <Layout>...</Layout>
}
```

Em seguida, abra `lib/posts.js` e adicione esta função. Isso retornará a lista de nomes de arquivos (excluindo `.md`) no diretório `posts`:

```javascript
export function getAllPostIds() {
  const fileNames = fs.readdirSync(postsDirectory)

  // Returns an array that looks like this:
  // [
  //   {
  //     params: {
  //       id: 'ssg-ssr'
  //     }
  //   },
  //   {
  //     params: {
  //       id: 'pre-rendering'
  //     }
  //   }
  // ]
  return fileNames.map(fileName => {
    return {
      params: {
        id: fileName.replace(/\.md$/, '')
      }
    }
  })
}
```

**Importante**: A lista retornada não é apenas um array de seqüências de caracteres - deve ser um array de objetos que se parecem com o comentário acima. Cada objeto deve ter a chave `params` e conter um objeto com a chave `id` (porque estamos usando `[id]` no nome do arquivo). Caso contrário, `getStaticPaths` falhará.

Por fim, em `pages/posts/[id].js`, importaremos esta função:

```javascript
import { getAllPostIds } from '../../lib/posts'
```

E crie `getStaticPaths` que chama esta função:

```javascript
export async function getStaticPaths() {
  const paths = getAllPostIds()
  return {
    paths,
    fallback: false
  }
}
```

O array de valores possíveis para `id` deve ser o valor da chave de `paths` do objeto retornado. É exatamente isso que `getAllPostIds()` retorna.

Ignorar `fallback: false` por enquanto - explicaremos isso mais tarde.

Estamos quase terminando, mas ainda precisamos implementar `getStaticProps`. Vamos fazer isso na próxima página!

## Implementando `getStaticProps`

Precisamos buscar os dados necessários para renderizar a postagem com o `id` fornecido.

Para fazer isso, abra `lib/posts.js` novamente e adicione esta função. Isso retornará os dados da postagem com base no `id`:

```javascript
export function getPostData(id) {
  const fullPath = path.join(postsDirectory, `${id}.md`)
  const fileContents = fs.readFileSync(fullPath, 'utf8')

  // Use gray-matter to parse the post metadata section
  const matterResult = matter(fileContents)

  // Combine the data with the id
  return {
    id,
    ...matterResult.data
  }
}
```

Por fim, em `pages/posts/[id].js`, substitua esta linha:

```javascript
import { getAllPostIds } from '../../lib/post
```

…with this:

```javascript
import { getAllPostIds, getPostData } from '../../lib/posts'
```

And create `getStaticProps` which calls this function:

```javascript
export async function getStaticProps({ params }) {
  const postData = getPostData(params.id)
  return {
    props: {
      postData
    }
  }
}
```

Atualize o componente `Post` para usar `postData`:

```javascript
export default function Post({ postData }) {
  return (
    <Layout>
      {postData.title}
      <br />
      {postData.id}
      <br />
      {postData.date}
    </Layout>
  )
}
```

É isso aí! Tente visitar estas páginas:

  - http://localhost:3000/posts/ssg-ssr
  - http://localhost:3000/posts/ssg-ssr

Você poderá ver os dados do blog para cada página:

<h1 align="center"><img src="../../images/blog-data-post-page.png"></h1>

Ótimo! Geramos páginas dinâmicas com sucesso.

### Algo errado?

Se você encontrar um erro, verifique se seus arquivos têm o código correto:

  - `pages/posts/[id].js` deve ficar [assim](https://github.com/zeit/next-learn-starter/blob/master/dynamic-routes-step-1/pages/posts/%5Bid%5D.js).

  - `lib/posts.js` deve ficar [assim](https://github.com/zeit/next-learn-starter/blob/master/dynamic-routes-step-1/lib/posts.js).

  - (Se ainda não estiver funcionando) O código restante deve ficar [assim](https://github.com/zeit/next-learn-starter/tree/master/dynamic-routes-step-1).

Se você ainda está preso, não hesite em perguntar à comunidade nos [debates do GitHub](https://github.com/zeit/next.js/discussions). Seria útil se você pudesse enviar seu código ao GitHub e vinculá-lo para que outros possam dar uma olhada.

### Sumário

Novamente, aqui está o resumo gráfico do que fizemos:

<h1 align="center"><img src="../../images/how-to-dynamic-routes (1).png"></h1>

Mas ainda não exibimos o conteúdo do markdown do blog. Então, vamos fazer isso a seguir.

## Renderizando o 

Para renderizar o conteúdo da markdown, usaremos a biblioteca de `remark`. Primeiro, vamos instalá-lo:

```bash
npm install remark remark-html
```

Importe-os em `lib/posts.js`:

```javascript
import remark from 'remark'
import html from 'remark-html'
```

E atualize o `getPostData()` da seguinte maneira para usar o `remark`.

```javascript
export async function getPostData(id) {
  const fullPath = path.join(postsDirectory, `${id}.md`)
  const fileContents = fs.readFileSync(fullPath, 'utf8')

  // Use gray-matter to parse the post metadata section
  const matterResult = matter(fileContents)

  // Use remark to convert markdown into HTML string
  const processedContent = await remark()
    .use(html)
    .process(matterResult.content)
  const contentHtml = processedContent.toString()

  // Combine the data with the id and contentHtml
  return {
    id,
    contentHtml,
    ...matterResult.data
  }
}
```

**Importante**: adicionamos a palavra-chave `async` ao `getPostData` porque precisamos usar o `await` para o `remark`.

Isso significa que precisamos atualizar o `getStaticProps` em `pages/posts/[id].js` para usar o `await` ao chamar `getPostData`:

```javascript
export async function getStaticProps({ params }) {
  // Add the "await" keyword like this:
  const postData = await getPostData(params.id)
  // ...
}
```

Por fim, atualize o componente `Post` para renderizar `contentHtml` usando `DangerouslySetInnerHTML`:

```javascript
export default function Post({ postData }) {
  return (
    <Layout>
      {postData.title}
      <br />
      {postData.id}
      <br />
      {postData.date}
      <br />
      <div dangerouslySetInnerHTML={{ __html: postData.contentHtml }} />
    </Layout>
  )
}
```

Tente visitar estas páginas novamente:

  - http://localhost:3000/posts/ssg-ssr

  - http://localhost:3000/posts/pre-rendering

Agora você deve ver o conteúdo do blog:

<h1 align="center"><img src="../../images/markdown.png"></h1>

Estamos quase terminando! Vamos polir cada página a seguir.

## Polindo a página de postagem

### Adicionando `title` à página de postagem

Em `pages/posts/[id].js`, adicione a tag `title` usando os dados da postagem. Importe `next/head` e adicione a tag `title`:

```javascript
import Head from 'next/head'

export default function Post({ postData }) {
  return (
    <Layout>
      <Head>
        <title>{postData.title}</title>
      </Head>
      ...
    </Layout>
  )
}
```

### Formatando a data

Para formatar a data, usaremos a biblioteca `date-fns`. Primeiro, instale-o:

```bash
npm install date-fns
```

Em seguida, crie o componente `Date` em `components/date.js`:

```javascript
import { parseISO, format } from 'date-fns'

export default function Date({ dateString }) {
  const date = parseISO(dateString)
  return <time dateTime={dateString}>{format(date, 'LLLL d, yyyy')}</time>
}
```

E use-o em `pages/posts/[id].js`:

```javascript
// Add this line to imports
import Date from '../../components/date'

export default function Post({ postData }) {
  return (
    <Layout>
      ...
      {/* Replace {postData.date} with this */}
      <Date dateString={postData.date} />
      ...
    </Layout>
  )
}
```

Se você acessar http://localhost:3000/posts/pre-rendering, deverá ver a data escrita como "January 1, 2020".

### Adicionando CSS

Por fim, vamos adicionar um pouco de CSS. Em `pages/posts/[id].js`, coloque tudo sob a tag `article` e use os módulos CSS como abaixo:

```javascript
// Add this line
import utilStyles from '../../styles/utils.module.css'

export default function Post({ postData }) {
  return (
    <Layout>
      <Head>
        <title>{postData.title}</title>
      </Head>
      <article>
        <h1 className={utilStyles.headingXl}>{postData.title}</h1>
        <div className={utilStyles.lightText}>
          <Date dateString={postData.date} />
        </div>
        <div dangerouslySetInnerHTML={{ __html: postData.contentHtml }} />
      </article>
    </Layout>
  )
}
```

Se você acessar http://localhost:3000/posts/pre-rendering, a página deverá ficar um pouco melhor:

<h1 align="center"><img src="../../images/post-page-css.png"></h1>

Ótimo trabalho! Vamos polir a página de índice a seguir e terminaremos!

## Polishing the Index Page

Como etapa final, vamos atualizar nossa página de índice (`pages/index.js`).

Em particular, precisamos adicionar links a cada página de postagem. Usaremos o componente `Link`, mas desta vez precisamos fazer algo diferente.

Para vincular a uma página com rotas dinâmicas, você precisa usar o componente Link de maneira diferente. No nosso caso, para criar um link para `/posts/ssg-ssr`, você precisa escrevê-lo assim:

```javascript
<Link href="/posts/[id]" as="/posts/ssg-ssr">
  <a>...</a>
</Link>
```

Como você pode ver, você precisa usar `[id]` para `href` e o caminho real (`ssg-ssr`) para as `as` prop.

Vamos implementá-lo. Primeiro, importe o `Link` e a `Date` em `pages/index.js`:

```javascript
import Link from 'next/link'
import Date from '../components/date'
```

Em seguida, próximo à parte inferior do componente `Home`, substitua a tag `li` pelo seguinte:

```javascript
<li className={utilStyles.listItem} key={id}>
  <Link href="/posts/[id]" as={`/posts/${id}`}>
    <a>{title}</a>
  </Link>
  <br />
  <small className={utilStyles.lightText}>
    <Date dateString={date} />
  </small>
</li>
```

Agora ele deve ter os links para cada artigo:

<h1 align="center"><img src="../../images/links.png"></h1>

Se algo não estiver funcionando, verifique se o seu código está [assim](https://github.com/zeit/next-learn-starter/tree/master/api-routes-starter).

É isso aí! Antes de concluirmos esta lição, vamos falar sobre algumas dicas de uso de rotas dinâmicas na próxima página.

## Detalhes das rotas dinâmicas

Você pode obter informações detalhadas sobre rotas dinâmicas em nossa documentação:

  - [Busca de dados](https://nextjs.org/docs/basic-features/data-fetching)

  - [Rotas dinâmicas](https://nextjs.org/docs/routing/dynamic-routes)

Mas aqui estão algumas informações essenciais que você deve saber sobre rotas dinâmicas.

### Buscar API externa ou banco de dados de consulta

Como `getStaticProps`, `getStaticPaths` pode buscar dados de qualquer fonte de dados. Em nosso exemplo, `getAllPostIds` (que é usado por `getStaticPaths`) pode buscar de um terminal de API externo:

```javascript
export async function getAllPostIds() {
  // Instead of the file system,
  // fetch post data from an external API endpoint
  const res = await fetch('..')
  const posts = await res.json()
  return posts.map(post => {
    return {
      params: {
        id: post.id
      }
    }
  })
}
```

### Desenvolvimento vs Produção

  - No **desenvolvimento** (`npm run dev` ou `yarn dev`), `getStaticPaths` é executado em *todas as solicitações*.
  - Na **produção**, o `getStaticPaths` é executado no *tempo de construção*.

### Fallback

Lembre-se de que retornamos `fallback: false` de `getStaticPaths`. O que isto significa?

Se o `fallback` for `false`, quaisquer caminhos não retornados por `getStaticPaths` resultarão em uma **página 404**.

Se o `fallback` for `true`, o comportamento de `getStaticProps` será alterado:

  - Os caminhos retornados de `getStaticPaths` serão renderizados em HTML no momento da construção.

  - Os caminhos que não foram gerados no tempo de construção **não** resultarão em uma página 404. Em vez disso, o Next.js exibirá uma versão de "fallback" da página na primeira solicitação para esse caminho.

  - Em segundo plano, o Next.js irá gerar estaticamente o caminho solicitado. Solicitações subsequentes para o mesmo caminho servirão a página gerada, assim como outras páginas pré-renderizadas no momento da criação.

Isso está além do escopo de nossas lições, mas você pode aprender mais sobre `fallback: true` em [nossa documentação](https://nextjs.org/docs/basic-features/data-fetching#fallback-pages).

### Rotas abrangentes

As rotas dinâmicas podem ser estendidas para pegar todos os caminhos adicionando três pontos (`...`) dentro dos colchetes. Por exemplo:

  - `pages/posts/[...id].js` corresponde a `/posts/a`, mas também `/posts/a/b`, `/posts/a/b/c` etc.

Se você fizer isso, em `getStaticPaths`, deverá retornar um array como o valor da chave `id` da seguinte maneira:

```javascript
return [
  {
    params: {
      // Statically Generates /posts/a/b/c
      id: ['a', 'b', 'c']
    }
  }
  //...
]
```

E `params.id` será um array em `getStaticProps`:

```javascript
export async function getStaticProps({ params }) {
  // params.id will be like ['a', 'b', 'c']
}
```

Dê uma olhada na [documentação de Rotas dinâmicas](https://nextjs.org/docs/routing/dynamic-routes) para saber mais.

### Roteador

Se você deseja acessar o roteador Next.js., faça isso importando o gancho `useRouter` do `next/router`. Dê uma olhada na [documentação do roteador](https://nextjs.org/docs/routing/dynamic-routes) para saber mais.

### 404 Pages

Para criar uma página 404 personalizada, crie `pages/404.js`. Este arquivo é gerado estaticamente no momento da construção.

```javascript
// pages/404.js
export default function Custom404() {
  return <h1>404 - Page Not Found</h1>
}
```

Consulte a documentação das [Páginas de erro](https://nextjs.org/docs/advanced-features/custom-error-page#404-page) para saber mais.

### Mais exemplos

Criamos vários exemplos para ilustrar `getStaticProps` e `getStaticPaths` - consulte seu código-fonte para saber mais:

[Blog Starter usando arquivos de marcação](https://github.com/zeit/next.js/tree/canary/examples/blog-starter) ([Demo](https://next-blog-starter.now.sh/))
[Exemplo do DatoCMS](https://github.com/zeit/next.js/tree/canary/examples/cms-datocms) ([Demo](https://next-blog-datocms.now.sh/))
[Exemplo de TakeShape](https://github.com/zeit/next.js/tree/canary/examples/cms-takeshape) ([Demo](https://next-blog-takeshape.now.sh/))
[Exemplo de sanity](https://github.com/zeit/next.js/tree/canary/examples/cms-sanity) ([Demo](https://next-blog-sanity.now.sh/))

É isso aí!

Na próxima lição, falaremos sobre o recurso Rotas da API para Next.js.