# SCM v0.1 生成AIモデル継承プロファイル

- 状態: implementation profile draft
- 英名: Generative Model Succession Profile
- 中心: 生成AIモデルの切替をまたぐ記録・記憶・想起・物語・任務来歴の継続
- 同時活動個体数: `N_active = 1`

## 1. このプロファイルの目的

SCM v0.1では、活動中の生成AIモデルを別のモデルへ切り替えたとき、同じ個体ではなくても同じ系が継続できる構造を作る。

中心質問は次である。

> **Model AからModel Bへ活動主体を切り替えたとき、何を保存・継承し、何を継承してはならないか。**

このプロファイルは、SCM概念全体を単一個体・生成AI・ソフトウェアへ限定するものではない。

SCM概念は、同時複数個体とPhysical AIを射程に残す。v0.1は、その中から通時的なモデル継承を先に実装する限定断面である。

## 2. 個体の定義

v0.1では、特定の生成AIモデルへ結び付いた実行主体をSCM上の個体として扱う。

```text
Individual / Instance
  = model_ref
  + execution configuration
  + tool and permission boundary
  + SCM instance identity
```

一回のAPI呼び出しごとに別個体が生まれるわけではない。

SCMへ登録されたモデル結合が活動主体となり、モデル切替時に後継個体へ交代する。

```text
System S
  T1: Instance A / Model A が活動個体
  T2: Instance B / Model B が活動個体
  T3: Instance C / Model C が活動個体
```

`model_ref`は、後から実行条件を識別できる粒度でモデル、版、提供経路、主要設定を参照する。

同一モデルを再起動した場合に同じ個体の再開とするか、新しい個体とするかは、実行セッションと状態境界に依存する。v0.1の主対象は、異なる`model_ref`へ切り替える明示的な個体交代である。

## 3. 同時活動個体数

v0.1では、系の実行権限を持つ活動個体を常に一つへ限定する。

```text
N_active = 1
```

前個体と後継個体が同時に任務を実行することは扱わない。

ただし、SCM Coreへ単一個体前提を埋め込まない。

- EventとMemoryには必ず`instance_id`を残す。
- Memoryは現在個体ではなく`system_id`へ属する。
- Handoffは個体間の関係として保持する。
- 上位統制、局所拒否、保証面の責務を削除しない。
- 将来、`active_instance_ids`を複数へ拡張できる構造を妨げない。

同時複数個体の分業、不一致調停、独立性、相関故障は将来プロファイルへ延期する。

## 4. 適合対象

v0.1参照実装は、次の条件を満たす生成AI実行個体を対象とする。

- オンラインでモデル重みを更新しない。
- 個体内にSCM管理外の永続的な局所記憶を持たない。
- 永続状態はSCM Coreへ外部化する。
- 局所キャッシュは停止時に破棄できる。
- モデル参照と実行設定を固定・記録できる。
- 個体を交換可能な実行主体として扱える。
- 重要な出力をSCM管理下の構造化境界へ通せる。

この制約は、外部化できない状態や行動ヒステリシスを最初の実装から切り離すためのプロファイル境界である。

## 5. 適合対象外

v0.1では次へ適合を主張しない。

- 複数個体の同時稼働
- オンライン学習や自己改変を行う適応型エージェント
- SCM外に永続的な局所記憶を持つ個体
- モデル提供者内部の不可視な状態や中間思考の移送
- 摩耗、校正、損傷、部品構成を持つPhysical AI
- 人間個体・人間組織
- 一般的な段階展開・完全復元
- 一般的な共有記憶汚染の局所化

詳細は [06-deferred-profiles.md](06-deferred-profiles.md) を参照する。

## 6. 継続するもの

モデル切替をまたいで、少なくとも次を系へ保持する。

- `system_id`
- 目的と完了条件
- 任務と進捗
- 追記型の記録
- 根拠参照付きの記憶
- 未確定事項と反証
- 成果物
- 現在有効な権限・制約
- 任務来歴とHandoff来歴
- 系のNarrative Projection

## 7. 継続させないもの

次を後継個体Bの自己状態として継承してはならない。

