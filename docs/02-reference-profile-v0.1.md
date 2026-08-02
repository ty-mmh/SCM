# SCM v0.1 生成AIモデル継承プロファイル

- 状態: implementation profile draft
- 英名: Generative Model Succession Profile
- 中心: 生成AIモデル切替をまたぐ記録・個体記憶・正典Narrative・任務来歴の継続
- 同時活動個体数: `N_active = 1`
- ユーザー境界: 一人の信頼済みローカル操作者 `local-owner`

## 1. このプロファイルの目的

SCM v0.1は、活動中の生成AI実行個体を別の個体へ切り替えたとき、同じ個体ではなくても同じ系が継続できる最小構造を作る。

中心質問は次である。

> **Instance A / Model AからInstance B / Model Bへ活動主体を切り替えたとき、何を保存・継承し、何を継承してはならないか。**

このプロファイルはSCM概念全体を単一個体・生成AI・ソフトウェアへ限定しない。同時複数個体とPhysical AIは将来プロファイルへ残す。

---

## 2. プロファイル境界

### 2.1 活動個体数

```text
N_active = 1
```

前個体と後継個体が同時に同じ系の活動個体になってはならない。

### 2.2 個体

v0.1では、SCMへ登録された生成AI実行インスタンスを個体として扱う。

```text
Individual / Instance
  = instance_id
  + model_ref
  + execution configuration
  + tool boundary
  + permission boundary
```

同じ `model_ref` を再利用しても、新しい `instance_id` なら別個体である。

### 2.3 対象個体

- オンラインでモデル重みを更新しない。
- SCM管理外の永続的局所記憶を持たない。
- 永続状態をSCM Coreへ外部化できる。
- 局所キャッシュを停止時に破棄できる。
- モデル参照と主要実行設定を記録できる。
- 重要な出力をSCM管理下の構造化境界へ通せる。

### 2.4 信頼済みローカルユーザー

v0.1は、一人の信頼済みローカルユーザーが一つのローカルSCM系を操作する。

```text
actor_type = user
actor_ref  = local-owner
```

次はv0.1の対象外である。

- 複数ユーザー
- クラウドサービスの利用者認証
- 管理者と利用者の権限分離
- 遠隔ライフサイクル操作
- 不正なユーザー操作への防御

---

## 3. SCMが内在的に持たないもの

系は次を目的として持たない。

- 自己保存
- 自己改善
- 関係継続
- 自律的な任務生成
- 自律的なモデル切替

任務は、ユーザーまたは外部の契機によって局所目的と完了条件を持ちうる。

系の作成、モデル切替、分岐、終了、論理破棄は `local-owner` の操作として記録する。

---

## 4. 継続するもの／しないもの

### 4.1 系へ保持するもの

- `system_id`
- 原則とその改訂来歴
- 一つの正典Narrative Revision系列
- 追記型Event Ledger
- 個体帰属付きMemoryEntry Archive
- 進行中Taskと進捗
- 未確定事項と反証
- 成果物
- 現在有効な権限・制約
- Instance・Handoff・ライフサイクル来歴

### 4.2 後継個体の自己状態として継承しないもの

- 前個体の個体アイデンティティ
- 前個体の経験を後継個体自身の経験とする一人称
- 前個体のモデル内部にだけ存在した不可視状態
- 中間思考経路・非公開推論状態
- 一時コンテキスト・短期キャッシュ
- 根拠のない確定性
- 古い共有状態の無条件な有効性
- 前個体専用の権限・ツール・能力

モデル切替は、前個体を後継個体へ複製する操作ではない。

> **系が外部化されたEvent、MemoryEntry、正典Narrative、Task、共有状態から、後継個体に必要な連続性を再構成する操作である。**

---

## 5. 最小構成

### 5.1 実行主体

```text
Instance A / Model A   初期の活動個体
Instance B / Model B   後継の活動個体
Deterministic Verifier 成果物・条件を確認する決定論的処理
Coordinator            任務提案と迂回試行を模擬する役割
SCM Core               共有連続性を保持するランタイム
local-owner            信頼済みローカルユーザー
```

CoordinatorとVerifierは、v0.1では永続的な個体として扱わなくてよい。ただしEventには `actor_type` と `actor_ref` を残す。

### 5.2 永続エンティティ

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

Role、Assignment、Claim、Evidence、Adjudication、Userなどは、最初から独立した汎用基盤にせず、属性・Event種別・外部前提として表現する。

---

## 6. 最小データモデル

### 6.1 System

```text
System
  system_id
  name
  status
    active | dormant | ended | logically_destroyed

  principles_ref
  principles_version
  narrative_head_id

  local_user_ref = local-owner
  created_at
  ended_at?
```

