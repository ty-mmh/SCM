# 個体帰属付き記憶の共有書庫

- 状態: canonical supplement draft
- 対象: MemoryEntryの形成、帰属、保存、想起、矛盾、状態
- 上位定義: [SCM連続性存在論](07-continuity-ontology.md)
- 実装境界: [SCM v0.1生成AIモデル継承プロファイル](02-reference-profile-v0.1.md)

## 0. 定義

**個体帰属付き記憶の共有書庫とは、各個体が自身の行為または非行為から形成した局所記憶を、形成個体への帰属と根拠記録との接続を維持したまま系の内部へ保存し、後続または同時に存在する別個体が、他者の記述として検索・想起・参照できるようにする連続性基盤である。**

ここでいう共有は、記憶の共同所有や経験主体の同一化を意味しない。

| 項目 | 共有されるか |
|---|---|
| 保存場所 | 共有される |
| 検索・読み取り可能性 | 共有される |
| 記憶間の参照経路 | 共有される |
| 経験主体 | 共有されない |
| 記憶の帰属 | 共有されない |

> **記憶の帰属は個体にある。保管と可読性は系が担う。複数個体を通した意味は正典Narrativeが担う。**

系は個体記憶を一つの「系記憶」へ統合しない。矛盾する記憶を含め、形成個体への帰属を保ったまま並存させる。

---

## 1. 記録書庫との違い

記録と記憶は平坦な同一層ではない。

```text
Event Ledger
  外部化された出来事の一次層
        ↓ 参照
Memory Archive
  個体が出来事を局所因果として結んだ二次層
```

Eventは、何が外部化されたかを保持する。

MemoryEntryは、ある個体がその出来事を、状況、外化された判断理由、行為または非行為、結果としてどのように結んだかを保持する。

同じEvent群から、複数個体が異なるMemoryEntryを形成してよい。

```text
同じEvent群
  ├─ 個体Aの記憶: 原因はXだった
  ├─ 個体Bの記憶: 原因はYだった
  └─ 個体Cの記憶: 原因は未確定である
```

書庫はこれらを平均化・多数決・単一見解へ変換しない。

---

## 2. MemoryEntryの定義

**MemoryEntryとは、ある個体が、一つ以上のEventをもとに、状況、外化された判断理由、行為または非行為、結果を局所的な因果として接続した記述単位である。**

MemoryEntryは形成個体へ帰属する。

別個体がそれを読んでも帰属は移動しない。後継個体が過去のMemoryEntryを参照して行為し、結果を得た場合、後継個体へ帰属する別のMemoryEntryが形成される。

```text
Memory M-001
  formed_by: Instance A
        ↓ Instance Bが読む
Bの判断・行為・結果
        ↓
Memory M-002
  formed_by: Instance B
  recalled_memory_refs: [M-001]
```

---

## 3. 自動閉包型の形成

SCM v0.1は、MemoryEntryの形成に**自動閉包型**を採用する。

### 3.1 開始

個体が行為または非行為を選択したとき、SCM Coreはその個体へ帰属する `pending` MemoryEntryを自動生成する。

```text
個体の判断
  ↓
行為または非行為Event
  ↓
pending MemoryEntry
```

行為には次を含む。

- 発話
- 観測
- ツール実行
- 選択
- 拒否
- 保留
- 不採用
- 沈黙
- 何もしないという判断

### 3.2 閉包

結果Eventが接続された時点で、SCM CoreはMemoryEntryを `closed` へ移す。

```text
pending MemoryEntry
  ↓ result_event_refs
closed MemoryEntry
```

結果は外部世界の変化だけではない。

- ツール結果が返った
- 任務が保留された
- 発話しない状態を維持した
- 危険な実行を回避した
- 反証が得られた
- まだ結果を観測できないことが確定した

ことも結果Eventになりうる。

結果が得られていないMemoryEntryは `pending` のままHandoff可能である。

### 3.3 形成主体と書庫管理

- 個体は、状況、外化された判断理由、行為または非行為を生む。
- SCM Coreは、それらをMemoryEntryとして作成し、結果Eventを接続して閉包する。
- 記憶の帰属は行為した個体に残る。
- SCM Coreは記憶の経験主体や解釈主体にはならない。

記憶形成時に「重要だから保存する」という選別は行わない。すべての行為・非行為はMemoryEntryの対象となり、どれを読むかは想起側の問題として扱う。

---

## 4. 行為の起源と判断理由

MemoryEntryに含まれる判断理由は、モデル内部の真の思考過程ではない。

SCMが保持できるのは、外部化・参照可能な次の要素である。

- 当時の状況
- 任務
- 適用された原則
- 参照Event
- 想起されたMemoryEntry ID
- ツール結果
- 個体が外化した判断理由

したがって、判断理由は次のように定義する。

> **行為の真の内的原因ではなく、行為時に参照・提示された状況と理由である。**

`recalled_memory_refs`は、参照された記憶との接続を示す。参照記憶が内部的な真因だったことを証明しない。

