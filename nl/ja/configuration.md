---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: configure node env, node environment, node credentials, ibm-cloud-env node

subcollection: nodejs

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Node.js 環境の構成
{: #configure-nodejs}

クラウド・ネイティブの原則を実装することによって、Node.js アプリケーションを、テストから実動へなど、ある環境から別の環境へ移動することができ、その際、コードを変更することはなく、したがってテストされていないコード・パスを発生させることもありません。

開発環境によって config の提供方法に大きな違いがある場合に問題が発生します。 例えば、ストリング化された JSON オブジェクトを使用する Cloud Foundry に対して、Kubernetes はフラット値またはストリング化された JSON オブジェクトを使用します。 Kubernetes は別にして、ローカル開発では他にもさまざまな考慮事項があります。 パブリックとプライベートの間で資格情報の提供方法が異なることがあるため、環境間でアプリを変更しないことはさらに難しくなります。

既存のアプリケーションに {{site.data.keyword.cloud}} サポートを追加する必要があるのか、それともスターター・キットを使用してアプリを作成するのかにかかわらず、目標は、どのような開発プラットフォームでも Node.js アプリの移植性を確保することです。

## 既存の Node.js アプリケーションへの {{site.data.keyword.cloud_notm}} 構成の追加
{: #addcloud-env-nodejs}

[`ibm-cloud-env`](https://github.com/ibm-developer/ibm-cloud-env){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") モジュールはさまざまなクラウド・プロバイダー (CloudFoundry や Kubernetes など) から環境変数を集約するため、アプリケーションは環境から独立しています。

### `ibm-cloud-env` モジュールのインストール
{: #install-module-nodejs}

1. 以下のコマンドを使用して `ibm-cloud-env` モジュールをインストールします。
  ```
  npm install ibm-cloud-env
  ```
  {: codeblock}

2. 以下のように `mappings.json` を参照して、コード内でこのモジュールを初期化します。
  ```js
  var IBMCloudEnv = require('ibm-cloud-env');
  IBMCloudEnv.init("/path/to/the/mappings/file/relative/to/project/root");
  ```
  {: codeblock}

  mappings ファイル・パスが `IBMCloudEnv.init()` に指定されていない場合、モジュールは `/server/config/mappings.json` のデフォルト・パスから mappings をロードしようとします。
  {: tip}

  `mappings.json` ファイルの例:
  ```json
  {
    "service1-credentials": {
        "searchPatterns": [
            "cloudfoundry:my-service1-instance-name", 
            "env:my-service1-credentials", 
            "file:/localdev/my-service1-credentials.json" 
        ]
    },
    "service2-username": {
        "searchPatterns":[
            "cloudfoundry:$.service2[@.name=='my-service2-instance-name'].credentials.username",
            "env:my-service2-credentials:$.username",
            "file:/localdev/my-service1-credentials.json:$.username" 
        ]
    }
  }
  ```
  {: codeblock}

### サービス資格情報の取得
{: #nodejs-get-creds}

以下のコマンドを使用して、アプリケーション内で値を取得します。

1. 変数 `service1credentials` を取得します。
  ```js
  // this will be a dictionary
  var service1credentials = IBMCloudEnv.getDictionary("service1-credentials");
  ```
  {: codeblock}

2. 変数 `service2username` を取得します。
  ```js
  var service2username = IBMCloudEnv.getString("service2-username"); // this will be a string
  ```
  {: codeblock}

これで、さまざまなクラウド計算プロバイダーから生じる相違点を抽象化することによって、任意のランタイム環境にアプリケーションを実装できるようになります。

### タグおよびラベルの値のフィルタリング
{: #filter-values-nodejs}

以下の例に示すように、モジュールによって生成された資格情報を、サービス・タグとサービス・ラベルに基づいてフィルターに掛けることができます。
```js
var filtered_credentials = IBMCloudEnv.getCredentialsForServiceLabel('tag', 'label', credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## スターター・キット・アプリからの Node.js 構成マネージャーの使用
{: #nodejs-config-skit}

[スターター・キット](https://cloud.ibm.com/developer/appservice/starter-kits){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を使用して作成された Node.js アプリには、多くのクラウド・デプロイメント・ターゲット ([Kubernetes](/docs/containers?topic=containers-getting-started)、[Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)、[{{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-about)、[仮想サーバー (VSI)](/docs/vsi?topic=virtual-servers-getting-started-tutorial)、または [{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-getting_started) など) での実行に必要な資格情報と構成が自動的に付属します。

  いくつかのスターター・キットで VSI デプロイメントを使用できます。この機能を使用するには、[{{site.data.keyword.cloud_notm}} ダッシュボード](https://{DomainName})に移動し、**「アプリ」**タイルで**「アプリの作成」**をクリックします。
  {: note} 

### サービス資格情報について
{: #credentials-nodejs}

サービスに関するアプリケーション構成情報は `/server/config` ディレクトリーの `localdev-config.json` ファイルに保管されます。 このファイルは、Git に機密情報が保管されるのを防ぐために、`.gitignore` ディレクトリーにあります。 ローカルで実行される構成済みサービス用の接続情報 (ユーザー名、パスワード、およびホスト名など) がこのファイルに保管されます。

アプリケーションは、構成マネージャーを使用して、環境およびこのファイルから接続情報および構成情報を読み取ります。 また、`server/config` ディレクトリーにあるカスタムビルトの `mappings.json` を使用して、各サービスの資格情報がある場所を伝達します。

ローカルで実行されているアプリケーションは、`mappings.json` ファイルから読み取ったアンバインドされた資格情報を使用して、{{site.data.keyword.cloud_notm}} サービスに接続できます。 アンバインドされた資格情報を作成する必要がある場合、{{site.data.keyword.cloud_notm}} Web コンソールから行うか、または、[CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") の `cf create-service-key` コマンドを使用して行うことができます。

アプリケーションを {{site.data.keyword.cloud_notm}} にプッシュすると、これらの値は使用されなくなります。 代わりに、アプリケーションは環境変数を使用して、バインドされたサービスに自動的に接続します。

* **Cloud Foundry**: サービス資格情報は、`VCAP_SERVICES` 環境変数から取得されます。 Cloud Foundry Enterprise Edition の場合、詳細については、[入門チュートリアル](/docs/cloud-foundry?topic=cloud-foundry-getting-started#getting-started)を参照してください。

* **Kubernetes**: サービス資格情報は、サービスごとに個別の環境変数から取得されます。

* **{{site.data.keyword.cloud_notm}} Container Service**: サービス資格情報は、VSI または {{site.data.keyword.openwhisk}} (Openwhisk) から取得されます。

## 次のステップ
{: #next_steps-config notoc}

`ibm-cloud-config` では、値の検索に使用できる検索パターン・タイプとして、`cloudfoundry`、`env`、および `file` の 3 つがサポートされています。 サポートされる他の検索パターンおよび検索パターン例を確認したい場合は、[Usage](https://github.com/ibm-developer/ibm-cloud-env#usage){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") セクションを参照してください。
