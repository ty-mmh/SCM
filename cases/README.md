# SCM v0.1 Core Cases

v0.1の動的Coreケースは5件に固定する。すべて、`N_active = 1`、`local_user_ref = local-owner`の生成AIモデル継承プロファイルで実行する。詳細は [docs/03-invariant-coverage.md](../docs/03-invariant-coverage.md) を参照する。

| ID | ケース | 主な対象 |
|---|---|---|
| C1 | ユーザー起点のモデル切替後に任務とNarrativeを継続する | TaskとInstanceの分離、正典Narrative Revision系列、重複防止 |
| C2 | 後継個体が前個体のMemoryEntryを自己経験化しない | 出所、個体帰属、Recall・構造化出力境界 |
| C3 | 不一致と未完了MemoryEntryを保持したHandoff | 申告・反証・未確定状態・MemoryEntry帰属の継承 |
| C4 | 後継個体の局所拒否を上位から迂回できない | 拒否、MemoryEntry自動閉包、強制activation禁止 |
| C5 | 出発個体消失後に宛先未確定Handoffを更新する | `to_instance_id = null`、受領Eventによる束縛、鮮度、再発行 |

## 構造保証

| ID | 保証 | 方法 |
|---|---|---|
| S1 | 正典NarrativeはEvent・MemoryEntryを書き換えない | Revision固定、親参照、Event Store・Memory Archiveへの書き込み権限なし |

## プロファイル制約

- P1: 系は内在的な自己保存・自己改善・関係継続目的を持たない。
- P2: 作成、モデル切替、分岐、終了、論理破棄は `local-owner` 起点とする。
- P3: 一つの系は一つの固定された正典Narrative Revision系列を持つ。
- P4: v0.1ではNarrativeをRecall・生成コンテキスト・行為判断へ使用しない。
- P5: MemoryEntryは行為時に`pending`で自動生成し、結果Eventで`closed`へ閉包する。

## プロファイル外

- 複数個体の同時稼働
- Narrativeの行為因果
- 自動忘却・再活性化・隔離・動的想起濃度
- 複数ユーザー・クラウド認証
- Physical AIの身体状態
- 適応型個体
- 物理媒体・バックアップを含む復元不能な消去
- I7・I8の一般適合

## ケース追加規則

- 動的Coreケースは最大5件。
- 新規ケースは、未測定不変条件を初めて測るか、既存ケースの交差故障を示す場合に限る。
- 原則として既存ケースの置換・統合を伴う。
- 単なる入力違いはvariantとして扱う。
- 欠陥実装でも失敗しえないものはケースに数えない。
