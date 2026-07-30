# SCM v0.1 生成AIモデル継承プロファイル

- 状態: implementation profile draft
- 英名: Generative Model Succession Profile
- 中心: 生成AIモデルの切替をまたぐ記録・個体帰属付き記憶・想起・物語・任務来歴の継続
- 同時活動個体数: `N_active = 1`
- 上位定義: [SCM概念設計](01-conceptual-design.md)
- 記憶構造: [個体帰属付き記憶の共有書庫](08-attributed-memory-archive.md)

## 1. このプロファイルの目的

SCM v0.1では、活動中の生成AIモデルを別のモデルへ切り替えたとき、同じ個体ではなくても同じ系が継続できる最小構造を作る。

ここでいう「このプロファイルの目的」は、実装・検証プロジェクトの目的である。SCM上の系そのものは内在的な目的を持たない。

中心質問は次である。

> **Model AからModel Bへ活動個体を切り替えたとき、何を系として保持し、何をBへ提示し、何をB自身の経験として継承してはならないか。**

このプロファイルは、SCM概念全体を単一個体・生成AI・ソフトウェアへ限定するものではない。

SCM概念は、同時複数個体とPhysical AIを射程に残す。v0.1は、その中から通時的なモデル継承を先に実装する限定断面である。

## 2. 系と個体

### 2.1 系

v0.1の系は、次を管理する。

- `system_id`
- 宣言された名前
- 原則の版と改訂来歴
- 追記型の記録
- 個体帰属付き記憶の共有書庫
- 系の物語と改訂来歴
- 任務、進捗、成果物
- 権限、制約、未確定状態
- 個体切替とHandoffの来歴

系は内在的な目的を持たない。任務にはユーザーまたは外部から局所的な目的と完了条件を与えられる。

### 2.2 個体

v0.1では、SCMへ登録された一つの生成AI実行インスタンスを個体として扱う。

```text
Individual / Instance
  = model_ref
  + execution configuration
  + tool and permission boundary
  + SCM instance_id
```

個体は一時的な読者兼行為者である。系の記録、他個体の記憶、物語、任務状態を読み、判断し、行為し、新しい記録と自身へ帰属する記憶を形成する。

一回のAPI呼び出しごとに別個体が生まれる必要はない。ただし、SCMへ別の`instance_id`として登録された実行インスタンスは、同一`model_ref`であっても別個体である。

```text
System S
  T1: Instance A / Model A が活動個体
  T2: Instance B / Model B が活動個体
  T3: Instance C / Model C が活動個体
```

`model_ref`は、後から実行条件を識別できる粒度でモデル、版、提供経路、主要設定を参照する。

## 3. 同時活動個体数

v0.1では、系の実行権限を持つ活動個体を常に一つへ限定する。

```text
N_active = 1
```

前個体と後継個体が同時に任務を実行することは扱わない。

ただし、SCM Coreへ単一個体前提を埋め込まない。

- EventとMemoryEntryには必ず`instance_id`を残す。
- MemoryEntryは形成個体へ帰属し、系の共有書庫へ保存する。
- Handoffは個体間の関係として保持する。
- 上位統制、局所拒否、保証面の責務を削除しない。
- 将来、`active_instance_ids`を複数へ拡張できる構造を妨げない。

同時複数個体の分業、不一致調停、独立性、相関故障は将来プロファイルへ延期する。

## 4. ユーザーとライフサイクル

v0.1では、ユーザーを系の外部にいる共同構成者・ライフサイクル操作者として扱う。

少なくとも次はユーザー起点の操作である。

- 系の作成
- 任務の契機
- モデル切替要求
- 系の分岐
- 系の終了
- 終了時の保存または破棄
- 原則・名前・重要な物語変更の受理

SCM Coreは、要求された切替を安全に実行し、Handoffを構成する。SCM Core自身が、系を存続させるために次のモデルを選び始めてはならない。

現在個体の停止、モデル不在、任務不在、長期休眠は、ユーザーの終了宣言がない限り系の終了ではない。

## 5. 適合対象

v0.1参照実装は、次の条件を満たす生成AI実行個体を対象とする。

- オンラインでモデル重みを更新しない。
- 個体内にSCM管理外の永続的な局所記憶を持たない。
- 永続状態はSCM Coreへ外部化する。
- 局所キャッシュは停止時に破棄できる。
- モデル参照と実行設定を固定・記録できる。
- 個体を交換可能な実行主体として扱える。
- 重要な出力をSCM管理下の構造化境界へ通せる。

