# 近傍・既知技術とのマッピング

- 状態: research map, 2026-08-02
- 方針: 成熟した標準・製品と、形成中の単一プレプリントを同じ証拠強度で扱わない
- 注意: 本文の「SCMの仕事」は新規性・優先権を断定するものではない

## 1. 証拠強度

| 区分 | 意味 |
|---|---|
| 成熟 | 標準、長期利用、複数実装または公式製品文書がある |
| 運用・実装 | 実装と運用例があるが、一般標準ではない |
| 形成中 | 単一または少数の新しいプレプリント・試作 |
| 未確認 | 会話や二次情報に現れたが、一次資料の内容確認が未完了 |

## 2. 責務マッピング

| SCMの責務 | 近傍・既知 | 借りられる部分 | SCMとして残る仕事 |
|---|---|---|---|
| 耐久的な任務継続 | Temporal、LangGraph | Event History、checkpoint、resume、同一Workflow/Threadの継続 | 実行個体IDと任務IDを分け、個体交代時に正典Narrative・個体記憶・未確定状態を含めてHandoffする |
| 追記型記録と来歴 | Event Sourcing、W3C PROV | append-only history、Entity/Activity/Agent、derivation、attribution | 型を持つEventと、個体が形成するMemoryEntryを分離する |
| 任務提案と拒否 | Contract Net Protocol、A2A Protocol | task proposal、allocation、task state、`REJECTED` | 局所拒否をHandoff activationへ結び、上位側から迂回できない経路を作る |
| 共有メモリ | Letta shared memory blocks | 複数エージェントから同じ永続メモリを利用する | 共同所有ではなく、個体帰属を維持した共有書庫として扱う |
| 統治された共有メモリ | MemClaw、Governed Collaborative Memory | scope、temporal supersession、provenance、selection・revision traces | 個体記憶を系記憶へ統合せず、Handoff・正典Narrative・系アイデンティティへ接続する |
| トランザクション的記憶 | MemTX | belief commit、provenance、validity、cascade repairという問題設定 | v0.1ではMemoryEntry自動閉包と未確定状態の継承へ限定する |
| 記憶の可搬性 | Portable Agent Memory | 構造化記憶、provenance graph、scoped transfer | 記憶転送ではなく、帰属を保った読書的継承として扱う |
| 保証・Attestation | Simplex/Runtime Assurance、RATS | untrusted componentとtrusted monitorの分離、Evidence/Verifier/Relying Party | v0.1では局所拒否、Handoff受領束縛、帰属保持の限定境界を扱う |
| 物理フリート | Open-RMF | 複数フリートと設備の相互運用、上位調停 | 外部化できない身体状態と診断記録を分ける将来プロファイルが必要 |

## 3. 成熟した近傍

### Temporal

TemporalのContinue-As-Newは、現在状態を新しいWorkflow Executionへ渡し、同じWorkflow IDと異なるRun IDで履歴を継続する。Event Historyはappend-only logとしてDurable Executionを支える。

SCMが追加しようとするのは、個体交換時に、誰の経験か、どのMemoryEntryがpendingか、どの正典Narrative Revisionへ接続するかを保持する意味論である。

