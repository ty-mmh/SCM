# 近傍・既知技術とのマッピング

- 状態: research map, 2026-07-29
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
| 耐久的な任務継続 | Temporal、LangGraph | Event History、checkpoint、resume、同一Workflow/Threadの継続 | 実行個体IDと任務IDを分け、個体交換時に経験帰属・未確定状態・権限を含めてHandoffする |
| 追記型記録と来歴 | Event Sourcing、W3C PROV | append-only history、Entity/Activity/Agent、derivation、attribution | 申告と検証を区別し、後継個体が使うRecallへ経験所有を保持する |
| 任務提案と拒否 | Contract Net Protocol、A2A Protocol | task proposal、allocation、task state、`REJECTED` | 局所拒否をHandoff activationへ結び、上位側から迂回できない経路を作る |
| 共有メモリ | Letta shared memory blocks | 複数エージェントから同じ永続メモリを利用する | 直接共有だけでなく、出所、経験帰属、未確定状態、個体交代を扱う |
| 統治された共有メモリ | MemClaw、Governed Collaborative Memory | scope、temporal supersession、provenance、selection・revision traces | 共有メモリを目的・任務・Handoff・系アイデンティティへ接続する |
| トランザクション的記憶 | MemTX | belief commit、provenance、validity、cascade repairという問題設定 | v0.1では多段階Memory Promotionを延期し、Handoffで未確定状態を潰さない最小部分だけを実装する |
| 記憶の可搬性 | Portable Agent Memory | 構造化記憶、provenance graph、scoped transfer | 記憶だけでなく、任務、進捗、未確定事項、経験帰属を一つのHandoffとして扱う |
| 保証・Attestation | Simplex/Runtime Assurance、RATS | untrusted componentとtrusted monitorの分離、Evidence/Verifier/Relying Party | v0.1では一般保証基盤を作らず、局所拒否とHandoff activationの限定境界だけを扱う |
| 物理フリート | Open-RMF | 複数フリートと設備の相互運用、上位調停 | 物理実体の固有状態・摩耗・校正を含むプロファイルは将来課題 |

## 3. 成熟した近傍

### Temporal

TemporalのContinue-As-Newは、現在状態を新しいWorkflow Executionへ渡し、同じWorkflow IDと異なるRun IDで履歴を継続する。Event Historyはappend-only logとしてDurable Executionを支える。

SCMはこの能力を代替しない。SCMが追加しようとするのは、実行個体の交換時に、誰の経験か、何が未確定か、どの権限が引き継がれるかを保持する意味論である。

- [Continue-As-New](https://docs.temporal.io/workflow-execution/continue-as-new)
- [Events and Event History](https://docs.temporal.io/workflow-execution/event)

### LangGraph

LangGraphはgraph stateをcheckpointとして保存し、thread単位で再開・interrupt・replayを行える。これは状態継続の既知実装である。

- [LangGraph Persistence](https://docs.langchain.com/oss/python/langgraph/persistence)

### W3C PROV

PROV-DM／PROV-Oは、Entity、Activity、Agent、derivation、attributionなど、来歴表現の成熟した共通語彙を提供する。SCMはPROVの代替ではなく、SCM固有のEvent・Memory・HandoffをPROVへ写像できる可能性がある。

- [PROV-DM](https://www.w3.org/TR/prov-dm/)
- [PROV-O](https://www.w3.org/TR/prov-o/)

### Contract Net Protocol

Contract Net Protocolは、分散ノード間のtask announcement、bid、awardを扱う古典的な任務配分方式である。SCMの「任務提案」は未踏の概念ではない。

- [R. G. Smith, The Contract Net Protocol (1980)](https://www.reidgsmith.com/Contract_Net_Protocol_Dec-1980.pdf)

### A2A Protocol

A2AはTask、Message、Artifact、task stateを定義し、`TASK_STATE_REJECTED`を持つ。SCMはA2A自体を置き換えず、拒否とHandoff継続の意味をSCM Coreで保持する。

- [A2A Protocol Specification](https://a2a-protocol.org/dev/specification/)

### Letta shared memory

Lettaのmemory blockは複数エージェントへattachでき、共有永続メモリとして利用できる。これは「複数個体が同じ記憶へ接続する」実装例である。

- [Letta Memory Blocks](https://docs.letta.com/guides/core-concepts/memory/memory-blocks)

### RATS

RFC 9334はAttester、Evidence、Verifier、Attestation Results、Relying Partyを分離する。SCMの将来保証面にとって有力な近傍だが、v0.1 Coreではremote attestationを必須にしない。

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

## 5. SCMが新しく作る仕事

個別機能の大半には既知の近傍がある。SCMの実装上の仕事は、その代替品を一から作ることではない。

v0.1で焦点を置く差分は次である。

1. **任務IDと個体IDを分離する。**
2. **Handoffを例外処理ではなく一級操作にする。**
3. **出発個体が消えても、系がLedgerからHandoffを再構成する。**
4. **完了申告と反証が食い違う場合、未確定状態を潰さず後継へ渡す。**
5. **他個体の経験を、Recallから出力まで自己経験へ変えない。**
6. **局所拒否を上位側から迂回できないHandoff activation経路を作る。**
7. **Narrativeを非権威的な読み取りProjectionにする。**
8. **不変条件とケースの対応を明示し、未測定条件を適合済みに見せない。**

## 6. 現時点で主張しないこと

- SCMの各構成要素が個別に新しいこと
- 同じ制約集合の先行例が絶対に存在しないこと
- v0.1がI1〜I8をすべて満たすこと
- 適応型個体、物理実体、人間組織へ同じ保証が成立すること
- 形成中プレプリントの結果が独立再現済みであること