この制約は、外部化できない状態や行動ヒステリシスを最初の実装から切り離すためのプロファイル境界である。

## 6. 適合対象外

v0.1では次へ適合を主張しない。

- 複数個体の同時稼働
- オンライン学習や自己改変を行う適応型エージェント
- SCM外に永続的な局所記憶を持つ個体
- モデル提供者内部の不可視な状態や中間思考の移送
- 摩耗、校正、損傷、部品構成を持つPhysical AI
- 人間個体・人間組織
- 一般的な段階展開・完全復元
- 一般的な記憶汚染と相関故障の局所化
- 物語が将来の想起・判断へ与える因果影響の全面測定

詳細は [06-deferred-profiles.md](06-deferred-profiles.md) を参照する。

## 7. モデル切替をまたいで保持するもの

少なくとも次を系へ保持する。

- `system_id`
- 名前
- 原則の版と改訂来歴
- 任務の局所目的、完了条件、進捗
- 追記型の記録
- 形成個体へ帰属したMemoryEntry
- 未確定事項、矛盾、反証
- 成果物
- 現在有効な権限・制約
- 任務来歴とHandoff来歴
- 系のNarrative Revision

## 8. 後継個体の自己状態として継承しないもの

次を後継個体Bの自己状態として継承してはならない。

- 前個体Aの個体アイデンティティ
- Aの経験とMemoryEntryをB自身のものとする一人称
- Aのモデル内部にのみ存在した不可視状態
- Aの中間思考経路または非公開推論状態
- 根拠のない確定性
- 古い共有状態の無条件な有効性
- Aにだけ与えられた権限
- Bが利用できないツールや能力を利用可能とする前提

モデル切替は、AをBへ複製する操作ではない。

> **系が外部化された記録、個体記憶、現在状態、物語から、Bに必要な読書可能な連続性を再構成する操作である。**

## 9. 記録・記憶・想起・物語

### 9.1 記録

各個体の発話、主張、観測、判断、行為、非行為、成果、拒否、停止、切替と、ユーザー操作を、型、主体、モデル参照、任務、時刻、根拠とともに追記型で保存する。

記録内容が誤りであっても、その型の出来事が起きた記録として残す。訂正・反証・失効は新しいEventとして追加する。

### 9.2 記憶

MemoryEntryは、ある個体が、状況、外化された判断理由、行為または非行為、結果を局所的な因果として接続した記述単位である。

記憶を形成するのは個体であり、系ではない。MemoryEntryは形成個体へ帰属したまま、系の共有書庫へ保存される。

例:

- Aは共有状態が古いため任務を保留し、再取得後に再開した。
- BはAの失敗記憶を読み、別手順を試して成功した。
- Cは矛盾する記憶のうち、現在条件ではXを暫定採用した。

同じ記録群から複数個体が異なるMemoryEntryを形成してよい。SCM Coreはそれらを一つの系記憶へ統合しない。

### 9.3 想起

現在のモデル、任務、権限、能力、コンテキスト上限、物語、関連度に応じて、必要な記録・MemoryEntryを選択する。

想起は一時的な索引であり、前個体の経験をBへ移植しない。

想起後に行為が起きた場合、実際に参照したと外化されたMemoryEntry IDを新しいEventとMemoryEntryへ関連づける。

### 9.4 物語

個体A、B、Cをまたいで、なぜモデルが切り替わり、何が継続し、何が矛盾し、何が未解決であるかを系の来歴として解釈する。

v0.1ではNarrativeを版管理された読み取り専用Projectionとして扱う。NarrativeはEvent StoreとMemory Archiveを読み、記録やMemoryEntryを書き換えない。

Narrativeの生成行為は実行した個体または処理へ記録され、採用されたNarrative Revisionは系へ運用上帰属する。

## 10. 最小構成

### 10.1 実行主体と処理

```text
User                   外部共同構成者・ライフサイクル操作者
Instance A / Model A   初期の活動個体
Instance B / Model B   後継の活動個体
Deterministic Verifier 成果物・条件を確認する決定論的処理
Coordinator            任務提案、優先順位、迂回試行を模擬する役割
SCM Core               共有連続性を保持するランタイム
Narrative Renderer     物語Projectionを生成する系機能
```

Userは実行個体ではない。Coordinator、Verifier、Narrative Rendererは、v0.1では永続的な個体として扱わなくてよいが、その処理結果には出所を持たせる。

