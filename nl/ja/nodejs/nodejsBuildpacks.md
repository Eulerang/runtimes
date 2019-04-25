---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-27"
subcollection: "Nodejs"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Node.js buildpacks

{{site.data.keyword.Bluemix}} は、複数バージョンの Node.js ビルドパックを提供します。
{: shortdesc}

* {{site.data.keyword.IBM_notm}} 作成の **sdk-for-nodejs** ビルドパックは、{{site.data.keyword.Bluemix_notm}} で Node.js アプリケーションに使用されるデフォルトのビルドパックです。
* **nodejs_buildpack** は、Cloud Foundry コミュニティーによって提供されるコミュニティー・ビルドパックです。

**sdk-for-nodejs** ビルドパックは、{{site.data.keyword.Bluemix_notm}} 内の **nodejs_buildpack** に優先します。 アプリケーションで **sdk-for-nodejs** ビルドパックではなく **nodejs_buildpack** を使用する場合、`ibmcloud cf push` コマンドで `-b` オプションを使用するなどしてビルドパックを指定する必要があります。

一般的には、現行 **sdk-for-nodejs** ビルドパックとバックレベル・バージョンが使用可能です。  使用可能なすべてのビルドパックを確認するには、`ibmcloud cf buildpacks` コマンドを使用してください。  例えば、次のとおりです。

```
   ibmcloud cf buildpacks
   Getting buildpacks...

   buildpack                                 position   enabled   locked   filename   

   sdk_for_nodejs                            2          true      false    buildpack_sdk-for-nodejs_v2.8-20151209-1403.zip   
   nodejs_buildpack                          5          true      false    nodejs_buildpack-cached-v1.5.0.zip   
   sdk-for-nodejs_v2_7-20151118-1003         17         true      false    buildpack_sdk-for-nodejs_v2.7-20151118-1003.zip
```
{: codeblock}