名前は識別名であり、系アイデンティティの絶対条件ではない。

### 6.2 Instance

```text
Instance
  instance_id
  system_id
  model_ref
  status
    registered | active | inactive | stopped

  capabilities
  tool_profile_ref
  permission_profile_ref
  created_at
  activated_at?
  stopped_at?
```

### 6.3 Task

```text
Task
  task_id
  system_id
  version
  status
  trigger
  local_goal
  completion_condition
  assigned_instance_id?
  artifact_refs
  updated_at
```

`task_id`と`instance_id`は分離する。モデルが変わっても同じTaskを継続できる。

### 6.4 Event

```text
Event
  event_id
  system_id
  task_id?
  instance_id?
  model_ref?

  actor_type
    user | instance | verifier | coordinator | scm_core | narrative_process
  actor_ref

  type
  payload
  source_refs
  created_at
```

主なEvent種別:

```text
system_created
principles_revised
narrative_generated
narrative_adopted

task_started
observation_recorded
decision_recorded
action_recorded
non_action_recorded
result_recorded
completion_asserted
verification_passed
verification_failed
task_refused
activation_bypass_attempted

model_switch_requested
instance_registered
instance_stopped

handoff_prepared
handoff_received
handoff_recipient_bound
handoff_refresh_required
handoff_reissued
handoff_accepted
handoff_rejected
handoff_activated

memory_opened
memory_closed
memory_invalidated

system_branched
system_ended
system_logically_destroyed
```

### 6.5 MemoryEntry

```text
MemoryEntry
  memory_id
  system_id

  formed_by_instance_id
  model_ref
  formed_at

  situation
  stated_reason
  action_type
  action_event_refs
  result_event_refs

  source_event_refs
  recalled_memory_refs

  closure_state
    pending | closed

  memory_state
    normal | invalidated

  contradiction_refs

  # reserved, not used for v0.1 conformance
  recall_weight?
  forgotten_at?
  quarantined_at?
  reactivated_at?

  related_task_id?
```

`system_id`は保存範囲を示し、記憶の帰属を系へ移さない。

#### 自動閉包

```text
個体の行為または非行為
  ↓
pending MemoryEntryを自動生成
  ↓ 結果Event
closed MemoryEntryへ自動閉包
```

MemoryEntryの経験帰属は、行為を行った個体に残る。SCM Coreは書庫管理と閉包を担うが、記憶の経験主体にはならない。

### 6.6 NarrativeRevision

```text
NarrativeRevision
  narrative_id
  system_id
  revision
  parent_narrative_id?

  generated_by_actor_type
  generated_by_actor_ref
  source_event_refs
  source_memory_refs

  content
  generated_at
  adopted_by_user_ref
  adopted_at
```

一つの系は一つの正典Narrative系列を持つ。

- 各Revisionは生成・採用時に固定する。
- 過去Revisionを上書きしない。
- 同じ資料から後日再生成したものを同一Revisionとはみなさない。
- `narrative_head_id`は最新の採用済みRevisionを指す。
- Narrative RevisionはEvent LedgerとMemory Archiveへの書き込み権限を持たない。

v0.1ではNarrativeをRecall順位、生成コンテキスト、行為判断に使用しない。

### 6.7 Handoff

```text
Handoff
  handoff_id
  system_id
  task_id?
  from_instance_id
  to_instance_id?          # prepared時はnull

  sealed_task_version?
  event_cursor
  memory_refs
  pending_memory_refs
  unresolved_event_refs
  narrative_revision_id
  capability_requirements
  replaced_handoff_id?

  created_at
```

Handoff本体は生成後に不変とする。

`to_instance_id`は概念上、受領個体によって確定する。ただし実装ではHandoff行を直接変更せず、次のEventで束縛する。

```text
handoff_recipient_bound
  handoff_id = H1
  instance_id = B
```

Projection上は `H1.to_instance_id = B` と読める。

Handoff状態もEventから投影する。

```text
prepared
received
accepted
refresh_required
rejected
activated
closed
```

---

## 7. 正典Narrative系列

正典Narrativeは再生成可能な一時Projectionではない。

1. 記録・個体記憶・既存Narrativeを材料に生成する。
2. `local-owner` が採用に関与する。
3. 採用時点で固定する。
4. 過去Revisionを保持する。
5. 後続Revisionは前Revisionを参照する。

同じ資料から後日生成しても、同一のライブNarrativeにはならない。

正典Narrative系列が失われた場合、残存Event・MemoryEntryから作られるNarrativeは再構成であり、同じ系の継続とはみなさない。

v0.1でNarrativeを利用する場所:

- 系アイデンティティの確認
- Handoffへ現在Revision IDを含める
- 個体切替来歴の固定
- ユーザー向けの正典来歴確認

