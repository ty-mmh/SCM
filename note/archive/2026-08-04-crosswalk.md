# 2026-08-04 空白スナップショット対応表

- 状態: non-normative archive index
- 入力: 統合前の`note/01`〜`note/03`、ユーザー作成の「SCM 空白事項・延期事項・未実証事項の統合整理」、Claude全量レビュー
- 出力: [`../ledger.md`](../ledger.md)

この表は、全量スナップショットの論点が新しい台帳のどこへ移ったかを示す。詳細な設問本文は、統合前コミットおよび元スナップショットを参照する。

## 概念核

| 元スナップショット | 新台帳 | 扱い |
|---|---|---|
| I-1 系の成立時点・最初のNarrative | U-01 | ユーザー判断カードへ圧縮 |
| I-2 根底原則の正典集合 | U-02 | 執行点の不足も統合 |
| I-3 可変原則を変更する系の決定 | U-06、D-01 | v0.1利用範囲は判断、改訂アルゴリズムは延期 |
| I-4 Narrative内容契約 | U-05、E-02 | 最小意味はユーザー、運用詳細はengineering |
| I-5 帰属・保存参照・再構成可能性 | D-03 | Physical／適応型個体の前まで延期 |
| I-6 モデル固有傾向 | D-02 | DDIS接続候補として延期 |
| I-7 Narrative喪失後の連続性表現 | E-05、D-05 | 状態表現はLifecycle、検出・復旧は延期 |
| 追加: 系分岐の意味論 | E-05 | Lifecycleの実装時に扱う |
| 追加: 根底原則ごとの執行点 | U-02 | 正典集合と同時に決める |

## Narrative Runtime

| 元スナップショット | 新台帳 | 扱い |
|---|---|---|
| II-8 日次Narrative境界 | E-02 | engineering |
| II-9 Narrative入力閉包 | E-02 | U-05の意味契約後 |
| II-10 計算実行契約 | E-02 | engineering |
| II-11 原子性・冪等性 | E-02 | engineering |
| II-12 書き込み許可リスト | E-02 | engineering |
| 追加: Narrative保持・アーカイブ | E-02 | 非再生成可能性を保つ運用契約 |

## MemoryEntry・Event

| 元スナップショット | 新台帳 | 扱い |
|---|---|---|
| III-13 MemoryEntry粒度 | U-03 | ユーザー判断 |
| III-14 結果Event対応 | U-03、E-01 | 意味はユーザー、参照実装はengineering |
| III-15 個体交代pending閉包 | U-03 | ユーザー判断 |
| III-16 非行為帰属 | U-03 | ユーザー判断 |
| III-17 invalidated正規経路 | E-01 | engineering。存在論判断が出れば昇格 |
| III-18 contradiction登録 | E-01 | engineering |
| III-19 reserved物理スキーマ | E-01 | 文書予約と物理列を分離 |
| 追加: Event順序・時刻・冪等性 | E-01 | 上流のEvent契約 |
| 追加: MemoryEntry状態の永続化方式 | E-01 | 可変行かEvent Projectionかを決める |

## Recall・表現

| 元スナップショット | 新台帳 | 扱い |
|---|---|---|
| IV-20 Recall選択規則 | E-03 | engineering |
| IV-21 出力経験帰属契約 | U-04 | ユーザー判断 |

## Handoff・Task・共有状態・Lifecycle

| 元スナップショット | 新台帳 | 扱い |
|---|---|---|
| V-22 Handoff資料選択 | E-04 | engineering |
| V-23 Handoff鮮度 | E-04 | engineering |
| V-24 SharedState実装・version | E-04 | engineering |
| V-25 受領・評価・受諾・束縛・活性化 | E-04 | engineering |
| V-26 Handoff端ケース | E-04 | engineering |
| V-27 Taskなしモデル切替 | E-05 | C1 variant候補 |
| V-28 System／Instance lifecycle | E-05 | engineering |
| VI-29 Task状態機械 | E-04 | engineering |
| 追加: 系分岐 | E-05 | Lifecycleに統合 |
| 追加: Narrative喪失の認知後 | E-05 | Lifecycleに統合 |

## Governance・機械修正

| 元スナップショット | 新台帳 | 扱い |
|---|---|---|
| VII-30 DesignClaimレジストリ | G-01 | joint-review |
| VII-31 P番号衝突 | M-01 | mechanical |
| VII-32 適合主張の正規遷移境界 | M-03 | mechanical |
| VII-33 self-attested表示 | M-02 | mechanical |
| VII-34 論文証拠メタデータ | M-04 | mechanical |
| VII-35 実装・CI・レビュー・ライセンス | V-01〜V-04 | 未実証台帳へ移動 |

## 延期・範囲外・未実証

| 元スナップショット | 新台帳 | 扱い |
|---|---|---|
| VIII-36 記憶・Narrative・原則の延期 | D-01、D-02、D-05 | 領域別に分離 |
| VIII-37 将来プロファイル | D-04 | deferred |
| VIII-38 保証・耐障害性 | D-05 | deferred |
| VIII-39 終了後削除 | D-06 | out_of_scope |
| IX-40 ケース未実行 | V-02 | unvalidated |
| IX-41 権限・不変条件未実装 | V-03 | unvalidated |
| IX-42 帰属・状態遷移未実装 | V-01、V-03 | 未決仕様と未実装を分離 |
| IX-43 CI・独立性欠如 | V-04 | unvalidated |

## 統合時に行った分類修正

- 「概念領域」「決定状態」「実装状態」「証拠状態」を一つの分類列へ混ぜない。
- 仕様未決と、仕様決定済み・未実装を分ける。
- `deferred`と`out_of_scope`を分ける。
- `C1-no-active-task`は未実証ではなく、E-05に属する未定義variantとする。
- 同一論点の本文を複数文書へ重複保持せず、台帳IDを正本とする。