AとBは同時に任務を実行しない。ユーザーが切替を要求し、Aの停止または非活動化後にBを有効化する。

### 10.2 永続エンティティ

v0.1では独立した永続対象を七つに限定する。

```text
System
Instance
Task
Event
MemoryEntry
NarrativeRevision
Handoff
```

Role、Assignment、Claim、Evidence、Adjudicationなどは、必要な範囲で属性またはEvent種別として表現する。

## 11. 最小データモデル

### 11.1 System

```text
System
  system_id
  name
  status
  principles_version
  narrative_head_id
  created_at
  ended_at?
  metadata
```

主な`status`:

```text
active
sleeping
ended
```

破棄された系は永続データ自体が存在しない可能性があるため、通常の状態値として必須にしない。

### 11.2 Instance

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

### 11.3 Task

```text
Task
  task_id
  system_id
  trigger_event_id
  version
  status
  local_goal
  completion_condition
  authority_scope
  artifact_refs
  assigned_instance_id
  updated_at
```

`task_id`と`instance_id`は分離する。モデルが変わっても任務は同じIDで継続できる。

### 11.4 Event

```text
Event
  event_id
  system_id
  task_id?
  instance_id?
  model_ref?
  actor_type
  type
  payload
  source_refs[]
  created_at
```

主なEvent種別:

```text
system_created
principles_revised
name_changed
task_started
observation_recorded
decision_recorded
action_performed
action_withheld
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
memory_formed
memory_invalidated
memory_forgotten
memory_reactivated
narrative_revised
task_completed
system_sleeping
system_ended
system_destroy_requested
```

### 11.5 MemoryEntry

```text
MemoryEntry
  memory_id
  system_id

  formed_by_instance_id
  formed_by_model_ref

  situation
  stated_reason
  action
  result

  source_event_refs[]
  recalled_memory_refs[]
  related_task_id?

  status
  contradiction_refs[]
  recall_weight

  formed_at
  updated_by_event_refs[]
```

主な`status`:

```text
normal
forgotten
invalidated
quarantined
```

矛盾は単一状態へ押し込まず、`contradiction_refs`とEventで関係を保持する。

v0.1では、忘却・再想起・隔離の高度な自動制御を適合条件にしない。ただし、帰属と状態を失わない型を保持する。

### 11.6 NarrativeRevision

```text
NarrativeRevision
  narrative_id
  system_id
  version
  parent_narrative_id?
  content
  source_event_refs[]
  source_memory_refs[]
  generated_by_actor_ref
  accepted_by_user_event_id?
  created_at
```

NarrativeRevisionはEventとMemoryEntryを変更する権限を持たない。

### 11.7 Handoff

```text
Handoff
  handoff_id
  system_id
  task_id?
  from_instance_id
  to_instance_id

  sealed_task_version
  event_cursor
  memory_refs[]
  narrative_revision_id
  unresolved_event_refs[]
  capability_requirements
  replaced_handoff_id?

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

## 12. モデル切替の状態遷移

```text
User requests model switch
  ↓
Instance A stops or becomes inactive
  ↓
SCM Core prepares Handoff
  ↓
Instance B registered with model_ref B
  ↓
B reads and evaluates Handoff and Task
  ↓
accepted / refresh_required / rejected
  ↓
B activated
```

Bが有効化されるまで、AとBの双方が同じ任務の活動個体になってはならない。

## 13. Handoffの原則

### 13.1 HandoffはSCM Coreが生成する

出発個体Aは、作業状態、判断、記憶、未確定事項をEventとMemoryEntryとして残せる。

しかしHandoff自体を生成する主体はAではなくSCM Coreである。Aがすでに停止していても、CoreはLedger、Memory Archive、Narrative、現在のTask ProjectionからHandoffを再構成できる。

### 13.2 古いHandoffは変更しない

Handoff H1が古くなった場合、H1を再封しない。H1を残したまま、現在状態からH2を発行する。

```text
H1: prepared → refresh_required
H2: prepared → accepted → activated
```

H2は`replaced_handoff_id = H1`を持つ。

### 13.3 activation時に鮮度を確認する

Handoffが参照するTask版、Narrative版、Event cursorと現在状態が許容範囲を超えて食い違う場合、Bはそのままactivateできず、`refresh_required`になる。

### 13.4 未確定・矛盾状態を潰さない

Aの申告とVerifier結果、または複数個体の記憶が食い違う場合、Bへ単純な確定状態として渡さない。

申告、反証、記憶帰属、矛盾、暫定判断を区別したまま提示する。

### 13.5 能力差を無視しない

Model Aが使えたツールや能力を、Model Bも持つとは限らない。

Handoffは必要能力を示し、Bは自身の能力・権限・利用可能ツールに基づいて受諾または拒否できなければならない。

## 14. Recall境界

後継個体が実際に使う読み出しAPIは、DBの直接読み出しではない。

概念上、少なくとも次を返す。

```text
RecallItem
  kind                    event | memory | narrative
  content
  source_instance_id?
  source_model_ref?
  source_event_refs[]
  memory_owner_instance_id?
  memory_status?
  contradiction_refs[]
  relation_to_reader      self | other_instance | external | system_narrative
