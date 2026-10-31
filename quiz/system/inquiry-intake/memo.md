# メモ

## Transactional Outbox
Transactional Outboxは、データベースへの保存と、キューに送る通知依頼の作成を「一緒に確定」する方法です。

DBへの保存とService Busへの送信を別々に行うと、片方だけ成功することがあります。たとえば問い合わせをSQLに保存できた後、Service Busが一時停止すると、受付記録はあるのに通知依頼がキューに届かない状態になります。

そこで、通知メッセージをいったんSQLの Outboxテーブルに保存します。問い合わせ本体とOutboxの行を、同じDBトランザクションでコミットします。
```mermaid
sequenceDiagram
    participant A as App Service
    participant DB as Azure SQL Database
    participant R as Outbox送信処理
    participant SB as Service Bus
    participant F as 通知Function

    A->>DB: 問い合わせ + Outbox行を保存
    Note over A,DB: 1つのDBトランザクションでコミット
    DB-->>A: 保存成功
    A-->>A: 受付番号を返す

    R->>DB: 未送信のOutbox行を取得
    DB-->>R: 通知メッセージ
    R->>SB: メッセージを送信
    SB-->>R: 送信成功
    R->>DB: Outbox行を送信済みに更新

    SB->>F: メッセージを配信
    F->>F: 担当者へメールを送信
```

> お問い合わせ番号を発行しないといけないため、キューに入れてあとから処理は不親切だった
- 外部リソースへの接続（ServiceBUs）が不確定なため、確実に更新するために行う。
- ttl設定してServiceBusaから帰ってこないときは再送、でもいい（これだとトランザクション張らなくていいが、成功時のDB更新が失敗する可能性がある∧インデックスが大変になることも）

##これから
- 模範解答の深堀
    - 冪等性(どうやって極力1件のメールを届けるか)
    - CQRSについて混乱していそうなので聞く
    - メール配信サービスへのリクエストの深堀
        - 送信完了通知が来ない場合どうするか
        - 外部サービスのレートリミットはどう対応するか