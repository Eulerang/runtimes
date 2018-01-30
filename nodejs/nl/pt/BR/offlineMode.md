---

copyright:
  years: 2016, 2017
lastupdated: "2017-10-26"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Trabalhar no modo off-line para o node.js
{: #offline_mode}

Quando um aplicativo node.js é enviado por push para o {{site.data.keyword.Bluemix}}, o buildpack do SDK for Node.js
normalmente faz o download de artefatos a partir de recursos externos, como módulos de nó do NPM.  Em algumas situações, como com
[{[site.data.keyword.Bluemix_dedicated_notm](/docs/dedicated/index.html#dedicated) e
[Bluemix_local_notm](/docs/local/index.html#local), você pode desejar não contar com ou ter mais controle
explícito no acesso a sites externos ao {{site.data.keyword.Bluemix_notm}}.  
{: shortdesc}

A seguir estão os sites externos que o buildpack do node.js pode acessar.  Em ambientes
[{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) e
[{{site.data.keyword.Bluemix_local_notm}}](/docs/local/index.html#local)
{{site.data.keyword.Bluemix_notm}}, esses sites podem precisar ser *incluídos na lista de desbloqueio*.

* http://nodejs.org/ pode ser usado para determinar as versões do mecanismo de nó disponíveis.
* https://s3pository.heroku.com é usado para recuperar as versões do mecanismo de nó não incluídas no buildpack.
*  https://www.npmjs.com/package/npm e https://semver.herokuapp.com são usados para recuperar as versões do npm não incluídas no buildpack.
* https://iojs.org é usado para recuperar a versão mais antiga do nó que não está contida no buildpack ou não está disponível em https://semver.herokuapp.com.
* https://registry.npmjs.org é usado para recuperar módulos de nó, como expresso.

Para minimizar o conjunto de sites incluídos na lista de desbloqueio, configure seus aplicativos de nó para usar uma versão do mecanismo de nó que esteja incluída no buildpack do SDK for Node.js.  Consulte [atualizações mais recentes](./updates.html) para obter o conjunto de versões do mecanismo de nó incluídas no buildpack.  Se isso estiver pronto, somente o site https://registry.npmjs.org será necessário para fazer o download dos módulos de nó.

Esteja ciente de que quando novas versões do buildpack do SDK for Node.js são instaladas, o conjunto de versões disponíveis do mecanismo de nó muitas vezes
é movido para versões mais novas.  Isso pode requerer que você reconfigure seu app de nó para especificar uma versão mais nova do mecanismo de nó.


## Aplicativos off-line
{: #offline_applications}

Para eliminar a necessidade de acessar https://registry.npmjs.org, é possível incluir em seu aplicativo todos os módulos de nó requeridos pelo aplicativo.  Para fazer isso, execute **npm install** para todos os módulos que o seu aplicativo requer e inclua o diretório *node_modules* resultante em seu aplicativo enviado por push.

Observe que, suas dependências podem ter dependências e estas podem ter dependências e assim por diante, mas o package.json
contém somente as dependências de nível superior. Se uma das dependências usar uma faixa no package.json e uma nova versão desta for liberada, os módulos no diretório node_modules poderão se tornar obsoletos. *Termo-retrátil* ajuda você a bloquear todas as versões de dependência para que isso não possa ocorrer.  Para usar o termo-retrátil, comece com um diretório node_modules vazio ou limpo; em seguida, no diretório-raiz do seu projeto, execute:
0. npm install
1. npm dedupe
2. npm shrinkwrap

Isso pode mudar o *package.json* e incluir o *npm-shrinkwrap.json* no diretório-raiz.
Sempre que você fizer uma mudança em dependências no arquivo *package.json*, repita as etapas *dedupe* e *shringwrap*.

## Trabalhando com um proxy
{: #working_with_proxy}

Em alguns ambientes, como [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) e [{{site.data.keyword.Bluemix_local_notm}}](/docs/local/index.html#local), um proxy pode ser configurado. Consulte
[Trabalhando com um proxy](/docs/manageapps/workingWithProxy.html) para obter mais detalhes.
