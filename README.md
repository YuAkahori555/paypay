```mermaid
sequenceDiagram
    participant User as ユーザー
    participant FuelDispenser as 給油機
    participant MerchantSystem as 加盟店システム
    participant PayPay as PayPay決済システム
    participant Backend as 支払い確認システム

    MerchantSystem->>User: 決済方法選択画面を表示

    alt ユーザーがキャンセル（決済方法選択前）
        User->>MerchantSystem: キャンセル
        MerchantSystem-->>User: 取引終了
    else
        User->>MerchantSystem: 決済方法選択（PayPay）
        MerchantSystem->>PayPay: QRコード生成リクエスト
        PayPay-->>MerchantSystem: QRコード発行
        MerchantSystem-->>User: QRコード表示

        alt ユーザーがキャンセル（QRコードスキャン前）
            User->>MerchantSystem: キャンセル
            MerchantSystem-->>User: 取引終了
        else
            User->>PayPay: QRコードスキャン

            alt ユーザーがキャンセル（QRコードスキャン後）
                User->>PayPay: キャンセル
                PayPay-->>MerchantSystem: キャンセル通知
                MerchantSystem-->>User: 取引終了
            else
                User->>PayPay: 支払い実行
                PayPay->>Backend: 決済リクエスト

                alt 決済不可（決済確認前のキャンセル）
                    PayPay-->>User: 決済エラーメッセージ表示
                else
                    Backend->>PayPay: 決済確認
                    PayPay-->>Backend: 決済成功 or 決済失敗 通知

                    alt 決済成功
                        Backend->>MerchantSystem: 決済成功通知
                        MerchantSystem->>User: 決済成功メッセージ表示
                        MerchantSystem->>FuelDispenser: 給油許可

                        opt ユーザーが給油を実施
                            User->>FuelDispenser: 給油開始
                            FuelDispenser->>MerchantSystem: 給油完了通知
                        end
                    else 決済失敗
                        Backend->>MerchantSystem: 決済失敗通知
                        MerchantSystem->>User: 決済失敗通知
                    end
                end
            end
        end
    end