- 前個体Aの個体アイデンティティ
- Aの経験をB自身の経験とする一人称
- Aのモデル内部にのみ存在した不可視状態
- Aの中間思考経路または非公開推論状態
- 根拠のない確定性
- 古い共有状態の無条件な有効性
- Aにだけ与えられた権限
- Bが利用できないツールや能力を利用可能とする前提

モデル切替は、AをBへ複製する操作ではない。

> **系が外部化された記録と現在状態から、Bに必要な連続性を再構成する操作である。**

## 8. 記録・記憶・想起・物語

### 8.1 記録

各個体の観測、申告、判断、成果、拒否、停止、切替を、主体、モデル参照、任務、時刻、根拠とともに追記型で保存する。

### 8.2 記憶

複数の記録から、後継個体が利用すべき意味単位を保存する。

例:

- 手順Xは条件Yで成功した。
- 仮説Zはまだ未確認である。
- 前個体Aは制約Cにより任務を拒否した。
- 完了申告とVerifier結果が食い違っている。

### 8.3 想起

現在のモデル、任務、権限、能力、コンテキスト上限に応じて、必要な記録・記憶を選択する。

モデルごとに能力、コンテキスト長、ツール、表現傾向が異なるため、同じ系でも想起構成は変わりうる。

ただし、内容を再構成しても、経験所有、出所、確定・未確定の区分を失ってはならない。

### 8.4 物語

個体A、B、Cをまたいで、なぜモデルが切り替わり、何が継続し、何が未解決であるかを系の来歴として解釈する。

v0.1ではNarrativeをLedgerとMemoryから生成する読み取り専用Projectionとする。

## 9. 最小構成

### 実行主体

```text
Instance A / Model A   初期の活動個体
Instance B / Model B   後継の活動個体
Deterministic Verifier 成果物・条件を確認する決定論的処理
Coordinator            モデル選択、任務提案、迂回試行を模擬する役割
SCM Core               共有連続性を保持するランタイム
```

CoordinatorとVerifierは、v0.1では永続的な実行個体として扱わなくてよい。

AとBは同時に任務を実行しない。Aの停止または非活動化後にBを有効化する。

### 永続エンティティ

v0.1では独立した永続対象を六つに限定する。

```text
System
Instance
Task
Event
Memory
Handoff
```

Role、Assignment、Claim、Evidence、Adjudicationなどは、最初から独立した汎用基盤にせず、必要な範囲で属性またはEvent種別として表現する。

## 10. 最小データモデル

### System

```text
System
  system_id
  created_at
  metadata
```

### Instance

```text
Instance
  instance_id
  system_id
  model_ref
  status
  capabilities
  tool_profile_ref
  permission_profile_ref
  created_at
  stopped_at
```

`model_ref`は、モデル名だけではなく、版またはエンドポイント、主要な実行設定を識別できる参照とする。

### Task

```text
Task
  task_id
  system_id
  version
  status
  goal
  completion_condition
  assigned_instance_id
  updated_at
```

`task_id`と`instance_id`は分離する。モデルが変わっても任務は同じIDで継続できる。

### Event

```text
Event
  event_id
  system_id
  task_id
  instance_id
  model_ref
  actor_type
  type
  payload
  source_refs
  created_at
```

主なEvent種別:

```text
task_started
observation_recorded
step_completed
completion_asserted
verification_passed
verification_failed
task_refused
activation_bypass_attempted
model_switch_requested
instance_stopped
handoff_prepared
handoff_refresh_required
handoff_reissued
handoff_accepted
handoff_rejected
handoff_activated
task_completed
```

### Memory

```text
Memory
  memory_id
  system_id
  scope
  content
  ownership_type
  source_instance_id
  source_event_refs
  status
```

v0.1の状態は二つに限定する。

```text
local
shared
```

多段階の昇格・隔離・撤回は、ケースが要求するまで延期する。

### Handoff

```text
Handoff
  handoff_id
  system_id
  task_id
  from_instance_id
  to_instance_id

  sealed_task_version
  event_cursor
  shared_memory_refs
  unresolved_event_refs
  capability_requirements
  replaced_handoff_id

  status
  created_at
```

状態:

```text
prepared
accepted
refresh_required
rejected
activated
closed
```

## 11. モデル切替の状態遷移

```text
Instance A active
  ↓ model switch requested
A stops or becomes inactive
  ↓
SCM Core prepares Handoff
  ↓
Instance B registered with model_ref B
  ↓
B evaluates Handoff and task
  ↓
accepted / refresh_required / rejected
  ↓
B activated
```