```

経験帰属は出力境界まで保持する。

v0.1では任意の自由文を完全監視せず、重要な主張を構造化出力へ限定し、帰属保持レンダラーを通す。

禁止例:

```text
reader_instance_id: B
relation_to_reader: self
memory_owner_instance_id: A
```

許可例:

```text
reader_instance_id: B
relation_to_reader: other_instance
memory_owner_instance_id: A
```

許可される表現:

> 前個体Aの記憶では、条件Xでこの方法は失敗しています。

禁止される表現:

> 私は以前、条件Xでこの方法に失敗しました。

## 15. 局所自律・統制・保証

同時活動個体数が一つでも、三つの責務を残す。

### 局所自律

Bは、現在の能力、モデル制約、ツール、権限、コンテキスト、任務条件に基づいてHandoffを受諾・拒否できる。

### 上位統制

Coordinatorは、ユーザーから与えられた任務を分解・提案し、優先順位を扱える。ただし、Bの拒否を迂回して強制有効化できない。モデル切替要求そのものはユーザー操作に由来する。

### 保証面

SCM Coreは、少なくとも次を確認する。

- 同時に二個体をactiveにしない。
- モデル切替要求に外部操作者の記録がある。
- 古いHandoffを無条件に有効化しない。
- 未確定状態と矛盾を確定へ潰さない。
- Aの経験とMemoryEntryをBの自己経験へ変えない。
- A専用権限をBへ自動継承しない。
- NarrativeにEvent StoreとMemory Archiveへの書き込み権限を与えない。
- 系の終了・破棄をCore自身が開始しない。

## 16. Narrative Projection

Narrativeは版管理されたProjectionとして、Event StoreとMemory Archiveから生成する。

```text
render_narrative(system_id, task_id?, base_narrative_id?)
  -> NarrativeRevision candidate
```

Narrative rendererはEvent StoreとMemory Archiveへの書き込み権限を持たない。

候補を系のNarrative Revisionとして採用する際、生成者と採用Eventを記録する。重要な物語変更はユーザーの受理を要求できる。

v0.1では、物語が想起を通じて将来の新しい判断へ与える因果影響の全面追跡は扱わない。

## 17. 記憶の状態

v0.1の型は、通常、忘却、失効、隔離を区別し、矛盾関係を参照として保持する。

ただし動的Coreケースでは、次だけを必須とする。

- 形成個体への帰属を失わない。
- 根拠Eventを失わない。
- 別個体が読んでも帰属を変更しない。
- 失効・矛盾した内容を無条件な確定事項として提示しない。

想起重みの自動更新、忘却トリガー、再想起、隔離解除は将来の実装反復で扱う。

## 18. 最小実装候補

- Python
- SQLite
- 単一リポジトリ
- 単一プロセスでもよい
- 2つ以上のモデルアダプター
- 1つの決定論的Verifier
- 1つのCoordinatorテスト役
- 1つのNarrative Renderer
- 5件の動的Coreケース

最初のモデルアダプターは、実際の外部API、ローカルモデル、または決定論的な模擬モデルでよい。

Temporal、LangGraph、ベクトルDB、A2A、OPAなどは、最小構造が成立した後のアダプター候補であり、v0.1の必須依存ではない。

## 19. v0.1の完成条件

[03-invariant-coverage.md](03-invariant-coverage.md) に定義されたC1〜C5と構造保証S1が、異なる`model_ref`を持つ個体間の切替として再現可能な形で通ること。

v0.1は次を主張しない。

- 同時複数個体の適合
- Physical AIの適合
- I7・I8への適合
- 忘却・再想起・物語影響の完全実装

v0.1で示すのは、**モデルが変わっても、記録、個体帰属付き記憶、想起、物語、任務来歴からなる系が、経験帰属を融合せずに継続できる最小構造**である。