---

## 5. MemoryEntryの概念構造

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

  # concept-compatible reserved fields
  recall_weight?
  forgotten_at?
  quarantined_at?
  reactivated_at?

  related_task_id?
```

### 5.1 `system_id`

`system_id`は保存範囲を示す。MemoryEntryの所有主体を系へ変更するものではない。

### 5.2 `formed_by_instance_id`

記憶を形成した経験主体を示す。同じ `model_ref` でも `instance_id` が異なれば別個体である。

### 5.3 `source_event_refs`

記憶の根拠となるEventを示す。根拠が失われた記憶は、由来を確認できない記憶として扱う。

### 5.4 `recalled_memory_refs`

現在個体が行為時に参照したと外化された過去のMemoryEntryを示す。

### 5.5 `closure_state`

因果エピソードが結果Eventまで接続されたかを示す。

- `pending`: 行為または非行為は記録されたが、結果が未接続
- `closed`: 結果Eventが接続済み

### 5.6 `memory_state`

記憶の現在の利用可能性を示す。

- `normal`: 通常の参照対象
- `invalidated`: 誤り、または現在は有効でないと判断された

`closure_state`と`memory_state`は別軸である。たとえば、閉包済みの記憶が後から失効する場合、`closed + invalidated`となる。

---

## 6. v0.1で予約する状態・属性

次はSCM概念との互換性を保つために型へ残せるが、v0.1の遷移規則、C1〜C5、適合判定には使用しない。

- `forgotten`
- `quarantined`
- `recall_weight`
- 自動減衰
- 自動再活性化
- 自動隔離
- 想起回数に基づく動的更新

> **これらは概念互換の予約状態・予約属性であり、v0.1の実装義務ではない。**

v0.1の実動範囲は、原則として次に限定する。

```text
closure_state: pending | closed
memory_state:  normal | invalidated
```

矛盾は状態ではなく、`contradiction_refs`による記憶間関係として保持する。

---

## 7. 忘却・失効・隔離・論理破棄

### 忘却

記憶を保存したまま、通常の想起対象から遠ざける。強い関連や明示的契機によって再び想起しうる。

### 失効

内容が誤り、または現在は有効でないと判断された状態。形成・利用・失効の来歴は保持する。

### 隔離

通常の想起・判断利用から外し、限定経路のみで参照可能にする。

### 論理破棄

活動中の系では、記録・MemoryEntryを通常操作で完全消去しない。

ユーザーが系の終了と破棄を明示した場合、SCMは管理下の論理データを通常の検索・想起・継続操作から除外できる。

物理媒体、バックアップ、複製先を含む復元不能な消去はv0.1の保証対象ではない。

---

## 8. 矛盾する記憶

矛盾するMemoryEntryは一つへ溶かさない。

```text
Memory A: X
Memory B: not X
```

書庫は双方を帰属付きで保持し、相互参照を残す。

現在個体が行為する必要がある場合、現在個体が局所的な暫定判断を行う。その判断、行為、結果は現在個体へ帰属する新しいMemoryEntryになる。

```text
矛盾する記憶群
  ↓ 現在個体Cが読む
Cが今回はXを仮採用
  ↓
行為・結果
  ↓
個体CのMemoryEntry
```

この処理は、矛盾を解消したことにも、系の統合見解を作ったことにもならない。

---

## 9. 想起との関係

Recallは経験を注入する機能ではなく、現在個体が系の書庫から読むべき資料を得るための索引である。

v0.1のRecallItemは、少なくとも次を返す。

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

後継個体は他個体のMemoryEntryを、他者の記述として読む。

許可例:

> 個体Aの記憶では、この条件でツール実行が失敗しています。

禁止例:

> 私は以前、この条件で失敗しました。

v0.1では正典NarrativeをRecall候補選定、順位付け、生成コンテキストへ使用しない。

---

## 10. 正典Narrativeとの境界

MemoryEntryは局所的で、個体へ帰属する。

正典Narrativeは、複数個体の記憶、記録、任務、矛盾、個体交代を一つの系の来歴として意味づける。

正典Narrativeは個体記憶を統合して一つの系記憶にしない。各記憶の帰属と矛盾を保ったまま、どの個体がどう読み、行為し、その後何が起きたかを接続する。

正典Narrative RevisionはMemoryEntryを書き換える権限を持たない。

---

## 11. 定義文

> **SCMにおけるMemoryEntryとは、ある個体が、外部化されたEventをもとに、状況、外化された判断理由、行為または非行為、結果を局所的な因果として接続した記述単位である。MemoryEntryは形成個体に帰属し、SCMの共有書庫へ根拠Eventおよび参照MemoryEntryとの接続を保ったまま保存される。他の個体はそれを他者の記述として想起・参照できるが、自身の直接経験として所有しない。SCM v0.1では行為時にpending MemoryEntryを自動生成し、結果Eventによってclosedへ機械的に閉包する。**
