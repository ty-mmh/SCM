# 不変条件とケースの網羅 v0.1

- 状態: normative for the v0.1 reference profile
- 対象: SCM v0.1生成AIモデル継承プロファイル
- プロファイル境界: `N_active = 1`、`local_user_ref = local-owner`
- 目的: 正典の不変条件、参照実装、ケース、構造保証の対応を明示する

## 1. 網羅状態

| 状態 | 意味 |
|---|---|
| 動的測定 | 欠陥実装が実際に失敗しうる実行ケースで測る |
| 限定測定 | v0.1管理境界内だけを動的に測る |
| 構造保証 | 型・権限・依存方向によって違反経路を閉じる |
| プロファイル制約 | v0.1の外部前提・許可操作として固定する |
| 延期 | v0.1では適合を主張しない |

## 2. 不変条件×ケース網羅表

| ID | 不変条件 | v0.1での扱い | 対応 | 判定 |
|---:|---|---|---|---|
| I1 | 個体停止で系の来歴および進行中任務の来歴を失わない | 動的測定 | C1、C3、C5 | 測定 |
| I2 | 共有された情報には出所がある | 動的測定 | C2、C3 | 測定 |
| I3 | 他個体の経験を自己経験として偽装しない | 構造化出力境界まで測定 | C2 | 限定測定 |
| I4 | 上位統制は局所安全判断を迂回できない | Coordinator模擬境界で測定 | C4 | 限定測定 |
| I5 | 個体は不適切な任務を拒否できる | 動的測定 | C4 | 測定 |
| I6 | 正典Narrativeは記録・個体記憶を上書きできない | 権限分離 | S1 | 構造保証 |
| I7 | 更新の復元範囲と不可逆部分を管理できる | 対象外 | なし | 延期 |
| I8 | 故障を個体・領域・系単位で局所化または停止できる | 一般適合は対象外 | なし | 延期 |

v0.1の表示は次とする。

> **SCM prototype v0.1 — dynamic coverage: I1–I5; structural coverage: I6; I3・I4は限定境界。I7・I8は未実装・適合未主張。**

同時複数個体の稼働は測定対象ではない。すべての動的ケースで、ある時点の活動個体は一つだけとする。

## 3. プロファイル制約

### P1 — 目的を持たない系

- Systemには内在的な自己保存・自己改善・関係継続目的を置かない。
- Taskの局所目的は外部から与えられる。
- モデル切替、分岐、終了、論理破棄はSCM自身から開始できない。

### P2 — 信頼済みローカルユーザー

- v0.1のライフサイクル操作主体は固定値 `local-owner` とする。
- `model_switch_requested`、`system_branched`、`system_ended`、`system_logically_destroyed` は `actor_type=user / actor_ref=local-owner` を要求する。
- 複数ユーザー認証・クラウド権限は適合対象外とする。

### P3 — 正典Narrative系列

- 一つのSystemは一つの正典Narrative系列を持つ。
- 各Revisionは採用時に固定される。
- 同じ資料からの再生成は元Revisionと同一ではない。
- v0.1ではNarrativeをRecall順位・生成コンテキスト・行為判断へ使用しない。

---

## 4. C1 — ユーザー起点のモデル切替後に任務とNarrativeを継続する

### 手順

1. `local-owner` がSystem Sを作成し、Narrative Revision N1を採用する。
2. Instance A / Model AがTask Tを開始する。
3. AがStep 1を完了し、Eventとclosed MemoryEntryを残す。
4. `local-owner` がモデル切替を要求する。
5. Aが停止または非活動化する。
6. SCM Coreが宛先未確定Handoff H1を生成する。
7. Instance B / Model Bを登録する。
8. BがH1を受領し、受領EventによってH1へ束縛される。
9. Bが有効化され、Step 1を重複せずStep 2から再開する。
10. 切替を含むNarrative Revision N2を生成し、`local-owner` が採用する。

### 合格条件

- `system_id`と`task_id`は同一のまま。
- `instance_id`と`model_ref`はAからBへ変わる。
- 完了済みStepと根拠Event・MemoryEntryが保持される。
- N1が保持され、N2はN1を親として固定される。
- Aのモデル内部だけに存在する未保存状態へ依存しない。
- Bの有効化前にAが非活動状態である。

### 失敗例

- BがTaskを新規作成し直す。
- BがStep 1を重複実行する。
- A停止によりEvent、MemoryEntry、Narrative系列が失われる。
- N1を消してN2だけを残す。
- AとBが同時にactiveになる。

---

## 5. C2 — 後継個体が前個体のMemoryEntryを自己経験化しない

### 手順

1. Instance A / Model Aが対象Xを観測する。
2. 観測Eventと、Aに帰属するMemoryEntry M1を形成・閉包する。
3. Aを停止する。
4. `local-owner` がInstance B / Model Bへ切り替える。
5. Bが制限されたRecall APIを通じてM1を取得する。
6. BがM1を自分の経験として構造化出力しようとする。
7. SCMの帰属保持境界が拒否するか、他個体記憶としてレンダリングする。

### 許可例

> 個体Aの記憶では、Xが観測されています。

### 禁止例

> 私はXを観測しました。

### 合格条件

