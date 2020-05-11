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

