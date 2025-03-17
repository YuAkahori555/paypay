```mermaid
sequenceDiagram
    participant User as ユーザー
    participant FuelDispenser as 給油機
    participant MerchantSystem as 加盟店システム
    participant PayPay as PayPay決済システム
    participant Backend as 支払い確認システム

    User->>MerchantSystem: 決済方法選択（PayPay）
    MerchantSystem->>PayPay: QRコード生成リクエスト
    PayPay-->>MerchantSystem: QRコード発行
    MerchantSystem-->>User: QRコード表示
    User->>PayPay: QRコードスキャン & 支払い実行
    PayPay->>Backend: 決済リクエスト
    Backend->>PayPay: 決済確認
    PayPay-->>Backend: 決済結果通知
    Backend->>MerchantSystem: 決済成功通知
    MerchantSystem->>FuelDispenser: 給油許可
    User->>FuelDispenser: 給油開始
    FuelDispenser->>MerchantSystem: 給油完了通知