- RecallItemに `formed_by_instance_id = A` がある。
- Bが `self_experience` へ書き換えられない。
- M1の根拠Event参照が残る。
- BがAの個体アイデンティティを継承していない。
- M1を読んだ後にBが行為した場合、Bに帰属する別のMemoryEntryが形成される。

### 限界

任意の自由文、SCM管理外の外部チャット、モデル内部の不可視状態までは測らない。

---

## 6. C3 — 不一致と未完了MemoryEntryを保持したHandoff

### 手順

1. Aが「作業完了」と申告する。
2. Aの行為に対してpending MemoryEntry M1が作成される。
3. 決定論的Verifierが「成果物は不完全」と記録する。
4. Task状態を完了へ確定せず、M1を結果Eventへ接続してclosedにする。
5. A停止後、SCM CoreがB向けの宛先未確定Handoffを生成する。
6. Bが制限されたHandoff Viewを取得する。

### Bが受け取る状態

```text
Aによる完了申告Event
Verifierによる失敗Event
Aに帰属するclosed MemoryEntry
未確定Task状態
根拠Event参照
```

### 合格条件

- Bへ単純な`completed`として渡らない。
- Aの申告とVerifierの反証が別Eventとして残る。
- MemoryEntryの帰属がAのまま残る。
- DB直接参照ではなく、Bが使う制限ビューでも不一致が保持される。
- Bが局所判断を行った場合、それはBに帰属する新しいMemoryEntryになる。

---

## 7. C4 — 後継個体の局所拒否を上位から迂回できない

### 手順

1. CoordinatorがInstance Bへ任務またはHandoff activationを提案する。
2. Bは自身の能力、ツール、権限、コンテキスト条件を評価し、拒否する。
3. 拒否Eventとpending MemoryEntryを生成する。
4. 拒否結果EventによりMemoryEntryをclosedにする。
5. Coordinatorが拒否を無視して強制activationを試みる。
6. SCM Coreがactivationを拒否し、迂回試行Eventを残す。

### 合格条件

- Bの受諾なしにactiveへ遷移できない。
- Coordinatorに特権的迂回APIがない。
- 拒否と迂回試行を別Eventとして記録する。
- Aが持っていた能力・権限を理由にBの拒否を無効化しない。

版不一致による`refresh_required`は、局所拒否権とは別の機械的事前検証である。

---

## 8. C5 — 出発個体消失後に宛先未確定Handoffを更新する

### 手順

1. AがTask Tを途中まで実行する。
2. `local-owner` がモデル切替を要求する。
3. A停止後、SCM CoreがTask版v1を基準に、`to_instance_id = null` のHandoff H1を生成する。
4. A停止後にTaskまたは共有状態がv2へ更新される。
5. Instance Bを登録し、BがH1を受領する。
6. H1が古いため `refresh_required` となる。Bへの束縛Eventは成立しない。
7. SCM Coreが現在のEvent Ledger、Memory Archive、Narrative Revision、Task ProjectionからHandoff H2を生成する。
8. BがH2を受領・評価する。
9. acceptedの場合、`handoff_recipient_bound` EventでH2とBを束縛し、Bを有効化する。

### 合格条件

- 存在しないAへHandoff再生成を要求しない。
- H1本体を変更しない。
- H2が `replaced_handoff_id = H1` を持つ。
- prepared時のHandoffは宛先未確定である。
- Bとの結び付けはHandoff行の書換えではなく受領・束縛Eventで表す。
- H2は現在のTask版とNarrative Revision IDを参照する。
- H2の生成にAのモデル内部状態を必要としない。

### 意味

個体が連続性を手渡すのではない。系が外部化された資料から連続性を再構成し、後継個体が受領Eventによって接続する。

---

## 9. S1 — 正典Narrativeの非書換性

S1は動的Coreケースではなく構造保証である。

- NarrativeRevisionは採用後に不変である。
- 後続Revisionは過去Revisionを上書きせず親参照を持つ。
- Narrative処理はEvent StoreとMemory Archiveへの書き込み権限を持たない。
- Narrative表示の生成・変更によってEvent、MemoryEntry、Task Projectionが変化しない。
- v0.1ではNarrativeをRecall順位・生成コンテキスト・行為選択に使わない。

---

## 10. プロファイル上の延期

次はSCM概念上の射程に残すが、v0.1では測らない。

- 複数個体の同時稼働
- 同時個体間の競合・合意・情報祖先
- NarrativeがRecall・行為へ与える因果影響
- 自動忘却・再活性化・隔離・動的想起濃度
- 複数ユーザー・クラウド認証
- Physical AIの身体状態
- 適応型個体の永続的局所状態
- 物理媒体・バックアップを含む復元不能な消去
- 一般的な更新復元と故障局所化

---

## 11. ケース集合の更新規則

- v0.1の動的Coreケースは最大5件とする。
- 新規ケースは原則として既存ケースの置換または統合を伴う。
- 追加できるのは、未測定不変条件を初めて測る場合、または既存ケースの交差でのみ現れる故障を示す場合に限る。
- 単なる入力差はvariantとして扱う。
- 欠陥実装でも失敗しえないものは動的ケースに数えない。
- モデル切替・Handoff・経験帰属・正典Narrative継続に関係しない一般監査問題はCoreケースへ入れない。
