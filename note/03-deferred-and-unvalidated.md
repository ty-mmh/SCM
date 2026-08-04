# 延期・範囲外・未実証

- 状態: non-normative status ledger
- 目的: 「未決」「延期」「範囲外」「未実証」を混同しない
- 最終更新: 2026-08-04

## 1. 延期している概念・機構

### DEF-01 — Narrativeから行為への因果

- 状態: `deferred`
- v0.1ではNarrativeをRecall順位、生成コンテキスト、Task行為へ使用しない。
- 将来論点: `narrative_read`、MemoryEntryへの接続、美学・persona・関係状態への影響、Narrative由来の汚染。

### DEF-02 — 可変原則の改訂アルゴリズム

- 状態: `deferred`
- 可変原則がNarrativeの時制から現れ、系としての決定により変化することだけを固定する。
- 候補生成、発火契機、決定処理、版構造は未定義。

### DEF-03 — 忘却・再活性化・隔離・想起濃度

- 状態: `deferred`
- `recall_weight`、`forgotten_at`、`quarantined_at`、`reactivated_at`は予約属性。
- v0.1の遷移規則・C1〜C5・適合判定へ使用しない。

### DEF-04 — モデル固有傾向への一般化

- 状態: `deferred`
- SCMは根拠資料を保存できるが、一般化規則を持たない。
- DDIS等の評価機構との接続候補。

### DEF-05 — 外部直接改変の検出・真正性証明・復旧

- 状態: `deferred`
- 候補: 更新日時、ハッシュ、署名、追記ログ、二重管理、レプリカ差分。
- 全状態が整合的に改変され、過去の手掛かりがない場合、SCM単独では検出できない。

### DEF-06 — Narrative喪失への耐障害性

- 状態: `deferred`
- Narrative系列喪失後の再生成は同じ系の継続ではない。
- 保存障害、削除、破損、バックアップ復旧、喪失検出は現行保証外。

### DEF-07 — Egress Contract

- 状態: `deferred`
- 外部出力が内部状態より強い確実性・経験帰属を表現しないための境界。
- v0.1では後継個体向けRecall・Handoff Viewを先に実装する。

### DEF-08 — External Root / Bootstrap Plane

- 状態: `deferred`
- 鍵、時計、Verifier、bootstrap policy、信頼の根をSCM外部前提から明示的な保証面へ移す機構。

### DEF-09 — 情報祖先・独立性評価

- 状態: `deferred`
- 複数証拠が同じ情報祖先を共有するかを評価する機構。

### DEF-10 — Task制約伝播

- 状態: `deferred`
- 拒否由来の制約を、再割当・タスク分解・子Taskへ伝播させる機構。

### DEF-11 — Restoration / behavior revalidation

- 状態: `deferred`
- 状態復元と行動回復を分ける。I7とともに延期。

### DEF-12 — General fault localization

- 状態: `deferred`
- 共有記憶汚染、相関故障、全系更新の隔離。I8とともに延期。

---

## 2. 将来プロファイル

### PROFILE-01 — Concurrent Multi-Instance

- 状態: `deferred`
- 複数個体の同時任務、競合、不一致、情報祖先、相関故障、複数記述のNarrative材料化。

### PROFILE-02 — Adaptive Stateful Executor

- 状態: `deferred`
- オンライン学習、永続的局所状態、行動ヒステリシス、方策版、behavior revalidation。

### PROFILE-03 — Embodied / Physical Executor

- 状態: `deferred`
- 個体帰属の機体構成・実状態・診断結果・摩耗・校正・損傷と、外部診断参照。

### PROFILE-04 — Multi-user / Cloud Operation

- 状態: `deferred`
- 認証、所有者・共同構成者・閲覧者・管理者、ライフサイクル権限、可視範囲、監査、移譲。

### PROFILE-05 — Human Organizational Operation

- 状態: `out_of_scope`
- 現時点では非規範の比較対象。人間の同意、責任、退出権、労働倫理をv0.1へ暗黙適用しない。

---

## 3. SCMの範囲外

### OOS-01 — 系終了後のデータ削除

- 状態: `out_of_scope`
- SCMが扱うのは系の終了まで。
- ファイル破棄、DBレコード削除、バックアップ・スナップショット削除、物理媒体処分、フォレンジック復元不能性は扱わない。

---

## 4. 概念・仕様はあるが未実証

### VAL-01 — 参照実装

- 状態: `unvalidated`
- Python／SQLite等の参照実装は未作成。

### VAL-02 — C1〜C5

- 状態: `unvalidated`
- ケース定義はあるが、欠陥実装を含む動的実行は未実施。

### VAL-03 — S1

- 状態: `unvalidated`
- Narrative ProcessがEvent Store・Memory Archiveを書き換えられない権限分離は未実装。

### VAL-04 — 根底原則の変更不能性

- 状態: `unvalidated`
- 正規遷移として変更経路を持たないという仕様はあるが、実装上の制約は未確認。

### VAL-05 — Narrativeの系帰属

- 状態: `unvalidated`
- 解釈・判断を系へ帰属させ、計算実行者の来歴と分けるスキーマ・実行境界は未検証。

### VAL-06 — MemoryEntry自動閉包

- 状態: `unvalidated`
- 行為・非行為から`pending`を作り、結果Eventで`closed`へ移す処理は未実装。

### VAL-07 — 宛先未確定Handoff

- 状態: `unvalidated`
- `to_instance_id = null`で生成し、受領・束縛Eventで後継へ接続する遷移は未実装。

### VAL-08 — `local-owner`起点のライフサイクル

- 状態: `unvalidated`
- 系作成、モデル切替、分岐、終了を`local-owner`以外から開始できない境界は未実装。

### VAL-09 — 文書整合の自動検査

- 状態: `unvalidated`
- GitHub Actionsなし。
- Markdownリンク、旧語彙、スキーマ名、不変条件×ケース対応、README同期の自動検査なし。

### VAL-10 — 独立レビュー

- 状態: `unvalidated`
- 第三者レビュー、実装レビュー、外部再現なし。

### VAL-11 — ライセンス

- 状態: `open`
- private運用中。外部公開前に決定が必要。

---

## 5. v0.1へ戻す条件

`deferred`項目をv0.1 Coreへ戻す場合、次を要求する。

1. 現行プロファイルで必要である。
2. 欠くと失敗するケースまたは構造保証がある。
3. 既存属性・Event・Projectionへ縮約できない。
4. 導入時に削除・縮約・延期できる既存構造を同時に検討する。
5. ユーザーの明示的決定とアシスタント提案が区別されている。
6. 正典・参照プロファイル・ケース・Noteを同期する。
