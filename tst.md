> モデル：老师の「ファイルタイプ」図（tes.png）
> 老師：転送日時フォルダ（HINBAN_YYYYMMDDhhmmssSSS）に 配置完了ファイル＋品番情報データファイル
> 科：KENSHU_ プレフィックスの転送日時フォルダに 配置完了ファイル＋検収データファイル（2例＋……）

```mermaid
flowchart LR

  ROOT["ファイルタイプ"]

  %% ---- 1つ目の転送日時 ----
  ROOT --> F1["KENSHU_20261023152304001<br/>（転送日時）"]
  F1 --> C1["configurationSts.txt<br/>（配置完了ファイル）"]
  F1 --> D1["KENSHU_DATA.csv<br/>（検収データファイル）"]

  C1 -.- N1["-- 転送した日時<br/><br/>-- 処理対象の出力日付ディレクトリ以下の<br/>ファイルの配置が完了した場合に配置される。<br/>本ファイルが存在することを前提に、<br/>出力されている構成が正常と見なす。<br/>・ファイル名：configurationSts.txt<br/>　（名称は変更不可）<br/><br/>［内容例］<br/>OK"]

  D1 -.- M1["-- 検収（ステータス）を行うデータファイル<br/>・ファイル名を以下とする<br/>　KENSHU_DATA.csv<br/>・データを「|」区切りで連結する。<br/>　※詳細なファイルの構成について、<br/>　「別紙」シートをご参照ください。<br/>・必須データは必ず指定すること。"]

  %% ---- 2つ目の転送日時 ----
  ROOT --> F2["KENSHU_20261023152507001<br/>（転送日時）"]
  F2 --> C2["configurationSts.txt<br/>（配置完了ファイル）"]
  F2 --> D2["KENSHU_DATA.csv<br/>（検収データファイル）"]

  C2 -.- N2["（注記は上記と同じ）<br/><br/>［内容例］<br/>OK"]
  D2 -.- M2["（注記は上記と同じ）"]

  %% ---- 継続 ----
  ROOT --> ELL["……"]

  %% ---- 凡例 ----
  subgraph LEG["凡例"]
    direction LR
    L1["フォルダ"]
    L2["ファイル"]
  end

  %% ---- スタイル ----
  style ROOT fill:#ffffff,stroke:#000000
  style F1 fill:#fff2cc,stroke:#000000
  style F2 fill:#fff2cc,stroke:#000000
  style C1 fill:#ffffff,stroke:#000000
  style D1 fill:#ffffff,stroke:#000000
  style C2 fill:#ffffff,stroke:#000000
  style D2 fill:#ffffff,stroke:#000000
  style ELL fill:#ffffff,stroke:#ffffff
  style N1 fill:#ffffff,stroke:#ffffff,color:#000000
  style M1 fill:#ffffff,stroke:#ffffff,color:#000000
  style N2 fill:#ffffff,stroke:#ffffff,color:#000000
  style M2 fill:#ffffff,stroke:#ffffff,color:#000000
  style L1 fill:#fff2cc,stroke:#000000
  style L2 fill:#ffffff,stroke:#000000
```

---

## フォルダ内の構成（テキスト版）

```
KENSHU_20261023152304001/          ← 転送日時フォルダ
├── configurationSts.txt           ← 配置完了ファイル（内容："OK"／名称変更不可）
└── KENSHU_DATA.csv                ← 検収データファイル（「|」区切り）

KENSHU_20261023152507001/          ← 2つ目以降も同構成
├── configurationSts.txt
└── KENSHU_DATA.csv
……
```

---

## メモ

- フォルダ名は「KENSHU_YYYYMMDDhhmmssSSS」（処理補足 3.① と整合）
- 配置完了ファイル configurationSts.txt は**名称変更不可**、内容が「OK」のもののみ処理処理対象（処理補足 3.② と整合）
- データファイルの詳細な項目構成は「別紙：ファイルの構成」シート参照（8項目を「|」で結合）
- 老師の図は1つのフォルダごとに全注記を書き写しているが、2つ目以降は「注記は上記と同じ」に省略可（Excelに貼る際は老师に合わせて展開してもよい）
- KENSHU_ プレフィックス・configurationSts.txt の実名称はTMKとの合意結果で確定
