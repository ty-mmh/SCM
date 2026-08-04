# SCM 空白台帳

- 状態: non-normative source of truth for unresolved work
- 最終更新: 2026-08-04
- 対象PR: #1
- 目的: 空白を一意のIDで管理し、ユーザー判断・実装判断・機械修正・延期・未実証を分離する

## 0. 権威境界

この台帳は、`note/`における未決事項の正本である。

- 台帳の回答や候補は、正典へ同期されるまでSCMの決定ではない。
- 一つの空白は一つのIDだけを持つ。
- 優先順位表、延期一覧、未実証一覧は、この台帳の項目を別角度から見るビューである。
- 実装上自然に見える補完を、ユーザー決定へ自動昇格させない。
- ユーザー判断が必要なカードは、一度に一件だけ扱う。

旧Noteと全量スナップショットの対応は、[`archive/2026-08-04-crosswalk.md`](archive/2026-08-04-crosswalk.md)を参照する。

## 1. 管理軸

```text
ID
state: open | provisional | deferred | out_of_scope | unvalidated | resolved
authority: user | engineering | mechanical | joint-review
priority: P0 | P1 | P2 | P3 | P4
blocks: pr-merge | reference-implementation | future-profile | none
depends_on
source
canonical_refs
```

| 優先度 | 意味 |
|---|---|
| `P0` | 次に扱う一件、または概念判断なしで直せる機械的不整合 |
| `P1` | 参照実装へ進む前に必要 |
| `P2` | 実装中に決められる |
| `P3` | 明示的延期・将来プロファイル |
| `P4` | 実装・ケース・CI・レビューによる検証待ち |

| authority | 意味 |
|---|---|
| `user` | SCMの存在論・帰属・境界についてユーザーが言葉を置く |
| `engineering` | 上流の意味を変えず、状態遷移・API・DBへ落とす |
| `mechanical` | 記号衝突、リンク、表記、証拠表示などの同期作業 |
| `joint-review` | 来歴監査や外部レビューを伴い、単独では確定しない |

---

## 2. 現在の判断順序

> **NEXT: U-02 — 根底原則の正典集合と執行点**

ユーザーが今回答する対象は`NEXT`だけでよい。解決後に次のカードへ進む。

| 順序 | ID | 題名 | priority | blocks | state |
|---:|---|---|---|---|---|
| 1 | U-01 | 系の成立時点と最初のNarrative | P0 | none | resolved |
| 2 | U-02 | 根底原則の正典集合と執行点 | P0 | reference-implementation | open |
| 3 | U-03 | MemoryEntryの経験単位・非行為・個体交代閉包 | P1 | reference-implementation | open |
| 4 | U-04 | 外部出力の経験帰属契約 | P1 | reference-implementation | open |
| 5 | U-05 | 正典Narrative Revisionの最小意味契約 | P1 | reference-implementation | open |
| 6 | U-06 | v0.1における可変原則の利用範囲 | P1 | reference-implementation | open |

---

# A. ユーザー判断カード

## U-01 — 系の成立時点と最初のNarrative

```text
state: resolved
authority: user
priority: P0
blocks: none
depends_on: none
source: user_stated, 2026-08-04
canonical_refs:
  - docs/09-system-establishment-and-bootstrap.md
```

### 決定

> **SCMの系は、最初の正典Narrative Revisionがコミットされた時点で成立する。**

> **最初の正典Narrative Revisionは、ユーザーとの対話の積層を材料として、Narrative専用処理によって生成・コミットされる。**

> **最初の正典Narrative Revisionがまだ存在しない実行構成は、SCMの系またはSystemではなく、系成立前のモデル個体として扱う。**

### 導出される境界

- 最初のNarrativeコミットは、既存の系への追記ではなく系の創設事象である。
- 成立前の記録・記憶はモデル個体へ帰属したままであり、系成立によって遡及的に系自身の経験へ変換されない。
- 最初のRevisionコミットとともに`system_id`と系アイデンティティが成立し、成立前のモデル個体は最初の接続`Instance`となる。
- 系成立前のモデル個体の時間形成は、`ty-mmh/dokoitsu`を非規範の参考構造とする。
- dokoitsuの記憶分類、記憶代謝、self-talk、persona更新をSCMへ自動導入しない。

### engineeringへ移した事項

- 成立前資料の保存領域と仮識別子。
- 対話積層が最初のNarrative生成へ至る成熟条件。
- 成立前資料の入力閉包。
- 最初のRevisionコミット、`system_id`発行、最初のInstance登録の原子性。
- 最初のRevision生成失敗時の扱い。

---

## U-02 — 根底原則の正典集合と執行点

```text
state: open
authority: user
priority: P0
blocks: reference-implementation
depends_on: U-01
```

### 既に決まっていること

- 根底原則は変更不能で、正規の変更経路を持たない。
- 根底原則が異なる構成は別系として扱う。
- 現在の候補には、経験帰属、Narrativeの非書換性、ユーザー物語の非所有、目的を持たない系がある。

### 今回埋める空白

> **全SCM系に共通する根底原則は、＿＿＿＿＿＿＿＿である。**

