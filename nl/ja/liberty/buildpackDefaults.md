---

copyright:
  years: 2015, 2018
lastupdated: "2017-10-26"
subcollection: "liberty"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# ビルドパックのデフォルト
{: #buildpack_defauts}

Liberty ビルドパックは {{site.data.keyword.Bluemix}} で頻繁に更新されます。 リリースごとに、セキュリティーに関するフィックスや機能拡張が含まれることがあります。

ビルドパックには、例えば、WAR アプリケーションまたは EAR アプリケーションの Java バージョンまたは Liberty フィーチャー・セットなど、各種設定のデフォルト値が含まれています。 それらのデフォルト値の一部はビルドパックのリリースが変わる際に変更されることがあり、それがアプリケーションに悪影響を及ぼす可能性があります。 ビルドパックのデフォルトの変更によるアプリケーションへの影響を防止するために、ビルドパックのデフォルトに依存しないようにアプリケーションを構成する手順を実行してください。

## Liberty のバージョン
{: #liberty_versions}

ビルドパックは、Liberty ランタイムの以下の 2 つのバージョンを提供します。
1. 長期安定リリース
  * これは、デフォルトの Liberty ランタイムです。
  * [ベータ・フィーチャー](/docs/runtimes/liberty/usingBetaFeatures.html)は提供しません。
  * 通常、四半期ベースで更新されます。

2. 月次リリース
  * **JBP_CONFIG_LIBERTY** 環境変数の **"version: +"** 値と **IBM_LIBERTY_MONTHLY** 環境変数の **true** を設定して、明示的に使用可能にする必要があります。
  * [月次フィーチャー](/docs/runtimes/liberty/usingMonthlyRuntime.html)を提供します。
  * 通常、4 週間ごとに更新されます。

## Liberty フィーチャー
{: #liberty_features}

WAR ファイルまたは EAR ファイルをデプロイするときには、
デフォルトの Liberty フィーチャー・セットが指定されたアプリケーション構成がビルドパックによって提供されます。 デフォルトの Liberty フィーチャー・セットが、ビルドパックのリリースが変わる際に変更されることがまれにあります。 デフォルトのフィーチャー・セットの変更がアプリケーションにマイナスの影響を与える可能性があります。 フィーチャーのデフォルトに変更があってもアプリケーションが影響を受けないことを確実にするためのオプションがあります。

* JBP_CONFIG_LIBERTY 環境変数を設定して、アプリケーションに対して有効にするフィーチャーのリストを明示的に指定します。 詳しくは、[『スタンドアロン・アプリケーション』](/docs/runtimes/liberty/optionsForPushing.html#stand_alone_apps)を参照してください。
* アプリケーションを [サーバー・ディレクトリー
](/docs/runtimes/liberty/optionsForPushing.html#server_directory)または[パッケージされたサーバー](/docs/runtimes/liberty/optionsForPushing.html#packaged_server)としてデプロイします。 カスタム server.xml ファイルを作成して、
アプリケーションが必要とするフィーチャーのセットを正確に指定します。

サーバー・ディレクトリーまたはパッケージされたサーバーとしてデプロイされたアプリケーションは、
Liberty フィーチャーのデフォルトの変更に影響されません。

## Java バージョン
{: #java_version}

ビルドパックは、
アプリケーションのデフォルト JRE を提供します。 ビルドパックのリリースが変わる際に JRE のメジャー・バージョンまたはマイナー・バージョンが変更される可能性があります。 JRE のメジャー・バージョンはめったに更新されませんが、
マイナー・バージョンは頻繁に更新されることがあります。 JRE のメジャー・バージョンの変更はアプリケーションに悪い影響を及ぼすことがあります。

メジャー・バージョンの変更がアプリケーションに影響を及ぼさないことを確実にするには、[JRE のカスタマイズ](/docs/runtimes/liberty/customizingJRE.html)に説明されているように、該当の JRE バージョンを指定して環境変数を設定します。 最高の結果を得るには、アプリケーションに対して Java 8 を採用してください。