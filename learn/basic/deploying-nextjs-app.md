[< Pagina Inicial](../../README.md#basico)
<h1 align="center">Implantando o aplicativo Next.js</h1>

Nesta lição básica final, implantaremos nosso aplicativo Next.js. em produção.

Aprenderemos como implantar o Next.js no [Vercel](https://vercel.com/), a plataforma de implantação do Jamstack criada pelos criadores do Next.js. Também falaremos sobre outras opções de implantação.

**Pre-requisite**: You need to have a [GitHub account](https://github.com/) for this lesson.

### O que você aprenderá nesta lição

Nesta lição, você aprenderá:

  - Como implantar seu aplicativo Next.js no [Vercel](https://vercel.com/).

  - O fluxo de trabalho do **DPS**: **D**evelop, **P**review e **S**hip.

  - Como implantar o aplicativo Next.js em seu próprio provedor de hospedagem.

## Push para o GitHub

Antes de fazer o deploy, vamos enviar nosso aplicativo Next.js para o [GitHub](https://github.com/zeit/next.js), se você ainda não o fez. Isso facilitará a implantação.

  - Na sua conta pessoal do GitHub, crie um novo repositório chamado `nextjs-blog`.

  - O repositório pode ser público ou privado. Você **não** precisa inicializá-lo com um README ou outros arquivos.

  - Se precisar de ajuda, consulte este [guia no GitHub](https://help.github.com/en/github/getting-started-with-github/create-a-repo).

Então:

  - Se você não inicializou o repositório git localmente para o seu aplicativo Next.js., faça-o agora.

  - De um push do aplicativo Next.js para este repositório do GitHub.

Para enviar para o GitHub, você pode executar os seguintes comandos (substitua `<username>` pelo nome de usuário do GitHub):

```bash
git remote add origin https://github.com/<username>/nextjs-blog.git
git push -u origin master
```

Quando seu repositório GitHub estiver pronto, continue na próxima página.

## Implantar no Vercel

A maneira mais fácil de implantar o Next.js. na produção é usar a plataforma [Vercel](https://vercel.com/) desenvolvida pelos criadores do Next.js.

O Vercel é uma plataforma multifuncional com CDN global que suporta a implantação estática e JAMstack e Serverless. Acreditamos que a Vercel é o local ideal para implantar aplicativos Next.js. Você pode começar a usá-lo gratuitamente - não é necessário cartão de crédito.

### Crie uma conta Vercel

Primeiro, acesse https://vercel.com/signup para criar uma conta Vercel. Escolha Continuar com o GitHub e siga o processo de inscrição.

### Importe seu repositório nextjs-blog

Depois de se inscrever, **importe** seu repositório `nextjs-blog` no Vercel. Você pode fazer isso aqui: https://vercel.com/import/git.

  - Você precisará **instalar o Now for GitHub**. Você pode dar acesso a **todos os repositórios**.
  - Depois de instalar o Now, importe o `nextjs-blog`.

Você pode usar valores padrão para as seguintes configurações - não é necessário alterar nada. O Vercel detecta automaticamente que você possui um aplicativo Next.js. e escolhe as configurações ideais de compilação para você.

  - Nome do Projeto
  - Diretório raiz
  - Comando Build
  - Diretório de saída
  - Comando de Desenvolvimento

Quando você implanta, seu aplicativo Next.js. começa a ser construído. Deve terminar em menos de um minuto.

>A ajuda está disponível: se a sua implantação falhar, você sempre poderá obter ajuda nas [discussões do GitHub](https://github.com/zeit/next.js/discussions). Para saber mais sobre a implantação, consulte nossa [documentação](https://nextjs.org/docs/deployment).

Quando terminar, você receberá **URLs de implantação**. Clique em um dos URLs e você verá a página inicial do Next.js. ao vivo.

Parabéns! Você acabou de implantar o aplicativo Next.js. em produção. Na próxima página, veremos os detalhes do Vercel e o fluxo de trabalho recomendado.

## Next.js e Vercel

O [Vercel](https://vercel.com/) é fabricado pelos criadores do Next.js e tem suporte de primeira classe para o Next.js. Quando você implanta o aplicativo Next.js no Vercel, o seguinte acontece por padrão:

  - As páginas que usam Geração estática e ativos (JS, CSS, imagens, fontes etc.) serão automaticamente veiculadas na [Vercel Edge Network](https://vercel.com/edge-network), que é extremamente rápida.

  - As páginas que usam renderização no Server-Side e rotas de API se tornarão automaticamente [funções Serverless](https://vercel.com/docs/v2/serverless-functions/introduction). Isso permite que a renderização da página e as solicitações da API sejam escaladas infinitamente.

O Vercel possui muitos outros recursos, como:

  - **Domínios personalizados**: depois de implantado no Vercel, você pode atribuir um domínio personalizado ao seu aplicativo Next.js. Dê uma olhada na nossa documentação [aqui](https://vercel.com/docs/v2/custom-domains).

  - **Variáveis de ambiente**: você também pode definir variáveis de ambiente no Vercel. Dê uma olhada na nossa documentação [aqui](https://vercel.com/docs/v2/build-step#environment-variables). Você pode usar [essas variáveis de ambiente](https://nextjs.org/docs/api-reference/next.config.js/environment-variables) no seu aplicativo Next.js.

  - **HTTPS automático**: o HTTPS é ativado por padrão (incluindo domínios personalizados) e não requer configuração extra. Renovamos automaticamente os certificados SSL.

Leia [nossa documentação](https://vercel.com/docs) para saber mais sobre a plataforma Vercel.

### Visualizar implantação para cada push

>Os passos abaixo são opcionais - você pode experimentar ou apenas ler.

Após a implantação no Vercel, tente o seguinte, se puder:

  - Crie uma nova ramificação no seu aplicativo.

  - Faça algumas alterações e envie para o GitHub.

  - Crie uma nova solicitação de recebimento (página de ajuda do GitHub).

Você deve ver um comentário do bot `now` na página de solicitação de recebimento.

<h1 align="center"><img src="../../images/now-bot.png"></h1>

Tente clicar no URL de **visualização** dentro deste comentário. Você deve ver as alterações que acabou de fazer.

Quando você tem uma solicitação de recebimento aberta, o Vercel cria automaticamente uma **implantação de visualização** para essa ramificação a cada envio. O URL de visualização sempre apontará para a implantação de visualização mais recente.

Você pode compartilhar esse URL de visualização com seus colaboradores e obter feedback imediato.

Se sua implantação de visualização parecer boa, **mescle-a para** `master`. Quando você faz isso, o Vercel cria automaticamente uma implantação de produção.

### Desenvolver, Visualizar, Enviar

Acabamos de analisar o fluxo de trabalho que chamamos de **DPS**: **D**evelop, **P**review, and **S**hip.

  - **Desenvolver**: escrevemos código no Next.js e usamos o servidor de desenvolvimento Next.js. em execução para aproveitar o recurso de recarga a quente.

  - **Preview**: introduzimos alterações em uma branch no GitHub, e a Vercel criou uma implantação de visualização disponível por meio de um URL. Podemos compartilhar esse URL de visualização com outras pessoas para obter feedback. Além de fazer revisões de código, você pode fazer visualizações de implantação.

  - **Ship**: mesclamos a solicitação `master` de recebimento a ser enviada para produção.

É altamente recomendável usar esse fluxo de trabalho ao desenvolver aplicativos Next.js. Isso ajudará você a iterar seu aplicativo mais rapidamente.

## Outras opções de hospedagem

O Next.js pode ser implantado em qualquer provedor de hospedagem que suporte o Node.js.

Se você seguiu as instruções até agora, seu package.json deve ter scripts de criação e início:

```javascript
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  }
}
```

No seu próprio provedor de hospedagem, execute o script de `build` uma vez, que cria o aplicativo de produção na pasta `.next`.

```bash
npm run build
```

Após a criação, o script `start` inicia um servidor Node.js que suporta páginas híbridas, servindo páginas geradas estaticamente e renderizadas no lado do servidor. O servidor também suporta rotas de API também.

```bash
npm run start
```

>Dica: Você pode customizar o script `start` no `package.json` para aceitar um parâmetro `PORT`, atualizando-o como: `"start": "next start -p $ PORT"`.

É isso aí! Se você tiver dúvidas sobre a implantação do Next.js., pode perguntar à nossa comunidade nas [Discussões do GitHub](https://github.com/zeit/next.js/discussions).

## Finalmente

Parabéns por terminar todas as lições básicas! Aqui estão algumas etapas recomendadas:

### Compartilhe seu aplicativo Next.js.

Recomendamos que você compartilhe o aplicativo que você criou neste tutorial no Twitter. Em caso afirmativo, mencione nossa equipe no [@vercel](https://twitter.com/vercel) para que possamos dar uma olhada! Gostaríamos de receber seus comentários sobre este tutorial também.

### Use TypeScript com Next.js

Se você preferir usar o [TypeScript](https://www.typescriptlang.org/), pode aprender como usar o TypeScript com o Next.js [aqui](https://nextjs.org/learn/excel/typescript).

### O que aprender em seguida

Dê uma olhada na nossa documentação para saber mais. Em particular, as seguintes páginas podem ser interessantes:

  - [Busca de dados](https://nextjs.org/docs/basic-features/data-fetching): Aprenda em profundidade sobre a busca de dados.

  - [Configuração do Next.js](https://nextjs.org/docs/api-reference/next.config.js/introduction): Em particular, a documentação de [Variáveis de ambiente](https://nextjs.org/docs/api-reference/next.config.js/environment-variables) pode ser útil.

  - [Suporte AMP](https://nextjs.org/docs/advanced-features/amp-support/introduction): O Next.js permite criar facilmente uma página AMP.

  ### Participe da conversa

Se você tiver alguma dúvida sobre algo relacionado ao Next.js., sempre solicite a nossa comunidade nas [Discussões do GitHub](https://github.com/zeit/next.js/discussions).