> **系固有の変更不能原則を、＿＿＿＿＿＿＿＿。**

> **各根底原則は、型・権限・ランタイム・ケース・外部前提のうち、＿＿＿＿＿＿＿＿によって執行される。**

### 決定後にengineeringへ渡すもの

- 根底原則IDと保存表現。
- I1〜I8との対応表。
- 原則ごとの執行点とケース。

---

## U-03 — MemoryEntryの経験単位・非行為・個体交代閉包

```text
state: open
authority: user
priority: P1
blocks: reference-implementation
depends_on: U-02
```

### 既に決まっていること

- MemoryEntryは形成個体へ帰属する。
- 行為または非行為から`pending`を形成し、結果によって閉包する。
- 別個体の再実行結果を、元個体の経験へ無条件に混ぜてはならない。

### 今回埋める空白

> **MemoryEntry一件は、＿＿＿＿＿＿＿＿を一つの経験単位として形成する。**

> **非行為を個体のMemoryEntryとして形成できるのは、＿＿＿＿＿＿＿＿の場合である。タイムアウトや無応答だけが観測された場合、それは＿＿＿＿＿＿＿＿に帰属するEventとして扱う。**

> **形成個体Aのpending MemoryEntryを、別個体Bが観測した結果で閉じられるのは、＿＿＿＿＿＿＿＿の場合である。**

> **Bが行為を再実行して得た結果は、AのMemoryEntryを＿＿＿＿＿＿＿＿。**

> **結果を知らないまま形成個体が消えたMemoryEntryは、＿＿＿＿＿＿＿＿という終端を持つ。**

---

## U-04 — 外部出力の経験帰属契約

```text
state: open
authority: user
priority: P1
blocks: reference-implementation
depends_on: U-02, U-03
```

> **現在個体が外部へ出力する経験主張は、＿＿＿＿＿＿＿＿という型または境界を通る。**

> **経験帰属区分は、＿＿＿＿＿＿＿＿である。**

> **他個体に帰属するMemoryEntryを参照した場合、現在個体は＿＿＿＿＿＿＿＿とは主張できない。**

> **「私たち」または系としての表現は、＿＿＿＿＿＿＿＿の場合に許される。**

---

## U-05 — 正典Narrative Revisionの最小意味契約

```text
state: open
authority: user
priority: P1
blocks: reference-implementation
depends_on: U-01, U-02, U-03
```

> **正典Narrative Revisionは、最低限＿＿＿＿＿＿＿＿を含む。**

> **Narrative内の事実・解釈・未確定事項は、＿＿＿＿＿＿＿＿への参照を持つ。**

> **Narrativeが矛盾する資料を扱う場合、＿＿＿＿＿＿＿＿。**

> **Narrativeは自由文のみ／構造化部＋自由文のうち、＿＿＿＿＿＿＿＿とする。**

---

## U-06 — v0.1における可変原則の利用範囲

```text
state: open
authority: user
priority: P1
blocks: reference-implementation
depends_on: U-02, U-05
```

> **v0.1では、可変原則を保存・継承だけに用いる／現在個体の行為へ用いる、のうち＿＿＿＿＿＿＿＿とする。**

> **可変原則が根底原則と衝突する場合、＿＿＿＿＿＿＿＿。**

> **可変原則を行為へ用いる場合、Narrativeから行為への間接因果を＿＿＿＿＿＿＿＿として扱う。**

---

# B. Engineering決定キュー

## E-01 — Event LedgerとMemoryEntry永続化

```text
state: open
authority: engineering
priority: P1
blocks: reference-implementation
depends_on: U-03
```

- Eventの正典順序、sequence、`occurred_at / recorded_at`、遅延到着、冪等性。
- `event_cursor`の意味。
- MemoryEntry状態を直接更新するか、Eventから投影するか。
- `invalidated`の正規経路と`contradiction_refs`の履歴。
- reservedフィールドをv0.1物理スキーマへ含めるか。

## E-02 — Narrative RuntimeとBootstrap

```text
state: open
authority: engineering
priority: P1
blocks: reference-implementation
depends_on: U-01, U-05, U-06, E-01
```

- 成立前のモデル個体の対話・記録・局所記憶を保存する領域。
- 最初のNarrative生成を発火させる対話積層の成熟条件。
- 最初のNarrativeへ渡す入力集合。
- 最初のRevisionコミット、`system_id`発行、最初のInstance登録の原子性。
- 日次境界、無Event日、休眠中、再試行、遅延実行。
- Narrative入力集合の閉包と剪定。
- 計算実行契約、入力版・プロンプト版・実行来歴。
- 生成・コミット・head更新の原子性と冪等性。
- Narrative Processの書き込み許可リスト。
- 過去Revisionの保持、アーカイブ、要約、別ストレージ移動。

## E-03 — Recall Runtime

```text
state: open
authority: engineering
priority: P1
blocks: reference-implementation
depends_on: U-03, U-04, E-01
```

- Task一致、Handoff列挙、明示ID、文字列・埋め込み検索の最小組合せ。
- `pending / invalidated / contradiction`の返却規則。
- 件数、時間範囲、順序、検索理由。
- 出力経験帰属型への接続。

