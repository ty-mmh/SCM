# SCM Note Archive

このディレクトリは、空白台帳へ統合する前のNoteと、統合時の対応表を保存する。

## 2026-08-04 統合前のNote

空白台帳へ再編する直前の詳細Noteは、次のコミットに固定して参照できる。

- 基準コミット: [`6e81b7ac7fea99e13fde62fddfa26a422c128bc4`](https://github.com/ty-mmh/SCM/commit/6e81b7ac7fea99e13fde62fddfa26a422c128bc4)
- [概念核に残る空白](https://github.com/ty-mmh/SCM/blob/6e81b7ac7fea99e13fde62fddfa26a422c128bc4/note/01-core-open-questions.md)
- [v0.1実装仕様に残る空白](https://github.com/ty-mmh/SCM/blob/6e81b7ac7fea99e13fde62fddfa26a422c128bc4/note/02-v0.1-open-questions.md)
- [延期・範囲外・未実証](https://github.com/ty-mmh/SCM/blob/6e81b7ac7fea99e13fde62fddfa26a422c128bc4/note/03-deferred-and-unvalidated.md)

## 統合方針

旧Noteとユーザー作成の全量スナップショットは、[`../ledger.md`](../ledger.md)へ次のように統合した。

- ユーザーが存在論的に埋める空白は、6件の`U-*`判断カードへ圧縮。
- 状態遷移・API・DBの詳細は、5件の`E-*`engineeringキューへ集約。
- 文書不整合は`M-*`、来歴監査は`G-*`へ分離。
- 延期・範囲外は`D-*`、未実証は`V-*`へ分離。
- 元の論点がどこへ移ったかは[`2026-08-04-crosswalk.md`](2026-08-04-crosswalk.md)で追跡する。

旧Noteを現行ブランチへ重複保持しない。詳細が必要な場合は、上記固定コミットを参照する。
