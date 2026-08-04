# 設計更新・剪定・証拠の規則

- 状態: project policy draft
- 目的: レビュー往復が単調増加し、SCM Coreが実装不能になることを防ぐ

## 1. 開発姿勢

SCMは概念文だけで完結しない。共有連続性の構造を実装し、各構造遷移と並行して、その遷移を破るケースを置く。

> **構造なき診断を先行させない。診断なき構造を固定しない。**

ケースは本体の代替ではない。合否判定だけでなく、構造上の不足を発見してCoreへ戻す観測装置として扱う。

また、会話内での補完や工学的提案を、ユーザーが明示的に置いた概念判断と混同しない。

## 2. v0.1の中心判定

新しい構造がv0.1 Coreに属するかは、次の問いで判定する。

> 個体Aが消え、個体Bが系へ接続するとき、この構造がなければ、何が失われ、混同され、または不正に継承されるか。

現在の中心対象は次である。

- System / Instance / TaskのID分離
- Append-only Event Ledger
- 個体帰属付きMemoryEntry Archive
- 変更不能な根底原則
- 一つの正典Narrative Revision系列
- 系の専用Narrative Process
- 宛先未確定Handoffと受領Eventによる束縛
- 経験帰属を保持するRecall境界
- 信頼済みローカルユーザー起点のライフサイクル

単一個体でも同じ価値を持つ一般的な権威・監査・証拠基盤は、Handoffと経験帰属へ直接必要な最小範囲を除き、Coreから延期する。

## 3. 新規構造の採用条件

新しい独立構造をCoreへ入れるには、次をすべて満たす。

1. モデル切替をまたぐ共有連続性に必要である。
2. その構造を欠くと失敗するCoreケース、構造保証、または明示的なプロファイル制約がある。
3. 既存構造の属性またはEvent種別へ単純に畳めない。
4. v0.1参照プロファイルの対象内である。
5. 外部の経験的主張へ依存する場合、証拠状態が明示されている。
6. ユーザーの明示的決定か、アシスタントによる提案・補正かが区別されている。
7. 追加時に、何を縮約・延期・削除できるかを同時に検討している。

## 4. 剪定条件

次のいずれかに該当する構造は、Coreから縮約・延期・削除する。

- 現在のCoreケース、構造保証、プロファイル制約のいずれからも参照されない。
- なくても同じケースが同じ意味で通る。
- 既存のより単純な構造へ統合できる。
- 将来プロファイルにしか必要ない。
- 未確認の外部主張だけを根拠に導入された。
- ユーザーが明示していない補完を、正典判断として固定している。
- 導入により、共有連続性より別問題のほうが大きくなる。
- 二巡連続で、どのCoreケースにも要求されない。

| 操作 | 意味 |
|---|---|
| retain | Coreに残す |
| collapse | 独立構造をやめ、既存属性・Eventへ畳む |
| defer | 将来プロファイル／拡張候補へ移す |
| remove | 現行仕様から除去し、再導入時は採用条件を通し直す |

各レビュー巡の終わりに、この四判定を必ず行う。

## 5. 権限・帰属・実行可能性を混同しない

次を別々に扱う。

- SCMの正規遷移として誰に帰属するか
- 実装上どのモデル・プロセスが計算したか
- ローカルユーザーが物理的に何を変更できるか
- SCM内部から変更を検出・証明できるか

例:

- Narrativeの解釈・判断・帰属は系にある。
- Narrativeを計算したモデル・プロセスは実行来歴として残る。
- ユーザーはファイルを直接変更できるが、Narrativeの正規編集主体ではない。
- 全状態が整合的に改変された場合、SCM単独では真正性を証明できない。

## 6. 予約フィールドの規則

概念互換のため予約フィールドを残す場合、次を明記する。

- v0.1の遷移規則に使用するか
- C1〜C5の合否に使用するか
- 適合判定に使用するか
- 未使用なら、どの将来プロファイルへ属するか

現在のMemoryEntryにおける次の項目は予約であり、v0.1適合判定へ使用しない。

- `recall_weight`
- `forgotten_at`
- `quarantined_at`
- `reactivated_at`

予約フィールドを置くことは、機構を実装済みと主張することではない。

## 7. ケース集合の統制

- v0.1の動的Coreケースは最大5件。
- ケース追加は、未測定不変条件を初めて測る場合、または既存ケースの交差故障を示す場合に限る。
- 新規追加は、原則として既存ケースの置換または統合を伴う。
- 単なるパラメータ違いはcase variantとして扱う。
- 特定構造を残すためだけに後付けしたケースを認めない。
- 合否条件がないケース、欠陥実装でも失敗しえないケースを数えない。

