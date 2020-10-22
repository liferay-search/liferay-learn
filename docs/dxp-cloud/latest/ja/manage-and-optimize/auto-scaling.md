# 自動スケーリング

Liferay DXP Cloudの自動スケーリング機能は、パフォーマンスを最適化するために必要に応じて、DXPサービスのインスタンスを自動的に作成および破棄します。 これは、サーバートラフィックの増加、メモリリーク、またはその他の急な変化に対処します。 デフォルトでは、この機能はすべてのDXP Cloudアカウントで *無効* になっています。

この機能を使用して、サービスはLiferay DXPインスタンスの数を最大10まで自動的に増加（アップスケール）したり、 [`LCP.json`](../reference/configuration-via-lcp-json.md)の `スケール` プロパティで指定された数まで減少（ダウンスケール）したりできます。 `スケール` プロパティは、実行するインスタンスの最小数を指定します。

``` json
  "scale": 2,
```

``` note::
   オートスケーリングは、本番環境のLiferay DXPサービスでのみ利用可能です。 オートスケーリングの課金方法の詳細については、`オートスケーリングの課金方法 <https://help.liferay.com/hc/en-us/articles/360030843592-How-Auto-Scaling-is-charged->`_.
```

## JVMメモリ設定

自動スケーリングが正しく機能するためには、LiferayイメージのJVMに適切なメモリ割り当てを設定することが重要です。 この割り当ては、サーバーの負荷に応じてメモリ使用量を拡大または縮小できるようにするために必要です。

`ライフレイ` サービスの `LIFERAY_JAVA_OPTS` 環境変数を設定して、 `-Xms` および `-Xmx` フラグを使用してメモリを割り当てます。 `-Xms` フラグは、サービスの開始時に初期メモリ割り当てを設定し、 `-Xmx` フラグは、JVMの最大メモリ割り当てを決定します。 たとえば、合計16 GBがプロビジョニングされ、このサービスで利用できる場合、プロパティを次のように設定できます。

    -Xms4096m -Xmx12288m

推奨される設定は、 `-Xms` フラグを使用可能なメモリの25%を使用して設定し、 `-Xmx` フラグを使用可能なメモリの75%を使用して設定することです。 `liferay` サービスで使用可能なさまざまなレベルのメモリについて、推奨値のリファレンスについては次の表を参照してください`

| **使用可能なメモリ** | **推奨されるLIFERAY \ _JAVA \ _OPTS** |
| ------------ | ------------------------------------ |
| 8 GB         | \-Xms2048m -Xmx6144m                |
| 16 GB        | \-Xms4096m -Xmx12288m               |
| 24 GB        | \-Xms6144m -Xmx18432m               |
| 32 GB        | \-Xms8192m -Xmx24576m               |
| 64 GB        | \-Xms16384m -Xmx49152m              |

``` note::
   変数 ``LIFERAY_JAVA_OPTS`` は ``-Xms``, ``-Xmx`` の他にも、他のフラグと一緒に使われることがあります。 他のフラグが存在する場合は、他のフラグを削除せずにメモリ引数で環境変数を更新します。
```

この環境変数を `liferay` サービスに追加するためのヘルプについては [環境変数の定義](../reference/defining-environment-variables.md) を参照してください。

## 自動スケーリングの管理

次の手順に従って、DXP Cloud管理コンソールで自動スケーリングを有効または無効にします。

1.  本番環境に移動します。
2.  *Services* → *Liferay* → *Scale*ます。
3.  自動スケーリングが無効になっている場合は、*[Enable Auto Scaling]* をクリックして有効にします。 自動スケーリングが既に有効になっている場合は、[ *無効にするAuto Scaling* ]をクリックして無効にします。

自動スケーリングを有効にすると、DXP Cloudはサービスを監視し、事前定義されたしきい値に従って自動的にスケーリングします。

![図1：サービスの [Scale] タブから自動スケーリングを有効または無効にします。](./auto-scaling/images/01.png)

## 平均目標使用率の指定

システム管理者は、 *平均目標使用率*を指定できます。 この値は、Liferay DXPサービス全体のメモリおよびCPU使用率の平均です。 自動スケーリングが開始される前に、その値のしきい値を超えなければなりません。

たとえば、3つのサービスインスタンスがそれぞれメモリの70％、90％、95％を使用する場合、平均メモリ使用率は85％です。 目標平均使用率が90に設定されている場合、アップスケーリングは必要ありません。 この状況でのアップスケーリングは、平均メモリ使用率がターゲットを超えた場合にのみ発生します。

使用可能なメモリの合計は、 `LCP.json`の [Configuration via LCP.json](../reference/configuration-via-lcp-json.md)で参照される `メモリ` プロパティで指定されます。

サービスの `LCP.json`の `autoscale` プロパティでターゲットの平均使用率を指定します。

``` json
"autoscale": {
    "cpu": 80,
    "memory": 90
}
```

`autoscale` プロパティが設定されていない場合、CPUとメモリの両方の使用率の目標平均使用率はデフォルトで80になります。

## 自動スケーリングとDXPアクティベーションキー

オンプレミスでLiferay DXPを使用するには、通常、アクティベーションキーを展開して検証する必要があります。 DXP Cloudで自動スケーリングが有効になっている場合、DXP CloudチームはDXPアクティベーションキーの問題を解決し、必要に応じてアクティベーションキーを追加および削除します。