# Azure の技術スタックを使うクイズ案

実務で触れた App Service・Azure Functions・Service Bus を足場に、Container Apps・Event Grid・Event Hubs と永続層の選択へ進む問題例。各レベル5問、計15問あり、独立して解ける。

| レベル | 問題例 | 主な判断点 |
|---|---|---|
| 初級 | [写真投稿と縮小画像の作成](./初級/image-upload/QUESTION.md) | Blob、Event Grid、Functions の役割 |
| 初級 | [在庫更新と欠品通知](./初級/stock-alert/QUESTION.md) | RDB の正本と非同期通知 |
| 初級 | [月次レポートの自動作成](./初級/monthly-report/QUESTION.md) | 定期処理、CSV 保管、実行基盤 |
| 初級 | [商品カタログの保存先選び](./初級/product-catalog/QUESTION.md) | RDB・NoSQL・Blob の選択 |
| 初級 | [イベント参加申込と確認メール](./初級/event-registration/QUESTION.md) | DB 制約、定員、非同期メール |
| 中級 | [商品データの一括取込](./中級/product-import/QUESTION.md) | Event Grid と Service Bus、取込処理、SQL と Cosmos DB |
| 中級 | [期限付き座席予約](./中級/reservation-expiry/QUESTION.md) | 状態遷移、期限、同時実行 |
| 中級 | [工場設備の状態ダッシュボード](./中級/telemetry-dashboard/QUESTION.md) | Event Hubs、Container Apps、現在値と履歴 |
| 中級 | [利用者の取引履歴フィード](./中級/activity-feed/QUESTION.md) | SQL の正本と Cosmos DB の参照用データ |
| 中級 | [配送状況の取り込み](./中級/delivery-status/QUESTION.md) | Webhook、重複・順序、Service Bus |
| 上級 | [注文と売上速報の基盤](./上級/commerce-events/QUESTION.md) | 3 種のメッセージ基盤と複数の永続層 |
| 上級 | [会員ポイント台帳と残高表示](./上級/points-ledger/QUESTION.md) | 台帳、投影、再計算、照合 |
| 上級 | [配送車両の位置追跡と遅延通知](./上級/logistics-stream/QUESTION.md) | 大量ストリーム、遅延データ、再処理 |
| 上級 | [マルチテナント SaaS の監査ログ](./上級/tenant-audit/QUESTION.md) | テナント分離、検索、長期保管 |
| 上級 | [保険請求書類の審査基盤](./上級/claims-review/QUESTION.md) | ファイル処理、人手審査、版管理 |

各問題の「候補」にあるサービスは、すべて採用する前提ではない。要件から必要なものを選び、採用しないものの理由も説明する。データも用途ごとに、RDB、NoSQL、オブジェクトストレージのどれを正本・参照用・原本保管にするか判断する。

## レベルの境界

- **初級**: 既知のサービスをつなぎ、同期処理と非同期処理、構造化データとファイルの置き場を説明できる。
- **中級**: 異なるイベント経路と実行基盤を比較し、重複イベント・再処理・RDB と NoSQL の役割を設計できる。
- **上級**: 業務上の正しさと速報性を分け、複数のメッセージ基盤・永続層をまたぐ整合性、障害回復、再集計まで設計できる。

「添付付き問い合わせの受付」は検討案から正式な問題 [quiz/system/inquiry-intake](../system/inquiry-intake/QUESTION.md) に移されているため、上の15問には含めていない。

## サービスの役割を確認する資料

- [Event Grid・Event Hubs・Service Bus の比較](https://learn.microsoft.com/azure/event-grid/compare-messaging-services)
- [Azure のデータストアの選び方](https://learn.microsoft.com/azure/architecture/data-guide/technology-choices/understand-data-store-models)
- [バックグラウンドジョブの実行基盤](https://learn.microsoft.com/azure/architecture/best-practices/background-jobs)
- [Blob Storage のイベントと発火条件](https://learn.microsoft.com/azure/event-grid/event-schema-blob-storage)
- [Service Bus のスケジュールメッセージ](https://learn.microsoft.com/azure/service-bus-messaging/message-sequencing)
- [Event Hubs のデータを Blob Storage に保存する Capture](https://learn.microsoft.com/azure/event-hubs/event-hubs-capture-overview)
