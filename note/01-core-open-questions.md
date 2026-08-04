# 概念核に残る空白

- 状態: non-normative open questions
- 対象: SCMの存在論・系アイデンティティ・正典Narrative・状態分類
- 最終更新: 2026-08-04

## CORE-01 — 系の成立時点と最初のNarrative

- 状態: `open`
- 対象: System bootstrap / Narrative N0

### 既に決まっていること

- 系アイデンティティは、変更不能な根底原則と一つの正典Narrative系列によって支えられる。
- Narrative Revisionは系の専用処理が生成・コミットし、コミット時に固定される。
- Narrative喪失後の再生成は、同じ系の継続ではない。

### 空白

- `system_created`の時点で系は成立するか。
- 最初のNarrative Revisionがコミットされた時点で成立するか。
- Narrativeがまだない期間を、系の前段階として認めるか。
- 最初のRevisionの材料は何か。
- 初回だけ日次処理を待たずに生成するか。
- 初回Narrative生成に失敗した場合の状態は何か。

### 決定が必要になる時点

System bootstrapと最初の参照実装を作る前。

### 影響先

- `docs/01-conceptual-design.md`
- `docs/02-reference-profile-v0.1.md`
- `docs/03-invariant-coverage.md`
- `docs/07-continuity-ontology.md`

---

## CORE-02 — 根底原則の正典集合

- 状態: `open`
- 対象: immutable foundational principles

### 既に決まっていること

- 根底原則は変更不能であり、SCMの正規遷移として変更経路を持たない。
- 根底原則が異なる構成は別系として扱う。
- 現在の例には、経験帰属、Narrativeの非書換性、ユーザー物語の非所有、目的を持たない系がある。

### 空白

- 全SCM系に共通する根底原則の全量。
- 系固有の変更不能原則を認めるか。
- I1〜I8と根底原則の対応。
- 局所拒否権・故障局所化を根底原則とするか、運用要件とするか。
- 根底原則の初期値をどこから与えるか。
- 根底原則の保存表現と正典文書の関係。

### 決定が必要になる時点

System bootstrap、適合判定、分岐判定を実装する前。

### 影響先

- `docs/01-conceptual-design.md`
- `docs/02-reference-profile-v0.1.md`
- `docs/03-invariant-coverage.md`
- `docs/07-continuity-ontology.md`

---

## CORE-03 — 正典Narrative Revisionの内容契約

- 状態: `open`
- 対象: canonical Narrative schema / semantic contract

### 既に決まっていること

- 一つの系は一つの正典Narrative系列を持つ。
- Narrative上の解釈・判断は系へ帰属する。
- 計算を実行したモデル・プロセスは実行来歴として別に残す。
- 過去Revisionは上書きしない。
- v0.1ではNarrativeをRecall・生成コンテキスト・行為判断へ使わない。

### 空白

- Revisionが必ず含む要素。
- 前Revisionからの変化を明示するか。
- 主要Event、MemoryEntry、pending MemoryEntry、Task、矛盾、個体切替をどこまで含めるか。
- 自由文だけか、構造化部と自由文を併用するか。
- 各解釈へEvent／MemoryEntry参照を要求するか。
- 確定・未確定・矛盾の表現形式。
- Narrativeの長さと圧縮規則。

### 決定が必要になる時点

Narrative Processの入出力型を実装する前。

### 影響先

- `docs/02-reference-profile-v0.1.md`
- `docs/03-invariant-coverage.md`
- `docs/07-continuity-ontology.md`

---

## CORE-04 — 帰属・保存参照・同一再構成可能性の用語体系

- 状態: `open`
- 対象: state classification vocabulary

### 既に決まっていること

ある状態について、少なくとも次を別軸として扱う。

1. 誰に帰属するか。
2. SCMまたは外部から保存・参照できるか。
3. 喪失後に同一のものとして再構成できるか。

### 空白

- 三軸の正式名称。
- データモデル上の属性名。
- 「個体状態」「系状態」という既存語をどの軸へ割り当てるか。
- 外部保存された個体帰属状態の表現。
- 同一再構成不能だが参照可能な状態の分類。

### 決定が必要になる時点

Physical AI、適応型個体、外部状態参照を型へ落とす前。

### 影響先

- `docs/01-conceptual-design.md`
- `docs/06-deferred-profiles.md`
- `docs/07-continuity-ontology.md`

---

## CORE-05 — モデル固有傾向への一般化条件

- 状態: `deferred`
- 対象: model-level tendency inference

### 既に決まっていること

- SCMはInstanceごとのEvent、MemoryEntry、`model_ref`、状況、結果を保持できる。
- 一個体・一回の出来事を、モデル全体の傾向へ即座に一般化しない。
- SCM単独では一般化規則を持たない。
- DDISとの接続で扱える可能性がある。

### 空白

- 必要なInstance数・反復数。
- 「異なる状況」の判定。
- 同一モデルの範囲と版・設定差。
- 同一兆候の判定方法。
- SCMと外部評価機構の責務境界。

### 決定が必要になる時点

モデル比較・傾向評価をSCMの運用へ接続するとき。

### 影響先

- `docs/06-deferred-profiles.md`
- 将来のDDIS接続文書