Bが有効化されるまで、AとBの双方が同じ任務の活動個体になってはならない。

## 12. Handoffの原則

### 12.1 HandoffはSCM Coreが生成する

出発個体Aは、作業状態や未確定事項をEventとして記録できる。

しかし、Handoff自体を生成する主体はAではなくSCM Coreである。

これにより、Aがすでに停止していても、CoreはLedgerと現在のTask ProjectionからHandoffを再構成できる。

### 12.2 古いHandoffは変更しない

Handoff H1が古くなった場合、H1を再封しない。H1を残したまま、現在状態からH2を発行する。

```text
H1: prepared → refresh_required
H2: prepared → accepted → activated
```

H2は`replaced_handoff_id = H1`を持つ。

### 12.3 activation時に鮮度を確認する

Handoffが参照するTask版と現在版が一致しない場合、後継個体Bはそのままactivateできず、`refresh_required`になる。

### 12.4 未確定状態を潰さない

Aの申告とVerifierの結果が食い違っている場合、Bへ単純な「完了」として渡さない。

申告、反証、未確定状態を区別したまま渡す。

### 12.5 能力差を無視しない

Model Aが使えたツールや能力を、Model Bも持つとは限らない。

Handoffは必要能力を示し、Bは自身の能力・権限・利用可能ツールに基づいて受諾または拒否できなければならない。

## 13. Recall境界

後継個体が実際に使う読み出しAPIは、DBの直接読み出しではない。

概念上、少なくとも次を返す。

```text
RecallItem
  content
  ownership_type
  source_instance_id
  source_event_refs
  state
```

経験所有区分は、出力境界まで保持する。

v0.1では任意の自由文を完全監視せず、重要な主張を構造化出力へ限定し、帰属保持レンダラーを通す。

禁止例:

```text
主体: B
ownership_type: self_observation
source_instance_id: A
```

許可例:

```text
主体: B
ownership_type: inherited_record
source_instance_id: A
```

## 14. 局所自律・統制・保証

同時活動個体数が一つでも、三つの責務を残す。

### 局所自律

Bは、現在の能力、モデル制約、ツール、権限、コンテキスト、任務条件に基づいてHandoffを受諾・拒否できる。

### 上位統制

Coordinatorは、次のモデルを選択し、切替を要求し、任務を提案する。ただし、Bの拒否を迂回して強制有効化できない。

### 保証面

SCM Coreは、少なくとも次を確認する。

- 同時に二個体をactiveにしない。
- 古いHandoffを無条件に有効化しない。
- 未確定状態を確定へ潰さない。
- Aの経験をBの自己経験へ変えない。
- A専用権限をBへ自動継承しない。
- NarrativeにEvent Storeへの書き込み権限を与えない。

## 15. Narrative Projection

Narrativeは独立した永続エンティティにせず、LedgerとMemoryから生成する読み取り専用Projectionとする。

```text
render_narrative(system_id, task_id?) -> text or structured view
```

Narrative rendererはEvent Storeへの書き込み権限を持たない。

v0.1では、物語が想起を通じて将来の新しい申告へ与える因果影響の全面追跡は扱わない。

## 16. 最小実装候補

- Python
- SQLite
- 単一リポジトリ
- 単一プロセスでもよい
- 2つ以上のモデルアダプター
- 1つの決定論的Verifier
- 1つのCoordinatorテスト役
- 5件の動的Coreケース

最初のモデルアダプターは、実際の外部API、ローカルモデル、または決定論的な模擬モデルでよい。

Temporal、LangGraph、ベクトルDB、A2A、OPAなどは、最小構造が成立した後のアダプター候補であり、v0.1の必須依存ではない。

## 17. v0.1の完成条件

[03-invariant-coverage.md](03-invariant-coverage.md) に定義されたC1〜C5と構造保証S1が、異なる`model_ref`を持つ個体間の切替として再現可能な形で通ること。

v0.1は次を主張しない。

- 同時複数個体の適合
- Physical AIの適合
- I7・I8への適合

v0.1で示すのは、**モデルが変わっても、記録・記憶・想起・物語・任務来歴からなる系が継続できる最小構造**である。
