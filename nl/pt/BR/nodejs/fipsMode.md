---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-13"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Modo FIPS
{: #fips_mode}

As versões v3.2-20160315-1257 a v3.20.2-20180523-1639 do buildpack Node.js suportam o [FIPS ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standards).  
{: shortdesc}

**O módulo OpenSSL FIPS foi descontinuado:** a partir do buildpack Node.js v3.22, o módulo OpenSSL FIPS não está mais disponível nas versões 6.x e 8.x do tempo de execução. Somente a versão 4.x continua a incluir o módulo OpenSSL FIPS. Saiba mais em [Atualizações mais recentes para o buildpack do Node.js](updates.html#fips-deprecation).

Para
usar um mecanismo ativado para FIPS, configure a variável de ambiente FIPS_MODE como true.
Por exemplo:

```
    ibmcloud cf set-env myapp FIPS_MODE true
```
{: codeblock}

É importante entender que quando FIPS_MODE é true, alguns módulos de nó podem não
funcionar.  Por exemplo, **módulos de nó que usarem [MD5 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/MD5) falharão**, como o [Express ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://expressjs.com/).  Para o Express, configurar [etag ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://expressjs.com/en/api.html) como false no
app Express pode ajudar a contornar isso. Por exemplo, é possível fazer o seguinte em seu código:
```
    app.set('etag', false);
```
{: codeblock}
Consulte este [post do stackoverflow ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://stackoverflow.com/questions/15191511/disable-etag-header-in-express-node-js)
para obter mais informações.

**NOTA** [App Management](../common/app_mng.html) e FIPS_MODE
*NÃO* são simultaneamente suportados.  Se a variável de ambiente
BLUEMIX_APP_MGMT_ENABLE for configurada e a variável de ambiente FIPS_MODE for
configurada como true, o app falhará no estágio.

Há vários métodos de verificar o estado de FIPS_MODE:
<ul>
<li> É possível verificar os logs do aplicativo para obter uma mensagem semelhante à seguinte:    

  <pre>
  Installing FIPS-enabled IBM SDK for Node.js (4.4.3) from cache
  </pre>
  {: codeblock}

Essa mensagem indica que o mecanismo node.js ativado para FIPS está em execução, mas não necessariamente que o FIPS está em execução
</li>

<li> É possível verificar o valor de **process.versions.openssl**. Por exemplo:

  <pre>
  console.log('ssl version is [' +process.versions.openssl +']');
  </pre>
  {: codeblock}

Se a versão do SSL contiver "fips", a versão do SSL que está em uso suportará FIPS.  
</li>

<li> Para node.js versão 6 e acima, é possível verificar o valor retornado por crypto.fips em código conforme o seguinte:

  <pre>
  console.log('crypto.fips== [' +crypto.fips +']');
  </pre>
  {: codeblock}

Se o valor retornado for 1, o FIPS estará em uso. Observe que crypto.fips retornará
*undefined* para as versões do node.js anteriores a 6.
</li>
</ul>

## Nodejs v4
{: #nodejs_v4_fips}

A tabela a seguir explica o comportamento do node.js v4 com FIPS:

|                 | Resultado        |
| :-------------- | :------------ |
|FIPS_MODE=true   |success (1)    |
|FIPS_MODE !=true |success (2)    |

* success (1)
  * FIPS está em uso.
  * Os logs incluirão a mensagem *Instalando o IBM SDK for Node.js ativado para FIPS*.
  * O valor retornado por process.versions.openssl conterá "fips".
* success (2)
  * FIPS *NÃO* está em uso.
  * Os logs *NÃO* incluirão a mensagem *Instalando o IBM SDK for Node.js ativado para FIPS*.
  * O valor retornado por process.versions.openssl *NÃO* conterá "fips".

## Node.js v6 e mais recente
{: #nodejs_v6_fips}

Para executar no modo FIPS com o Node.js versão 6 e mais recente, além de configurar
**FIPS_MODE=true**, também deve-se incluir **-- enable-fips** em seu comando inicial conforme
no exemplo a seguir:
```
{
    ...   
    "scripts": {
      "start": "node --enable-fips app.js"
    }
}
```
{: codeblock}

A tabela a seguir explica o comportamento do node.js v6 e mais recente com FIPS.

|                 |--enable-fips  |NO --enable-fips |
| :-------------- | :------------ | :-------------- |
|FIPS_MODE=true   |success (1)    |success (2)      |
|FIPS_MODE !=true |failure (3)    |success (4)      |

* success (1)
  * FIPS está em uso.
  * Os logs incluirão a mensagem *Instalando o IBM SDK for Node.js ativado para FIPS*.
  * O valor retornado por process.versions.openssl conterá "fips"
  * crypto.fips retornará 1, indicando que FIPS está em uso
* success (2)
  * FIPS *NÃO* está em uso.
  * Os logs incluirão a mensagem *Instalando o IBM SDK for Node.js ativado para FIPS*.
  * O valor retornado por process.versions.openssl conterá "fips"
  * crypto.fips retornará 0, indicando que FIPS *NÃO* está em uso
* failure (3)
  * FIPS *NÃO* está em uso.
  * Os logs *NÃO* incluirão a mensagem *Instalando o IBM SDK for Node.js ativado para FIPS*.
  * A preparação falhará com a mensagem "ERR node: bad option: --enable-fips"
* success (4)
  * FIPS *NÃO* está em uso.
  * Os logs *NÃO* incluirão a mensagem *Instalando o IBM SDK for Node.js ativado para FIPS*.
  * O valor retornado por process.versions.openssl *NÃO* conterá "fips"
  * crypto.fips retornará 0, indicando que FIPS *NÃO* está em uso