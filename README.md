```mermaid
sequenceDiagram
    participant User as ユーザー
    participant FuelDispenser as 給油機
    participant MerchantSystem as 加盟店システム
    participant PayPay as PayPay決済システム
    participant Backend as 決済システム

    User->>MerchantSystem: 決済方法選択（PayPay）
    MerchantSystem->>PayPay: QRコード生成リクエスト
    PayPay-->>MerchantSystem: QRコード発行
    MerchantSystem->>User: QRコード表示

    User->>PayPay: QRコードスキャン & 支払い実行

    alt 決済リクエスト受理エラー
        PayPay-->>MerchantSystem: 決済リクエスト受理不可（エラー通知）
        MerchantSystem->>User: 決済エラーメッセージ表示
    else
        PayPay->>Backend: 決済リクエスト
        Backend->>PayPay: 決済確認
        PayPay-->>Backend: 決済結果通知

        alt 決済成功
            Backend-->>PayPay: 決済成功判定
            PayPay-->>Backend: 決済成功通知
            Backend->>MerchantSystem: 決済成功通知
            MerchantSystem->>User: 決済成功メッセージ表示
            MerchantSystem->>FuelDispenser: 給油許可

            opt ユーザーが給油を実施
                User->>FuelDispenser: 給油開始
                FuelDispenser->>MerchantSystem: 給油完了通知
            end
        else 決済失敗
            Backend-->>PayPay: 決済失敗判定
            PayPay-->>Backend: 決済失敗通知
            Backend->>MerchantSystem: 決済失敗通知
            MerchantSystem->>User: 決済失敗メッセージ表示
        end
    end