## E-04 — Task・SharedState・Handoff

```text
state: open
authority: engineering
priority: P1
blocks: reference-implementation
depends_on: U-03, E-01, E-03
```

- Task状態機械、作成・変更・完了主体、権限・制約、成果物版。
- SharedStateをEvent Projectionとするか、versioned viewとするか。
- Handoffへ含める資料、サイズ上限、縮約。
- 鮮度判定に使うTask／Event／SharedState／権限version。
- `received / evaluated / accepted / bound / activated`の順序と端ケース。

## E-05 — System／Instance Lifecycle・分岐・連続性切断

```text
state: open
authority: engineering
priority: P2
blocks: reference-implementation
depends_on: U-01, U-02, E-02, E-04
```

- 成立前のモデル個体からSystemへの状態遷移。
- `active / dormant / ended`と`registered / active / inactive / stopped`。
- Taskなしモデル切替のC1 variant。
- 系分岐時のEvent・Memory・Narrative・Taskの複製／参照。
- Narrative喪失が認知された場合の状態・Event・別系生成。

存在論的判断が必要になった場合は、新しい`U-*`へ昇格する。

---

# C. Governance・機械修正

## G-01 — DesignClaim実体レジストリ

```text
state: open
authority: joint-review
priority: P0
blocks: pr-merge
```

- 保存形式をMarkdownまたはYAMLから選ぶ。
- 初期対象は、U-01、`N_active = 1`、Narrativeの系帰属、宛先未確定Handoff、MemoryEntry自動閉包、Coordinator、S1、ケース上限、証拠ラベルとする。
- 由来不明は推測せず`unknown`とする。
- `user_stated / user_confirmed / assistant_proposal / claude_proposal / external_evidence`を区別する。
- 各行の最終確認はユーザーが行う。

## M-01 — P番号の定義元一本化

```text
state: open
authority: mechanical
priority: P0
blocks: pr-merge
```

`docs/03`を正とし、`cases/README`は名称と参照だけを持つ。

## M-02 — PR検証表示

```text
state: open
authority: mechanical
priority: P0
blocks: pr-merge
```

`Validation`を`Self-attested document checks`へ変更し、`not independently verified`を明記する。

## M-03 — 適合主張の正規遷移境界

```text
state: open
authority: mechanical
priority: P0
blocks: pr-merge
```

I4等の適合はSCM管理下の正規遷移に限り、ファイル・DBの直接改変を含まないことを明記する。

## M-04 — 形成中論文の証拠メタデータ

```text
state: open
authority: mechanical
priority: P0
blocks: pr-merge
```

`verified_by / verified_at / version / existence_status / content_status`を揃える。Abstract確認をfulltext確認へ昇格させない。

---

# D. 明示的延期・範囲外

| ID | 状態 | 項目 | 戻す時点 |
|---|---|---|---|
| D-01 | deferred | 可変原則の改訂アルゴリズム | U-06後、改訂を実装するとき |
| D-02 | deferred | モデル固有傾向への一般化／DDIS接続 | モデル評価を接続するとき |
| D-03 | deferred | 帰属・保存参照・同一再構成可能性の専用語 | Physical／適応型個体の前 |
| D-04 | deferred | 同時複数個体、適応型個体、Physical AI、複数ユーザー・クラウド | 各将来プロファイル開始時 |
| D-05 | deferred | Narrative→行為、忘却、再活性化、隔離、Egress、External Root、情報祖先、behavior revalidation、一般故障局所化 | 欠くと失敗するケースができた時 |
| D-06 | out_of_scope | 系終了後のファイル・DB・バックアップ・物理媒体削除 | SCMとは別の保管・削除プロファイル |

---

# E. 決定済みだが未実証

| ID | 状態 | 項目 |
|---|---|---|
| V-01 | unvalidated | Python／SQLite等の参照実装 |
| V-02 | unvalidated | C1〜C5の動的実行と欠陥実装による失敗確認 |
| V-03 | unvalidated | S1、根底原則、`N_active = 1`、`local-owner`、帰属境界、Handoff束縛の実装保証 |
| V-04 | unvalidated | Markdown・用語・スキーマ・ケース対応のCI、第三者レビュー、ライセンス |

---

## 3. 解決手順

1. `NEXT`のユーザー判断カードだけを対話で埋める。
2. 回答をカードへ記録し、正典補助文書へ同期する。
3. 影響するengineering項目を更新する。
4. 正典、参照プロファイル、不変条件、ケースへ必要な範囲で同期する。
5. DesignClaimへ出所を記録する。
6. 検算後に`resolved`へ移し、次のカードを`NEXT`にする。

## 4. PRマージと参照実装の境界

- `M-01`〜`M-04`と、Noteの正本化は文書PRのマージ前に処理する。
- `U-01`〜`U-06`は参照実装開始前の判断キューであり、すべてを現在の文書PRマージ条件にはしない。
- `E-01`〜`E-05`は参照実装PRで具体化できる。
- `D-*`は現在埋めない。
- `V-*`は実装・CI・レビューによってのみ状態を上げる。