ケース集合をCoreの唯一の権威にはしない。不変条件、参照プロファイル、ケース、構造保証、プロファイル制約を [03-invariant-coverage.md](03-invariant-coverage.md) で対応づける。

## 8. 設計主張の証拠レジストリ

外部文献だけでなく、会話内の提案、設計仮説、局所実験も、Coreへ影響した時点で同じ台帳へ置く。

```text
DesignClaim
  claim_id
  statement

  kind:
    logical_deduction
    canonical_requirement
    design_choice
    external_evidence
    conversation_hypothesis
    assistant_proposal
    local_experiment

  source
  verification_status
  affected_structure
  disposition
```

### 検証状態

```text
unverified
user_stated
user_confirmed
existence_verified
metadata_verified
abstract_verified
fulltext_verified
claim_location_verified
reproduced_locally
independently_reproduced
```

「ユーザーが置いた」「ユーザーが提案を受理した」「アシスタントが補完した」を分ける。

「論文が存在する」と「本文が特定結果を報告する」と「結果が再現した」も分ける。

### Core採用との接続

次の条件をともに満たす構造は、最初の削除・延期候補になる。

- 根拠が`conversation_hypothesis`、`assistant_proposal`、未確認の`external_evidence`だけである。
- 現在のCoreケース・構造保証・プロファイル制約から要求されない。

一方、`task_id`と`instance_id`の分離、Handoffの宛先未確定、MemoryEntryの個体帰属、Narrativeの系帰属など、ケースと正典から論理的に要求される構造は、学術文献がなくてもCoreへ置ける。

## 9. 形成中研究の引用規約

形成中研究を引用するときは、最低限次を一組で固定する。

- タイトル
- 著者
- 永続ID（例: arXiv ID）
- 版または確認日
- existence status
- content status
- reproduction status

文献名やIDの正しさだけで、内容主張を確認済みとみなさない。

## 10. 現在の剪定結果

### Coreへ残す

- System / Instance / TaskのID分離
- Append-only Event Ledger
- 自動閉包型MemoryEntry
- 個体帰属付きMemory Archive
- Recall境界
- 変更不能な根底原則
- 一つの固定された正典Narrative Revision系列
- 系の専用Narrative Process
- 宛先未確定Handoff
- 受領EventによるHandoffと後継個体の束縛
- 信頼済みローカルユーザー `local-owner`

### 既存構造へ縮約

- Claim／Evidence／Adjudication: Event種別とTask Projectionへ縮約
- RefusalEvent: Event種別とMemoryEntryへ縮約
- InheritedClaim: Handoffの未確定参照へ縮約
- Decision Context Manifest: v0.1では任意の`source_refs`・`recalled_memory_refs`へ縮約
- Userエンティティ: v0.1では固定`actor_ref = local-owner`へ縮約
- Handoffの宛先更新: Handoff行の変更ではなく受領・束縛Eventへ縮約
- 可変原則の改訂履歴: v0.1ではSystem参照とEventへ縮約

### 延期

- External Root／Bootstrap Plane
- Egress Contract
- 多次元Independence Assessment
- 未捕捉チャネル比較
- タスク分解木への制約伝播
- Narrativeから行為への因果
- 可変原則の改訂アルゴリズム
- 自動忘却・再活性化・隔離・動的想起濃度
- Clock Attestation
- behavior revalidation
- 複数ユーザー・クラウド運用
- モデル固有傾向への一般化
- 外部直接改変の検出・復旧
- Narrative喪失への耐障害性
- 適応型・物理・人間プロファイル
- 系終了後のデータ削除

延期は不要の宣言ではない。現行ケースから要求されない、またはv0.1の中心を漂流させるため、Coreへ入れないという判断である。

## 11. `note/`による空白管理

現時点の未決事項、仮置き、延期事項、未実証事項は、非規範の[`note/`](../note/README.md)で管理する。

### 権威境界

- Noteは正典、参照プロファイル、適合条件を変更しない。
- Note内の選択肢や実装案を、回答がないまま正典へ補完しない。
- `open`、`provisional`、`deferred`、`out_of_scope`、`unvalidated`、`resolved`を区別する。

### 正典への昇格条件

Noteの項目を正典へ反映するには、次をすべて満たす。

1. 明示的な決定が置かれている。
2. 決定の出所が`user_stated`、`user_confirmed`、論理的演繹などとして区別されている。
3. 影響する正典文書、参照プロファイル、不変条件、ケースが特定されている。
4. 既存構造との衝突と、剪定・延期・削除対象が検算されている。
5. Noteの状態と正典へのリンクが更新されている。

> **空白は、決定されるまで空白として管理する。**
