# paypay
sequenceDiagram
    participant User as ユーザー
    participant FuelDispenser as 給油機
    participant MerchantSystem as 加盟店システム
    participant PayPay as PayPay決済システム
    participant Backend as 支払い確認システム

    User->>FuelDispenser: 決済方法選択（PayPay）
    FuelDispenser->>MerchantSystem: PayPay決済リクエスト
    MerchantSystem->>PayPay: QRコード生成リクエスト
    PayPay-->>MerchantSystem: QRコード発行
    MerchantSystem-->>FuelDispenser: QRコード表示
    User->>PayPay: PayPayアプリでQRコードスキャン & 支払い実行
    PayPay->>MerchantSystem: 決済通知（成功/失敗）
    alt 決済成功
        MerchantSystem->>FuelDispenser: 給油許可
        User->>FuelDispenser: 給油開始
        FuelDispenser->>MerchantSystem: 給油データ送信
        MerchantSystem->>PayPay: 最終決済確定（給油量分）
        PayPay->>Backend: 支払い完了通知
        Backend-->>MerchantSystem: 支払い確認完了
    else 決済失敗
        MerchantSystem->>FuelDispenser: 給油不可通知
        FuelDispenser->>User: 決済エラー表示
    end

