---

copyright:
  years: 2017
lastupdated: "2017-12-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}

# Tutorial Introdução

* {: download} Parabéns, você implementou um aplicativo de amostra Hello World no {{site.data.keyword.Bluemix}}!  Para iniciar, siga este guia passo a passo. Ou <a class="xref" href="http://bluemix.net" target="_blank" title="(Fazer download de código de amostra)"><img class="hidden" src="../../images/btn_starter-code.svg" alt="Fazer download de código do aplicativo" />faça download do código de amostra</a> e explore você mesmo.

Seguindo este tutorial, você irá configurar um ambiente de desenvolvimento, implementará um aplicativo localmente e no
{{site.data.keyword.Bluemix}} e integrará um serviço de banco de dados do {{site.data.keyword.Bluemix_notm}} em seu
aplicativo.

## Antes de Começar
{: #prereqs}
* [Git ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://git-scm.com/downloads){: new_window}
* [Cloud Foundry CLI ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}
* [Compilador Swift ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://swift.org/download/) de sua plataforma.

## Etapa 1: clonar o aplicativo de amostra
{: #clone}

Agora você está pronto para começar a trabalhar com o app Swift. Clone o repositório e mude para o diretório no qual o app de amostra está localizado.
  ```
git clone https://github.com/IBM-Bluemix/get-started-swift
  ```
  {: pre}

  ```
cd get-started-swift
  ```
  {: pre}

  Examine os arquivos no diretório *get-started-swift* para familiarizar-se com o conteúdo.

## Etapa 2: executar o app localmente
{: #run_locally}

Depois de ter instalado o compilador Swift e clonado esse repositório Git, será possível agora compilar e executar o aplicativo. Acesse o diretório-raiz desse repositório em seu sistema e emita o comando a seguir:
```
swift build
```
{: pre}

Esse comando pode levar alguns minutos para ser executado.

Quando o aplicativo for compilado com sucesso, será possível executar o executável que foi gerado pelo compilador Swift:
```
.build/debug/kitura-helloworld
```
{: pre}

Você deve ver uma saída semelhante à seguinte:

```
Server is listening on port: 8080
```
{: codeblock}

Visualize seu app em: http://localhost:8080

## Etapa 3: preparar o app para implementação
{: #prepare}

Para implementar no {{site.data.keyword.Bluemix_notm}}, poderá ser útil configurar um arquivo manifest.yml. O manifest.yml inclui informações básicas sobre seu app, como o nome, quanta memória alocar para cada instância e a rota. Nós fornecemos um arquivo manifest.yml de amostra no diretório `get-started-swift`.

Abra o arquivo manifest.yml e mude o `nome` de `GetStartedSwift` para o nome de seu app, <var class="keyword varname" data-hd-keyref="app_name">app_name</var>.
{: download}

  ```
  applications:
   - name: GetStartedSwift
     random-route: true
     memory: 256M
     command: kitura-helloworld
     buildpack: swift_buildpack
  ```
  {: codeblock}

Nesse arquivo manifest.yml, **random-route: true** gera uma rota aleatória para seu app para evitar que sua rota colida com outras.  Se você optar por isso, será possível substituir **random-route: true** por **host: myChosenHostName**, fornecendo um nome de host de sua preferência. [Saiba mais...](/docs/manageapps/depapps.html#appmanifest)
{: tip}

## Etapa 4: implementar o app
{: #deploy}

É possível usar a CLI do Cloud Foundry para implementar apps.

Escolha seu terminal de API
  ```
cf api <API-endpoint>
  ```
  {: pre}

Substitua o *API-endpoint* no comando por um terminal de API da lista a seguir.

| **Nome da região** | **Local geográfico** | **Endpoint da API** |
|-----------------|-------------------------|-------------------|
| Região Sul dos EUA | Dallas, EUA | api.ng.bluemix.net |
| Região Leste dos EUA | Washington, DC, EUA | api.us-east.bluemix.net |
| Região do Reino Unido | Londres, Inglaterra | api.eu-gb.bluemix.net |
| Região de Sydney | Sydney, Austrália | api.au-syd.bluemix.net |
| Região da Alemanha | Frankfurt, Alemanha | api.eu-de.bluemix.net |
{: caption="Tabela 1.  {{site.data.keyword.cloud_notm}} lista de região" caption-side="top"}

Efetue login em sua conta do {{site.data.keyword.Bluemix_notm}}

   ```
 cf login
   ```
   {: pre}

Se não for possível efetuar login usando os comandos `cf login` ou `bx login` porque você tem um ID de usuário federado, use os comandos `cf login --sso` ou `bx login --sso` para efetuar login com seu ID de conexão única. Veja [Efetuando login com um ID federado](https://console.bluemix.net/docs/cli/login_federated_id.html#federated_id) para saber mais.

No diretório *get-started-swift*, envie seu app por push para o {{site.data.keyword.Bluemix_notm}}
   ```
 cf push
   ```
   {: pre}

Isso pode levar um minuto. Se houver um erro no processo de implementação, será possível usar o comando `cf logs <Your-App-Name> --recent` para solucionar problemas.

Quando a implementação for concluída, você deverá ver uma mensagem indicando que o app está em execução.  Visualize o app na URL listada na saída do comando push.  Também é possível emitir o comando `cf apps` para visualizar o status dos apps e ver a URL.

## Etapa 5: incluir um banco de dados
{: #add_database}

Em seguida, vamos incluir um banco de dados NoSQL nesse aplicativo e configurar o aplicativo para que ele possa ser executado localmente e no {{site.data.keyword.Bluemix_notm}}.

1. Efetue login no {{site.data.keyword.Bluemix_notm}} em seu navegador. Procure o `Painel`. Selecione seu aplicativo clicando em seu nome na coluna `Nome`.
2. Clique em `Conexões` e, em seguida, `Criar conexão`.
3. Na seção `Data & Analytics`, selecione `Cloudant NoSQL DB`
4. Selecione um plano de precificação. O {{site.data.keyword.Bluemix_notm}} oferece planos `Lite`
gratuitos para uma coleção selecionada de seus serviços de nuvem com capacidade suficiente para você começar
5. Selecione `Remontar` quando solicitado. O {{site.data.keyword.Bluemix_notm}} reiniciará o aplicativo e fornecerá as credenciais do banco de dados para ele usando a variável de ambiente `VCAP_SERVICES`. Essa variável de ambiente ficará disponível para o aplicativo somente quando ele estiver em execução no {{site.data.keyword.Bluemix_notm}}.

As variáveis de ambiente permitem separar as configurações de implementação do seu código-fonte. Por exemplo, em vez de codificar permanentemente uma senha do banco de dados, é possível armazená-la em uma variável de ambiente que seja referenciada em seu código-fonte. [Saiba mais...](/docs/manageapps/depapps.html#app_env)
{: tip}

## Etapa 6: usar o banco de dados
{: #use_database}

Vamos agora atualizar seu código local para apontar para esse banco de dados. Crie um arquivo json que armazenará as credenciais dos serviços que o aplicativo usará. Esse arquivo será usado SOMENTE quando o aplicativo estiver sendo executado localmente. Ao executar no {{site.data.keyword.Bluemix_notm}}, as credenciais serão lidas por meio da variável de ambiente VCAP_SERVICES.

Crie um arquivo chamado `my-cloudant-credentials.json` no diretório `config` com o
conteúdo a seguir (como referência, consulte `config/my-cloudant-credentials.json.example`):

 ```
 {
   "password": "<password>",
   "url": "<url>",
   "username": "<username>"
 }
 ```

Atualize o arquivo `mappings.json` no diretório `config` substituindo o
item temporário `cloudant` pelo **nome** de sua instância do banco de dados:

```
{
  "MyCloudantDB": {
    "searchPatterns": [
      "cloudfoundry:cloudant",
      "env:kube-cloudant-credentials",
      "file:config/my-cloudant-credentials.json"
    ]
  }
}
```

Esse aplicativo de amostra usa o pacote `CloudEnvironment` para interagir com o {{site.data.keyword.Bluemix_notm}}
para analisar as variáveis de ambiente. [Saiba mais...](https://packagecatalog.com/package/IBM-Swift/CloudEnvironment)

O item temporário `cloudant` na configuração `cloudfoundry:cloudant` torna mais
fácil ligar um serviço Cloudant fornecido pelo usuário ao seu aplicativo. Com a configuração
`cloudfoundry:cloudant`, é possível criar um serviço Cloudant que inclui a sequência,
`cloudant` em algum lugar no nome do serviço e liga-o ao seu aplicativo, sem editar o arquivo `config.json`.
Se você modificar essa configuração e depois quiser usar um serviço Cloudant fornecido pelo usuário, será necessário editar a
configuração para `cloudfoundry:cloudant` ou definir `cloudfoundry:` com o nome do seu
serviço fornecido pelo usuário.
{: tip}

De volta na UI do {{site.data.keyword.Bluemix_notm}}, selecione seu App -> Conexões -> Cloudant -> Visualizar credenciais

Copie e cole apenas as credenciais para os campos correspondentes em seu arquivo config.json local.

Construa e execute o aplicativo localmente.
 ```
swift build  
 ```
 {: pre}

  ```
.build/debug/kitura-helloworld
 ```
 {: pre}

Visualize seu app em: http://localhost:8080. Os nomes que você inserir no app serão agora incluídos no banco de dados.

 Esse aplicativo de amostra usa o pacote Kitura-CouchDB para interagir com o Cloudant. [Saiba mais...](https://packagecatalog.com/package/IBM-Swift/Kitura-CouchDB)

 Faça quaisquer mudanças desejadas e reimplemente para {{site.data.keyword.Bluemix_notm}}!
 ```
 cf app push
 ```

 Visualize o aplicativo na URL listada na saída do comando push, por exemplo, *myUrl.mybluemix.net*.
Lembre-se, se você não precisar do app em tempo real, pare-o para não incorrer em encargos inesperados.
{: tip}

## Próximas etapas

* [Tutoriais](/docs/tutorials/index.html)
* [Amostras ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-cloud.github.io){: new_window}
* [Centro de arquitetura ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/garage/category/architectures){: new_window}
