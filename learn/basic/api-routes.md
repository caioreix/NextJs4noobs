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

