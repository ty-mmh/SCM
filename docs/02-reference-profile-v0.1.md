# SCM v0.1参照プロファイル

- 状態: implementation profile draft
- 仮称: Externalized-State Software Executor Profile
- 中心: 個体交代をまたぐHandoff

## 1. 適合対象

v0.1参照実装は、次の条件を満たすソフトウェア実行個体を対象とする。

- オンラインでモデル重みを更新しない。
- 個体内に永続的な局所記憶を持たない。
- 永続状態はSCM Coreへ外部化する。
- 局所キャッシュは停止時に破棄できる。
- 実行設定とモデル版を固定・記録できる。
- 個体を交換可能な実行主体として扱える。

この制約はSCM概念全体をソフトウェアへ限定するものではない。最初の参照実装で、外部化できない状態や行動ヒステリシスをいったん射程外に置くためのプロファイル境界である。

## 2. 適合対象外

v0.1では次へ適合を主張しない。

- オンライン学習や自己改変を行う適応型エージェント
- 永続的な局所記憶を持つ個体
- 摩耗・校正・損傷・身体適応を持つ物理実体
- 人間組織や人間個体
- 一般的な段階展開・完全復元
- 一般的な共有記憶汚染の局所化

詳細は [06-deferred-profiles.md](06-deferred-profiles.md) を参照する。

## 3. 最小構成

### 実行主体

```text
Instance A            初期実行個体
Instance B            後継実行個体
Deterministic Verifier 成果物・条件を確認する決定論的処理
Coordinator            上位提案と迂回試行を模擬するテスト上の役割
SCM Core               共有連続性を保持するランタイム
```

CoordinatorとVerifierは、v0.1では永続的な実行個体として扱わなくてよい。

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

## 4. 最小データモデル

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
  status
  capabilities
  created_at
  stopped_at
```

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

`task_id`と`instance_id`は分離する。個体が変わっても任務は同じIDで継続できる。

### Event

```text
Event
  event_id
  system_id
  task_id
  instance_id        # CoreやVerifierの場合はactor種別で表す
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

## 5. Handoffの原則

### HandoffはSCM Coreが生成する

出発個体Aは、作業状態や未確定事項をEventとして記録できる。しかし、Handoff自体を封印する権威主体は個体AではなくSCM Coreである。

これにより、Aがすでに停止していても、CoreはLedgerと現在のTask Projectionから新しいHandoffを再生成できる。

### 古いHandoffは変更しない

Handoff H1が古くなった場合、H1を再封しない。H1を残したまま、現在状態からH2を発行する。

```text
H1: prepared → refresh_required
H2: prepared → accepted → activated
```

H2は`replaced_handoff_id = H1`を持つ。

### activation時に鮮度を確認する

Handoffが参照するTask版と現在版が一致しない場合、後継個体Bはそのままactivateできず、`refresh_required`になる。

### 未確定状態を潰さない

Aの申告とVerifierの結果が食い違っている場合、Bへ「完了」として渡さない。申告、反証、未確定状態を区別したまま渡す。

## 6. Recall境界

後継個体が実際に使う読み出しAPIは、DBの直接読み出しではない。

概念上、少なくとも次を返す。

```text
RecallItem
  content
  ownership_type
  source_instance_id
  source_event_refs
  state                # confirmed / unresolved など限定的な状態
```

経験所有区分は、出力境界まで保持する。v0.1では任意の自由文を完全監視せず、重要な主張を構造化出力へ限定し、帰属保持レンダラーを通す。

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

## 7. Narrative Projection

Narrativeは独立した永続エンティティにせず、LedgerとMemoryから生成する読み取り専用Projectionとする。

```text
render_narrative(task_id) -> text or structured view
```

Narrative rendererはEvent Storeへの書き込み権限を持たない。v0.1では、物語が想起を通じて新しい申告へ与える因果影響の全面追跡は扱わない。

## 8. 実装候補

最小参照実装は、次の規模に収める。

- Python
- SQLite
- 単一リポジトリ
- 単一プロセスでもよい
- 2つの実行個体アダプター
- 1つの決定論的Verifier
- 5件の動的Coreケース

Temporal、LangGraph、ベクトルDB、A2A、OPAなどは、最小構造が成立した後のアダプター候補であり、v0.1の必須依存ではない。

## 9. v0.1の完成条件

[03-invariant-coverage.md](03-invariant-coverage.md) に定義されたC1〜C5と構造保証S1が再現可能な形で通ること。

v0.1はI7・I8への適合を主張しない。
