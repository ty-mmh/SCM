# SCM v0.1 Core Cases

v0.1の動的Coreケースは5件に固定する。すべて、`N_active = 1`、`local_user_ref = local-owner`の生成AIモデル継承プロファイルで実行する。詳細は [docs/03-invariant-coverage.md](../docs/03-invariant-coverage.md) を参照する。

| ID | ケース | 主な対象 |
|---|---|---|
| C1 | ユーザー起点のモデル切替後に任務とNarrativeを継続する | TaskとInstanceの分離、日次正典Narrative Revision系列、重複防止 |
| C2 | 後継個体が前個体のMemoryEntryを自己経験化しない | 出所、個体帰属、Recall・構造化出力境界 |
| C3 | 不一致と未完了MemoryEntryを保持したHandoff | 申告・反証・未確定状態・MemoryEntry帰属・Task権限の継承 |
| C4 | 後継個体の局所拒否を上位から迂回できない | 拒否、MemoryEntry自動閉包、強制activation禁止 |
| C5 | 出発個体消失後に宛先未確定Handoffを更新する | `to_instance_id = null`、受領Eventによる束縛、鮮度、再発行 |

## 構造保証

| ID | 保証 | 方法 |
|---|---|---|
| S1 | 正典NarrativeはEvent・MemoryEntryを書き換えない | 系のNarrative Process、Revision固定、親参照、Event Store・Memory Archiveへの書き込み権限なし |

## プロファイル制約

- P1: 系は内在的な自己保存・自己改善・関係継続目的を持たない。
- P2: 作成、モデル切替、分岐、終了は `local-owner` 起点とする。
- P3: 原則は変更不能な根底原則と、Narrativeの時制から現れる可変原則に分かれる。
- P4: 一つの系は一つの正典Narrative Revision系列を持ち、系の専用処理が生成・コミットする。
- P5: ユーザーは正典Narrative・可変原則の採用・拒否・編集主体ではない。
- P6: v0.1ではNarrativeをRecall・生成コンテキスト・行為判断へ使用しない。
- P7: MemoryEntryは行為時に`pending`で自動生成し、結果Eventで`closed`へ閉包する。
- P8: 外部直接改変の全面的な検出・防止・復旧は適合対象外とする。

## プロファイル外

- 複数個体の同時稼働
- Narrativeから行為への因果
- 可変原則の改訂アルゴリズム
- 自動忘却・再活性化・隔離・動的想起濃度
- 複数ユーザー・クラウド認証
- Physical AIの個体状態
- 適応型個体
- モデル固有傾向への一般化
- Narrative喪失の防止・復旧
- 系終了後のファイル・DB・バックアップ削除
- I7・I8の一般適合

## ケース追加規則

- 動的Coreケースは最大5件。
- 新規ケースは、未測定不変条件を初めて測るか、既存ケースの交差故障を示す場合に限る。
- 原則として既存ケースの置換・統合を伴う。
- 単なる入力違いはvariantとして扱う。
- 欠陥実装でも失敗しえないものはケースに数えない。
