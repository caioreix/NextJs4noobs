[< Pagina Inicial](../../README.md#basico)

<h1 align="center">Rotas de API</h1>

O Next.js tem suporte para Rotas da API, o que permite criar facilmente um endpoint da API como uma função Node.js. Embora não seja necessário para o nosso aplicativo de blog, falaremos brevemente sobre como usá-lo nesta lição.

### O que você aprenderá nesta lição

Nesta lição, você aprenderá:

  - Como criar uma rota de API.

  - Algumas informações úteis sobre rotas de API.

## Criando rotas de API

As rotas da API permitem criar um endpoint da API dentro de um aplicativo Next.js. Você pode fazer isso criando uma `função` dentro do diretório `pages/api` que possui o seguinte formato:

```javascript
// req = request data, res = response data
export default (req, res) => {
  // ...
}
```

Eles podem ser implantados como Funções Serverless (também conhecidas como Lambdas).

### Criando um terminal de API simples

Vamos tentar. Crie um arquivo chamado `hello.js` em `pages/api` com o seguinte código:

```javascript
export default (req, res) => {
  res.status(200).json({ text: 'Hello' })
}
```

Tente acessá-lo em `http://localhost:3000/api/hello`. Você deve ver `{"text": "Hello"}`. Observe que:

  - `req` é uma instância do [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage), além de alguns middlewares pré-criados que você pode ver [aqui](https://nextjs.org/docs/api-routes/api-middlewares).
  - `res` é uma instância do [http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse), além de algumas funções auxiliares que você pode ver [aqui](https://nextjs.org/docs/api-routes/response-helpers).

É isso aí! Antes de encerrarmos esta lição, vamos falar sobre algumas dicas para usar rotas de API na próxima página.

## Detalhes das rotas da API

Você pode obter informações detalhadas sobre rotas de API em nossa [documentação](https://nextjs.org/docs/api-routes/dynamic-api-routes). Mas aqui estão algumas informações essenciais que você deve saber sobre rotas de API.

### Não obtenha uma rota de API de `getStaticProps` ou `getStaticPaths`

Você não deve buscar uma rota de API em getStaticProps ou getStaticPaths. Em vez disso, escreva seu código no server-side diretamente em `getStaticProps` ou `getStaticPaths` (ou chamar uma função auxiliar).

Eis o motivo: `getStaticProps` e `getStaticPaths` são executados apenas no server-side. Ele nunca será executado no client-side. Ele nem será incluído no pacote JS do navegador. Isso significa que você pode escrever código como consultas diretas ao banco de dados sem que elas sejam enviadas para os navegadores.

### Um bom caso de uso: manipulação de entrada de formulário

Um bom caso de uso para rotas de API é manipular a entrada de formulário. Por exemplo, você pode criar um formulário em sua página e enviar uma solicitação `POST` para sua rota de API. Você pode escrever o código para salvá-lo diretamente no seu banco de dados. O código de rota da API não fará parte do seu pacote de clientes, portanto, você pode escrever com segurança o código do servidor.

```javascript
export default (req, res) => {
  const email = req.body.email
  // Then save email to your database, etc...
}
```

### Modo de pré-visualização

A geração estática é útil quando suas páginas buscam dados de um headless CMS. No entanto, não é o ideal quando você está escrevendo um rascunho no seu CMS decapitado e deseja **visualizá-lo** imediatamente na sua página. Você deseja que o Next.js renderize essas páginas no **momento da solicitação**, em vez do tempo de compilação, e busque o conteúdo de rascunho em vez do conteúdo publicado. Você quer que o Next.js ignore a Geração estática apenas neste caso específico.

O Next.js possui o recurso chamado **Modo de visualização**, que resolve esse problema e utiliza rotas de API. Para saber mais, consulte a documentação do [Modo de visualização](https://nextjs.org/docs/advanced-features/preview-mode).

### Rotas de API dinâmicas

As rotas da API podem ser dinâmicas, assim como as páginas comuns. Dê uma olhada na documentação de [rotas de API dinâmicas](https://nextjs.org/docs/api-routes/dynamic-api-routes) para saber mais.

### É isso aí!

Na próxima e na lição básica final, falaremos sobre como implantar o aplicativo Next.js. em produção.