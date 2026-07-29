# 延期したプロファイルと機構

- 状態: non-normative roadmap
- 目的: SCM概念上の射程と、v0.1で適合を主張する範囲を分ける

## 1. プロファイル区分

| プロファイル | SCM概念上の位置 | 現在の扱い |
|---|---|---|
| Generative Model Succession | 通時的なモデル切替 | v0.1参照実装・適合対象 |
| Concurrent Multi-Instance | 共時的な複数個体稼働 | 将来プロファイル、未定義 |
| Adaptive Stateful Executor | 永続的局所状態を持つ個体 | 将来プロファイル、未定義 |
| Embodied / Physical Executor | Physical AI | SCMの射程内、将来プロファイル、未定義 |
| Human Organizational Operation | 人間を含む運用 | 非規範の類似構造、適合対象外 |

v0.1の対象を限定しても、SCM概念全体から同時複数個体やPhysical AIを削除しない。

一方、現在の適合条件を、それらの領域へ暗黙に適用しない。

## 2. Concurrent Multi-Instance

SCM概念は、同じ時刻に複数の個体が同じ系へ接続する構造を保持する。

将来プロファイルでは、少なくとも次が必要になる。

- 複数個体への同時任務配分
- 共有状態の競合・整合性
- 複数観測の不一致
- 経験所有と情報祖先の分離
- 個体間通信と局所拒否
- 同じ失敗をする相関故障
- 複数個体の追加・離脱・隔離

v0.1では`N_active = 1`とし、これらを測定しない。

ただし、Event、Memory、Handoffなどの型から`instance_id`を削除せず、単一個体前提をSCM Coreへ埋め込まない。

## 3. Adaptive Stateful Executor

オンライン学習、永続的な局所記憶、自己改変、長期キャッシュなどを持つ個体は、外部状態を復元しても行動傾向が回復するとは限らない。

必要になる可能性がある論点:

- 局所状態と系状態の境界
- 行動ヒステリシス
- 状態復元後のbehavior revalidation
- 個体再構築と連続性判定
- モデル重み・方策版の来歴

v0.1ではこれらを扱わず、交換可能・外部状態型の生成AIモデル個体へ限定する。

## 4. Embodied / Physical Executor

Physical AIはSCM概念の射程に含まれる。

物理実体には、ソフトウェア状態だけでは外部化できない固有状態がある。

- 摩耗
- 校正
- 温度・荷重履歴
- 損傷
- バッテリー劣化
- センサー差
- 身体適応
- 部品構成と交換履歴

Physical SCMでは、Handoffは単純な個体交換ではなく、身体適合性、再校正、能力差、安全保証を含む再認可になる。

近傍としてOpen-RMF、Asset Administration Shell、Runtime Assuranceなどがあるが、現行v0.1の適合条件をそのまま物理系へ適用しない。

v0.1では、機体情報、部品来歴、摩耗、校正、機能安全をデータモデルへ含めない。

まず生成AIモデルの切替で、記録・記憶・想起・物語・任務来歴とHandoffの骨格を作る。

## 5. Human Organizational Operation

交代勤務、当直、申し送り、オンコール輪番は、個体交代をまたぐ任務継続という点でSCMの一部構造に先行する類例である。

ただし人間は、次の意味でv0.1プロファイルと根本的に異なる。

- 永続的な局所記憶を外部化できない。
- 停止時に内部キャッシュを破棄できない。
- 経験による行動変化を持つ。
- 同意、責任、退出権、労働倫理が必要になる。

したがって、人間運用は現時点では適合対象ではなく、非規範の比較対象とする。

## 6. 延期した横断機構

### Egress Contract

外部利用者へ出す結果が、内部の権威状態や不確実性より強い表現にならないための境界。重要だが、v0.1では後継個体向けの制限ビューを先に作る。

### External Root / Bootstrap Plane

SCMが自己認可できない信頼の根、鍵、Verifier、時計、bootstrap policy。v0.1では実行環境の外部前提として固定する。

### Information Ancestry / Independence Assessment

複数証拠が同じ情報祖先を共有していないかを評価する機構。v0.1では決定論的Verifierを置き、一般的な独立性評価を行わない。

### 多段階Memory Promotion

```text
local → candidate → verified → scoped_shared → system_memory
```

といった昇格・隔離・撤回。v0.1では`local / shared`の二状態から始める。

### Task constraint propagation

拒否由来の制約を再割当やタスク分解の子孫へ伝播する機構。C4は同一activationの直接迂回だけを測り、一般的な制約ロンダリングは延期する。

### Restoration / behavior revalidation

状態復元と行動回復を分ける機構。I7とともに延期する。

### General fault localization

共有記憶汚染、相関故障、全系更新の隔離。I8とともに延期する。

## 7. 再導入条件

延期した構造をCoreへ戻すには、次を満たす。

1. 現行または次期プロファイルの明示的な適用対象である。
2. その構造を欠くと失敗するケースがある。
3. 根拠主張の証拠状態が記録されている。
4. 既存構造へ縮約できない。
5. 導入時にCoreの中心が共有連続性から漂流しない。