- [Continue-As-New](https://docs.temporal.io/workflow-execution/continue-as-new)
- [Events and Event History](https://docs.temporal.io/workflow-execution/event)

### LangGraph

LangGraphはgraph stateをcheckpointとして保存し、thread単位で再開・interrupt・replayを行える。これは状態継続の既知実装である。

- [LangGraph Persistence](https://docs.langchain.com/oss/python/langgraph/persistence)

### W3C PROV

PROV-DM／PROV-Oは、Entity、Activity、Agent、derivation、attributionなど、来歴表現の成熟した共通語彙を提供する。SCM固有のEvent、MemoryEntry、NarrativeRevision、HandoffをPROVへ写像できる可能性がある。

- [PROV-DM](https://www.w3.org/TR/prov-dm/)
- [PROV-O](https://www.w3.org/TR/prov-o/)

### Contract Net Protocol

Contract Net Protocolは、分散ノード間のtask announcement、bid、awardを扱う古典的な任務配分方式である。SCMの任務提案自体は未踏ではない。

- [R. G. Smith, The Contract Net Protocol (1980)](https://www.reidgsmith.com/Contract_Net_Protocol_Dec-1980.pdf)

### A2A Protocol

A2AはTask、Message、Artifact、task stateを定義し、`TASK_STATE_REJECTED`を持つ。SCMはA2Aを置き換えず、拒否とHandoff継続の意味をCoreで保持する。

- [A2A Protocol Specification](https://a2a-protocol.org/dev/specification/)

### Letta shared memory

Lettaのmemory blockは複数エージェントへattachでき、共有永続メモリとして利用できる。

SCMとの差は、同じ保存場所を読むことではなく、MemoryEntryを形成個体へ帰属させ、後継個体が他者の記述として読む点にある。

- [Letta Memory Blocks](https://docs.letta.com/guides/core-concepts/memory/memory-blocks)

### RATS

RFC 9334はAttester、Evidence、Verifier、Attestation Results、Relying Partyを分離する。SCMの将来保証面に有力な近傍だが、v0.1はremote attestationを必須にしない。

- [RFC 9334: RATS Architecture](https://www.rfc-editor.org/rfc/rfc9334.html)

### Runtime Assurance

Simplex型Runtime Assuranceは、未検証の高度制御器と信頼済みの安全制御器を分ける。SCMのI4・将来の保証面に近いが、v0.1では限定されたactivation gateだけを扱う。

- [NASA: A Formal Verification Framework for Runtime Assurance](https://ntrs.nasa.gov/citations/20230017350)

### Open-RMF

Open-RMFは複数のロボットフリートと物理設備の相互運用を提供する。Physical SCMの将来アダプター候補であり、現行参照プロファイルの適合対象ではない。

- [Open-RMF](https://www.open-rmf.org/)

## 4. 形成中の近傍

以下は2026年のプレプリントである。問題設定の独立到達や試作の存在を示すが、成熟した標準・複数ベンダ実装と同じ証拠力ではない。

### Governed Shared Memory for Multi-Agent LLM Systems / MemClaw

fleet-memoryの故障モードとして、unauthorized leakage、stale propagation、contradiction persistence、provenance collapseを定式化し、scoped retrieval、temporal supersession、provenance tracking、policy-governed propagationを実装する。

- arXiv:2606.24535
- [Abstract / metadata](https://arxiv.org/abs/2606.24535)
- 確認状態: existence verified / abstract verified

### Governed Collaborative Memory as Artificial Selection

agent-local、shared institutional、archive、project-continuity memoryを分け、どの記憶を共有制度状態へ昇格・棄却・改訂するかを設計対象にする。

- arXiv:2605.04264
- [Abstract / metadata](https://arxiv.org/abs/2605.04264)
- 確認状態: existence verified / abstract verified

### MemTX: Transactional Belief Commit for Stateful Agent Memory

memory writeとbelief commitを分け、evidence、permissions、provenance、validity、staged commit、cascade repairを扱う。

- arXiv:2607.23929
- [Abstract / metadata](https://arxiv.org/abs/2607.23929)
- 確認状態: existence verified / abstract verified

### Portable Agent Memory

異種エージェント間で、構造化記憶とprovenance graph、scoped accessを伴う移送を提案する。

- arXiv:2605.11032
- [Abstract / metadata](https://arxiv.org/abs/2605.11032)
- 確認状態: existence verified / abstract verified

## 5. SCM v0.1が作る仕事

個別機能の大半には既知の近傍がある。SCMの仕事は、それらの代替品を一から作ることではない。

v0.1で焦点を置く差分は次である。

1. **任務IDと個体IDを分離する。**
2. **記録と、形成個体へ帰属するMemoryEntryを分離する。**
3. **共有を共同所有ではなく、帰属付き書庫の可読性として定義する。**
4. **Handoffを例外処理ではなく一級操作にする。**
5. **出発個体が消えても、系が外部化資料からHandoffを再構成する。**
6. **宛先未確定Handoffを、受領Eventによって後継個体へ束縛する。**
7. **完了申告と反証が食い違う場合、未確定状態を潰さない。**
8. **他個体のMemoryEntryを、Recallから出力まで自己経験へ変えない。**
9. **一つの正典Narrative系列を固定されたRevisionとして保持する。**
10. **Narrativeを失った後の再生成を、継続ではなく再構成とする。**
11. **系のライフサイクルを信頼済みローカルユーザー起点に限定する。**

## 6. 現時点で主張しないこと

- SCMの各構成要素が個別に新しいこと
- 同じ制約集合の先行例が絶対に存在しないこと
- v0.1がI1〜I8をすべて満たすこと
- Narrativeが行為へ影響する構造を実装済みであること
- 忘却・隔離・想起濃度の自動制御を実装済みであること
- 適応型個体、物理実体、人間組織へ同じ保証が成立すること
- 複数ユーザー・クラウド運用へ適合すること
- 形成中プレプリントの結果が独立再現済みであること
