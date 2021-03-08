# サーバー管理パネルの使用

サーバー管理は、さまざまな低レベルのLiferay DXP構成を保持するシステムスコープの管理パネルです。 サーバー管理では、システムメモリの使用状況、低レベルのプロパティ、一部のサードパーティ統合、ドキュメントリポジトリの移行、ログ、スクリプト、メールサーバーの構成、ポータルのシャットダウンを管理および監視できます。

サーバー管理にアクセスするには、 *コントロールパネル* → *構成* → *サーバー管理*クリックします。

![サーバー管理の[リソース]タブには、サーバーのメモリ使用量のグラフが表示されます。](using-the-server-administration-panel/images/01.png)

サーバー管理の機能はタブに分割されています。

| サーバー管理タブ | 説明                                                                                                                                                                                                                                                                                  | ドキュメントリンク                                                                                                            |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| 資源       | システムを監視し、管理タスクを実行します（ガベージコレクターの実行、データベースキャッシュのクリアなど）。                                                                                                                                                                                                                               | [システムリソースの管理](./managing-system-resources.md)                                                                        |
| ログレベル    | ログレベルを表示および設定します。 Liferay DXPクラスおよびパッケージのログレベルを変更します。 ログ設定にカスタムオブジェクトを追加します。                                                                                                                                                                                                        | [ロギングの構成](./configuring-logging.md)                                                                                  |
| プロパティ    | システムとポータルのプロパティを表示します。 システムプロパティは、JVMおよびLiferay DXPのシステムプロパティを示し、実行中の構成をデバッグまたは確認するときに使用されます。 ポータルプロパティには、現在のポータルプロパティ値が表示されます。 詳細については、 [ポータルプロパティリファレンスドキュメント](https://docs.liferay.com/portal/7.3-latest/propertiesdoc/portal.properties.html) を参照してください。                        | [ポータルプロパティ](./../../installation-and-upgrades/reference/portal-properties.md)                                        |
| データ移行    | あるリポジトリから別のリポジトリにドキュメントを移行します。 たとえば、ドキュメントを別のディスクの新しいリポジトリや新しい形式に移行できます。                                                                                                                                                                                                            | [ファイルストレージの移行](./../file-storage/file-store-migration.md)                                                            |
| 郵便物      | [ポータルプロパティファイル](../../installation-and-upgrades/setting-up-liferay-dxp/configuring-mail/alternative-email-configuration-methods.md#configuring-the-built-in-mail-session-using-portal-properties)代わりに、このタブからメールサーバーを構成できます。 これらの設定は、 `portal-ext.properties` ファイルのメールサーバー設定を上書きします。 | [メールサーバーに接続](../../installation-and-upgrades/setting-up-liferay-dxp/configuring-mail/connecting-to-a-mail-server.md) |
| 外部サービス   | 画像、オーディオファイル、ビデオファイルのファイルプレビューを生成するための外部サービスを構成します。                                                                                                                                                                                                                                 | [外部サービスの構成](./configuring-external-services.md)                                                                      |
| 脚本       | 移行または管理コードを実行するために、スクリプトコンソールでGroovyスクリプトを記述します。                                                                                                                                                                                                                                    | [スクリプトエンジンの使用](./../using-the-script-engine/using-the-script-engine.md)                                              |
| シャット ダウン | ログインしているユーザーにシャットダウンが迫っていることを警告するシャットダウンをスケジュールします（今から *x* 分後）。                                                                                                                                                                                                                     | [シャットダウンイベントの構成](./configuring-a-shutdown-event.md)                                                                  |