# カスタムドメイン

カスタムドメインをDXP Cloudサービスに追加するには、まずそのドメインを専用環境IPに `A` レコードとして登録する必要があります。 これは、選択したドメイン名レジストラを使用して行います。 DNSの伝播が有効になるまでに24〜48時間かかることがありますが、場合によっては数分しかかからないこともあります。 この伝播プロセス中、デバイスが到達するDNSサーバーによっては、1つのデバイスが更新されたアドレスでドメインに到達できる一方で、別のデバイスは到達できない場合があります。 最終的に、ドメインは任意のデバイスから到達可能になり、Ingress Load Balancerから標準の `デフォルトバックエンド-404` エラーを返します。 これで、次のステップの準備が整いました。

> **注：** 専用環境IPは、サービスのカスタムドメインまたはネットワークページにあります。

![図1：この例では、Cloudflareをドメイン名レジストラとして使用してDNSレコードを作成しています。](./custom-domains/images/01.png)

## DXP Cloudサービスへのカスタムドメインの追加

ドメインが到達可能になったら、サービスに追加すると、Liferay DXP Cloudがルーティングを処理します。 これは、Webコンソールまたは `LCP.json`を使用して行うことができます。

次の手順に従って、Webコンソールからカスタムドメインを追加します。

1.  環境ページに移動します。
2.  カスタムドメインを追加するサービスを選択します。
3.  [ *Custom Domains* ]タブをクリックして、カスタムドメインを追加します。 複数のカスタムドメイン名を追加するには、作成された新しい各フィールドにすべての名前を入力し、[ *Update Custom Domains*]をクリックします。 カスタムドメインの数は、プロビジョニングプロセス中に設定されたクォータによって制限できます。

![図2：サービスの[カスタムドメイン]タブを使用してドメインを追加します。](./custom-domains/images/02.png)

または、サービスの `LCP.json`の `customDomains` プロパティを使用してカスタムドメインを追加できます。

``` json
{
  "id": "webserver",
  "loadBalancer": {
    "customDomains": ["acme.com", "www.acme.com"]
  }
}
```

DXP Cloudは、そのIngress Load Balancerを50個のカスタムドメインに制限していることに注意してください。 設定が完了すると、DXP Cloudがルーティングを処理します。

## カスタムドメインのステータスの確認

カスタムドメインを追加したら、そのステータスを確認する方法が2つあります。

1.  サービスエンドポイントが到達可能になり、 `デフォルトバックエンド - 404` エラーで応答を停止するまで待ちます。
2.  DXP Cloud管理コンソールの[ネットワーク]ページのステータスに移動します。 ![図3：すべてのエンドポイントとカスタムドメインを[ネットワーク]ページに表示します。](./custom-domains/images/03.png)

カスタムドメインを設定すると、バックエンドで次のプロセスが発生するため、検証に時間がかかる場合があります。

1.  ルートはIngress Load Balancerに追加する必要があります。 これは、リージョンによっては約30分かかる場合があります。
2.  Liferay DXP Cloudが [達し、SSLサーバー証明書を](https://letsencrypt.org/) に暗号化しましょう。 *Let's Encrypt* サイトがチャレンジで応答します。
3.  チャレンジに合格すると、Ingress Load Balancerは証明書で更新され、サービスが到達可能で安全な状態になります。 チャレンジプロセス中に誰かがドメインにアクセスしようとすると、ブラウザにセキュリティ警告が表示されます。 プロセスがまだ完了していないため、これらの警告は無視しても問題ありません。

カスタムSSL証明書の設定方法など、Liferay DXP CloudのSSL証明書の詳細については、 [ロードバランサー](./load-balancer.md) 記事を参照してください。

## 追加情報

  - [LCP.jsonによる設定](../../reference/configuration-via-lcp-json.md)