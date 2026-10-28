# temp
This is a shared directory



入出力概要図

```mermaid
flowchart LR
  subgraph IN["入力"]
    CONF[("設定ファイル")]
    SRV[["連携サーバ<br/>KENSHU_ フォルダ<br/>KENSHU_DATA.csv<br/>（検収ファイル：QR）"]]
    DB_IN[("HACCHU_INFO<br/>（発注情報）")]
  end

  subgraph BATCH["ステータス情報連携バッチ"]
    direction TB
    T1["1. 初期処理"]
    T2["2. 多重起動チェック"]
    T3["3-①② フォルダ取得<br/>配置完了チェック"]
    T4["3-③(a)(b) ファイル取得<br/>中身チェック"]
    T5["3-③(c)(d) 項目チェック<br/>項目数／桁数／必須／フォーマット"]
    T6["3-③(e)(f)(g) 発注情報<br/>登録/更新・コミット"]
    T1 --> T2 --> T3 --> T4 --> T5 --> T6
  end

  subgraph OUT["出力"]
    DB_OUT[("HACCHU_INFO<br/>検収年月日／来社区分<br/>／受入数量 を更新")]
    MAIL["メール送信<br/>（システム管理者）"]
    LOG["ログ出力<br/>0:正常 1:エラー終了"]
  end

  CONF --> T1
  SRV --> T3
  DB_IN --> T6
  T5 -.->|"エラー"| MAIL
  T6 --> DB_OUT
  T6 -.->|"DB異常"| MAIL
  T4 -.->|"空ファイル"| MAIL
  T6 --> MAIL
  T6 --> LOG

  style IN fill:#f5f9ff,stroke:#b4c6e7
  style OUT fill:#f5fff5,stroke:#a9d08e
  style BATCH fill:#fffdf0,stroke:#ffd966
  style DB_OUT fill:#d9f2d9,stroke:#548235,stroke-width:2px
  style MAIL fill:#fce4d6,stroke:#c55a11
```

---

## 更新項目の対応（テーブル設計書との整合）
