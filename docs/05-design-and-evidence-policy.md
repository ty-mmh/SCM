# 設計更新・剪定・証拠の規則

- 状態: project policy draft
- 目的: レビュー往復が単調増加し、SCM Coreが実装不能になることを防ぐ

## 1. 開発姿勢

SCMは概念文だけで完結しない。共有連続性の構造を実装し、各構造遷移と並行して、その遷移を破るケースを置く。

> **構造なき診断を先行させない。診断なき構造を固定しない。**

ケースは本体の代替ではない。合否判定だけでなく、構造上の不足を発見してCoreへ戻す観測装置として扱う。

## 2. v0.1の中心判定

新しい構造がv0.1 Coreに属するかは、次の問いで判定する。

> 個体Aが消え、個体Bが任務を継続するとき、この構造がなければ、何が失われ、混同され、または不正に継承されるか。

現在の中心は、記録、個体帰属付き記憶、Narrative Revision、Task、Handoffを通じたモデル継承である。

単一個体でも同じ価値を持つ一般的な権威・監査・証拠基盤は、Handoffへ直接必要な最小範囲を除き、Coreから延期する。

## 3. 新規構造の採用条件

新しい独立構造をCoreへ入れるには、次をすべて満たす。

1. Handoff中心の中核現象に必要である。
2. その構造を欠くと失敗するCoreケースまたは構造不変条件がある。
3. 既存構造の属性またはEvent種別へ単純に畳めない。
4. v0.1参照プロファイルの対象内である。
5. 外部の経験的主張へ依存する場合、証拠状態が明示されている。
6. 追加する場合に、何を縮約・延期・削除できるかを同時に検討している。

目的を持たない系、個体への記憶帰属、ユーザー起点のライフサイクル、物語の非書換性は、現在の概念核として扱う。これらと衝突する構造は、新規機能ではなく概念変更として明示的に審議する。

## 4. 剪定条件

次のいずれかに該当する構造は、Coreから縮約・延期・削除する。

- 現在のCoreケースまたは構造不変条件のどれからも参照されない。
- なくても同じケースが同じ意味で通る。
- 既存のより単純な構造へ統合できる。
- 将来プロファイルにしか必要ない。
- 未確認の外部主張だけを根拠に導入された。
- 導入により、個体交代をまたぐ連続性より別問題のほうが大きくなる。
- 二巡連続で、どのCoreケースにも要求されない。

| 操作 | 意味 |
|---|---|
| retain | Coreに残す |
| collapse | 独立構造をやめ、既存属性・Eventへ畳む |
| defer | 将来プロファイル／拡張候補へ移す |
| remove | 現行仕様から除去し、再導入時は採用条件を通し直す |

各レビュー巡の終わりに、追加だけでなくこの四判定を必ず行う。

## 5. ケース集合の統制

- v0.1の動的Coreケースは最大5件。
- ケース追加は、未測定不変条件を初めて測る場合、または既存ケースの交差故障を示す場合に限る。
- 新規追加は、原則として既存ケースの置換または統合を伴う。
- 単なるパラメータ違いはcase variantとして扱う。
- 特定構造を残すためだけに後付けしたケースを認めない。
- 合否条件がないケース、欠陥実装でも失敗しえないケースを数えない。

ケース集合をCoreの唯一の権威にはしない。不変条件、参照プロファイル、ケースの三者を [03-invariant-coverage.md](03-invariant-coverage.md) で対応づける。

系に内在的目的を置かないこと、モデル切替がユーザー起点であることなど、型と権限で閉じる条件は、動的ケース数を増やさず構造検査として扱える。

## 6. 設計主張の証拠レジストリ

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
    local_experiment

  source
  verification_status
  affected_structure
  disposition
```

### 検証状態

```text
unverified
existence_verified
metadata_verified
abstract_verified
fulltext_verified
claim_location_verified
reproduced_locally
independently_reproduced
```

「論文が存在する」と「本文が特定の結果を報告している」と「結果が再現した」を分ける。

### Core採用との接続

次の条件をともに満たす構造は、最初の削除・延期候補になる。

- 根拠が`conversation_hypothesis`または未確認の`external_evidence`だけである。
- 現在のCoreケースまたは構造不変条件から要求されない。

一方、`task_id`と`instance_id`の分離、MemoryEntryの形成個体、User Eventを起点とする切替のように、正典とケースから論理的に要求される構造は、学術文献がなくてもCoreへ置ける。設計上の演繹と経験的主張を混同しない。

## 7. 形成中研究の引用規約

形成中研究を引用するときは、最低限次を一組で固定する。

- タイトル
- 著者
- 永続ID（例: arXiv ID）
- 版または確認日
- existence status
- content status
- reproduction status

文献名やIDの正しさだけで、内容主張を確認済みとみなさない。

## 8. 現在の剪定結果

### Coreへ残す

- Continuity Registry相当のSystem・Instance ID分離
- Append-only Event Ledger
- 個体帰属付きMemoryEntryと共有書庫
- Recall境界
- Taskと局所目的
- Handoff
- 版管理された読み取り専用Narrative Projection
- User起点の切替・終了境界

### 既存構造へ縮約

- Claim／Evidence／Adjudication: Event種別とTask Projectionへ縮約
- RefusalEvent: Event種別へ縮約
- InheritedClaim: HandoffとRecallの出所参照へ縮約
- Decision Context Manifest: v0.1では任意の`source_refs`と`recalled_memory_refs`へ縮約
- 記憶の共同所有: 採用せず、形成個体への帰属と共有可読性へ分解

### 延期

- External Root／Bootstrap Plane
- Egress Contract
- 多次元Independence Assessment
- 未捕捉チャネル比較
- タスク分解木への制約伝播
- 忘却・再想起・隔離解除の自動制御
- 物語が将来判断へ与える因果影響の全面追跡
- Clock Attestation
- behavior revalidation
- 適応型・物理・人間プロファイル
- 完全破棄の実データ検証

延期は不要の宣言ではない。現行ケースから要求されず、v0.1の中心を漂流させるため、Coreへ入れないという判断である。