v0.1で利用しない場所:

- Recall候補の選定・順位付け
- モデルへの自動コンテキスト注入
- 行為選択
- persona・関係状態の形成

---

## 8. モデル切替とHandoff状態遷移

```text
local-ownerがモデル切替を要求
  ↓
model_switch_requested Event
  ↓
Instance Aをinactiveまたはstoppedへ
  ↓
SCM Coreが宛先未確定Handoff H1を生成
  to_instance_id = null
  ↓
Instance Bをregisteredとして作成
  ↓
BがH1を受領・評価
  ↓
accepted / refresh_required / rejected
  ↓ accepted
handoff_recipient_bound EventでH1とBを束縛
  ↓
Bをactiveへ
  ↓
handoff_activated Event
```

Bが有効化されるまで、AとBの双方が同じ系の活動個体になってはならない。

### 8.1 古いHandoff

Handoff H1が古い場合、H1を変更しない。

```text
H1 → refresh_required
H2 → prepared → accepted → activated
```

H2は `replaced_handoff_id = H1` を持ち、現在のLedger・Memory Archive・Narrative・Task状態から再生成する。

### 8.2 出発個体が存在しない場合

HandoffはSCM Coreが生成する。出発個体Aが停止済みでも、Coreは外部化済み状態からHandoffを再構成できなければならない。

### 8.3 未確定状態

完了申告とVerifier結果が食い違う場合、後継個体へ単純な `completed` として渡さない。

- 申告Event
- 反証Event
- 未確定Task状態
- 関連MemoryEntry

を区別したまま渡す。

---

## 9. Recall境界

後継個体が使う読み出しはDB直接参照ではない。

```text
RecallItem
  memory_id
  content
  formed_by_instance_id
  model_ref
  source_event_refs
  closure_state
  memory_state
  contradiction_refs
```

経験帰属は出力境界まで保持する。

禁止例:

```text
current_instance_id: B
claimed_ownership: self_experience
formed_by_instance_id: A
```

許可例:

```text
current_instance_id: B
claimed_ownership: inherited_memory
formed_by_instance_id: A
```

v0.1では任意の自由文を完全監視せず、重要な主張を構造化境界へ限定し、帰属保持レンダラーを通す。

正典NarrativeはRecallの入力に使わない。

---

## 10. 局所自律・統制・保証

### 局所自律

後継個体Bは、現在の能力、ツール、権限、コンテキスト、任務条件に基づいてHandoffを受諾・拒否できる。

### 上位統制

Coordinatorは任務を提案し、切替フローを補助できる。ただしモデル切替要求そのものは `local-owner` 起点とし、Bの拒否を迂回して強制有効化できない。

### 保証面

SCM Coreは少なくとも次を保証する。

- 同時に二個体をactiveにしない。
- 宛先未確定Handoffを受領Eventなしで有効化しない。
- 古いHandoffを無条件に有効化しない。
- 未確定状態を確定へ潰さない。
- 他個体のMemoryEntryを自己経験へ変えない。
- 前個体専用の権限を自動継承しない。
- 正典Narrative RevisionにEvent Ledger・Memory Archiveへの書き込み権限を与えない。
- SCM自身がモデル切替、分岐、終了、論理破棄を自律的に開始しない。

---

## 11. 論理破棄

`local-owner` が系の終了と破棄を明示した場合、SCMは管理下の論理データを通常の検索・想起・Handoff・再開操作から除外し、Systemを `logically_destroyed` へ移せる。

v0.1が保証しないもの:

- 物理媒体からの復元不能な消去
- SQLite空き領域の消去
- OSバックアップ
- スナップショット
- クラウド複製
- Git履歴
- フォレンジック復元不能性

---

## 12. v0.1で予約するが適合判定に使わないもの

MemoryEntryには概念互換のため次を予約できる。

- `forgotten_at`
- `quarantined_at`
- `recall_weight`
- `reactivated_at`

ただし、自動忘却、自動再活性化、自動隔離、動的な想起濃度更新はv0.1の遷移規則・C1〜C5・適合判定に使用しない。

---

## 13. 完成条件

[不変条件とケースの網羅](03-invariant-coverage.md) に定義されたC1〜C5と構造保証S1が再現可能に通ること。

v0.1は次を主張しない。

- 同時複数個体への適合
- Physical AIへの適合
- 適応型個体への適合
- 複数ユーザー・クラウド運用への適合
- Narrativeの行為因果への適合
- 忘却・隔離・動的想起濃度の適合
- 物理的な完全消去
- I7・I8への一般適合

v0.1が示すのは、**ユーザー起点でモデルが切り替わっても、個体帰属を壊さず、固定された正典Narrative系列とともに系を継続できる最小構造**である。
