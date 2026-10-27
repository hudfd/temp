# システム間I/F機能関連図

| 項目 | 内容 |
|---|---|
| プロジェクト名 | トク九州 |
| システム名/サブシステム名 | Meister SRM／資材 |
| 図名 | システム間I/F機能関連図〔かんばん発注～納入～検収（No19/No22）〕 |

> 参考図のうち、担当範囲（かんばん発注情報の連携、ステータス（検収）情報の連携、資材発注書・受領書発行画面／一括印刷）のみを記載。

```mermaid
flowchart LR

%% ============ TMK ============
subgraph TMK["TMK"]
  direction LR

  subgraph TMK1["TMK資材<br/>（TMK基盤）"]
    direction TB
    F01["発注処理<br/>QR読取（発注）"]
    F02["受領・検収<br/>QR読取（受入）"]
  end

  subgraph TMK2["連携サーバ<br/>（MeisterSRM）"]
    direction TB
    FL01[/"発注情報"/]
    FL02[/"ステータス情報<br/>（検収QRデータ）"/]
  end
end

%% ============ MeisterSRM ============
subgraph SRM["MeisterSRM"]
  direction LR

  subgraph OPT["連携オプション"]
    direction TB
    F03["自動起票連携<br/>（参考：既存）"]
    F04["ステータス情報連携バッチ<br/>（新規）"]
  end

  subgraph CALL["受付確認・かんばん発行"]
    direction TB
    DB01[("発注情報<br/>（ステータス）")]

    subgraph BUY["仕入先様"]
      direction TB
      F05["受付確認・かんばん<br/>発行一覧"]
      F06["資材発注書・受領書<br/>発行画面<br/>（サプライヤ・新規）"]
      F07["資材発注書・受領書<br/>発行 一括印刷"]
    end
  end
end

%% ============ データの流れ ============
%% かんばん発注情報（オンライン）
F01 -->|"かんばん発注データ<br/>（自動連携）"| FL01
FL01 --> F03
F03 --> DB01
DB01 -->|"オンライン"| F05

%% ステータス（検収）情報（バッチ）
F02 -->|"検収データ（QR）<br/>発注書番号・納品書番号・<br/>発注年月日・検収年月日・<br/>未検収区分・受入数量"| FL02
FL02 -.->|"バッチ連携"| F04
F04 -.->|"ステータス更新"| DB01

%% 仕入先様画面への展開（オンライン）
DB01 --> F06
F06 --> F07

%% ============ 注記 ============
NOTE1>"QRコード（検収データ）で<br/>ステータス（検収年月日）を更新"]
MAT["Material-XX<br/>（担当機能）"]
FL02 ~~~ NOTE1
F04 ~~~ MAT

%% ============ 凡例 ============
subgraph LEGEND["凡例"]
  direction TB
  subgraph LG1[" "]
    direction LR
    E1[" "] --> E2["データの流れ（オンライン）"]
  end
  subgraph LG2[" "]
    direction LR
    E3[" "] -.-> E4["データの流れ（バッチ）"]
  end
  subgraph LG3[" "]
    direction LR
    E5[" "] ==> E6["処理の流れ"]
  end
  subgraph LG4[" "]
    direction LR
    S1["機能"]
    S2[("情報<br/>（DB含む）")]
    S3[/"情報<br/>（ファイル）"/]
    S4>"注記"]
  end
  S5["＝ システム／サブシステム<br/>＝ 機能の枠"]
end

%% ============ スタイル ============
style TMK fill:#fff8dc,stroke:#333,stroke-width:2px
style TMK1 fill:#e0f0e0,stroke:#333
style TMK2 fill:#ffe9b0,stroke:#333
style SRM fill:#dceeff,stroke:#333,stroke-width:2px
style OPT fill:#ffffff,stroke:#333
style CALL fill:#ffffff,stroke:#333
style BUY fill:#ffffff,stroke:#333
style F04 fill:#d4f7d4,stroke:#333,stroke-width:2px
style F06 fill:#d4f7d4,stroke:#333,stroke-width:2px
style F07 fill:#d4f7d4,stroke:#333,stroke-width:2px
style MAT fill:#ffffff,stroke:#ff0000,stroke-width:2px,color:#ff0000
style NOTE1 fill:#ffffcc,stroke:#999
style LEGEND fill:#f5f5f5,stroke:#999
```

## 補足（参考図との対応）

| 参考図（No19/No22） | 本図での表現 |
|---|---|
| TMK資材：発注処理 → 連携サーバ：発注情報 | 機能「発注処理」→ 情報「発注情報（ファイル）」 |
| MeisterSRM：自動起票連携 → 発注情報 → 受付確認・かんばん発行一覧 | 機能「自動起票連携（参考・既存）」→ 情報「発注情報（DB）」→ 機能「受付確認・かんばん発行一覧」 |
| 受領・検収（QR読取）→ ステータス情報 → ステータス情報連携 | 機能「受領・検収」→ 情報「ステータス情報（検収QRデータ・ファイル）」→ 機能「ステータス情報連携バッチ（新規）」（バッチ＝破線） |
| 検収ファイルのQR項目 | データフローのラベルに記載（発注書番号・納品書番号・発注年月日・検収年月日・未検収区分・受入数量） |
| 資材発注書・受領書発行一覧／一括印刷 | 担当機能「資材発注書・受領書発行画面（新規）」「一括印刷」 |

## 担当機能（図中の緑色枠）

1. **受付確認・かんばん発行 ステータス情報連携バッチ**（連携オプション／新規）
   - 情報取得処理、データ登録・更新処理、DB新規
2. **資材発注書・受領書発行画面（サプライヤ）**
   - 画面の新規、検索条件を追加、一覧項目を追加、検索処理を追加
3. **資材発注書・受領書発行 一括印刷**
   - テンプレート作成処理、選択した案件を印刷チェック状態にする処理、データ取得処理、EXCEL出力処理、ZIP圧縮処理、ダウンロード処理、一括印刷処理
