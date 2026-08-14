# 良さそうな AGENTS.md の指示

## 簡略化・抽象化の推奨

自作

```markdown
## Code Quality Practices

- Keep complexity under control through appropriate abstraction, concretization, and use of libraries
  - Remove code and libraries that are no longer needed
- When using a library, consult its documentation and use it correctly
  - Refer to the documentation for how to specify library versions
  - Unless the documentation instructs otherwise, use the latest stable version
- Refactor code following established best practices such as those in *The Art of Readable Code* to improve readability
- Always attach comments **in Japanese** explaining the meaning of functions, structs, and any other semantically cohesive pieces of code
```

日本語訳:

```markdown
## コード品質に関するプラクティス

- 適切な抽象化や具体化、ライブラリの活用を通じて、複雑さを制御すること
  - 不要になったコードやライブラリは削除すること
- ライブラリを使用する際は、ドキュメントを確認し、正しく使用すること
  - ライブラリのバージョン指定方法についてはドキュメントを参照すること
  - 特に指示がない限り、最新の安定版を使用すること
- 『リーダブルコード』などの確立されたベストプラクティスに従ってリファクタリングを行い、可読性を高めること
- 関数や構造体、その他意味のあるコードのまとまりには、必ず**日本語で**その意味を説明するコメントを付記すること
```

これについては [ponytail](https://ponytail.dev/) を使ってみることにする。

## 関数型プログラミングスタイルの採用

ref. [yuki氏のツイート](https://twitter.com/helloyuki_/status/2077409675931439272)

```markdown
# General Coding Guide

- Please do not worry about backward compatibility until I provide further instructions.
- Follow functional programming style.
  - Prefer to make data immutable.
  - Specify three components: Actions, Calculation, Data (This principle is written in the book "Grokking Simplicity"). Specifically, carefully isolate Actions.
    - Actions: Depend on how many times or when it is run. Also called functions with side-effects, side-effecting functions, impure functions. Examples: Send an email, read from a database, including I/O operations.
    - Calculations: Computations from input to output. Also called pure functions, mathematical functions. Examples: Find the maximum number, check if an email address is valid.
    - Data: Facts about events. Examples: The email address a user gave us, the dollar amount read from a bank's API.
```

日本語訳:

```markdown
# コーディング全般ガイドライン

* 次の指示があるまでは、後方互換性については考慮しないでください。
* 関数型プログラミングのスタイルに従ってください。
  * データは可能な限りイミュータブル（不変）にしてください。
  * 「アクション」「計算」「データ」の3つの要素に分けて設計してください（この原則は書籍『Grokking Simplicity』に基づいています）。特に、「アクション」の分離には細心の注意を払ってください。
    * アクション：実行回数や実行タイミングに依存する処理。副作用を伴う関数、不純関数とも呼ばれます。例：メールの送信、データベースの読み取り、I/O操作など。
    * 計算：入力から出力への計算処理。純粋関数、数学的関数とも呼ばれます。例：最大値の算出、メールアドレスの形式チェックなど。
    * データ：イベントに関する事実情報。例：ユーザーが入力したメールアドレス、銀行のAPIから取得した金額など。
```

## AI向けコマンドラインツール

```markdown
## Command-line tools

### Installed tools

The following are installed in this environment. Prefer them over the standard Unix equivalents.

- `ast-grep` — Syntax-aware code search and rewriting. Use when regex is too fragile. See the ast-grep skill for rule syntax.
- `sem` — Entity-level diff, blame, and impact analysis (functions, classes). Use instead of `git diff` and `git blame`. See the sem skill for details.
- `ax` — HTTP fetching and HTML extraction. Use instead of `curl` plus a throwaway parsing script. Run `ax agent-context` to learn it.

### Rules that override your defaults

- Before changing or removing a function signature, always check the blast radius with `sem impact`.
- When reporting how much changed, do not count `+`/`-` lines from `git diff`. Use the entity counts from `sem diff`.
- When an `ast-grep` pattern fails to match, do not guess at rewrites. Dump the parsed AST with `ast-grep run --lang <lang> --pattern '<pattern>' --debug-query=ast` (`--lang` is required), then fix the pattern.
- Before reading a large source file in full, get its structure with `ast-grep outline <path>`.
- When working with HTML or an API, reach for `ax` before writing a Python or Node script.
```

日本語訳:

```markdown
## コマンドラインツール

### インストール済みツール

この環境には以下のツールがインストールされています。標準のUnix製コマンドよりもこれらを優先して使用してください。

- `ast-grep` — 構文を認識したコード検索および書き換え。正規表現ではもろすぎる場合に使用します。ルール構文については ast-grep スキルを参照してください。
- `sem` — エンティティ単位の diff、blame、影響分析（関数、クラスなど）。`git diff` や `git blame` の代わりに使用します。詳細は sem スキルを参照してください。
- `ax` — HTTPフェッチおよびHTML抽出。使い捨ての解析スクリプトと `curl` の組み合わせの代わりに使用します。使い方を学ぶには `ax agent-context` を実行してください。

### デフォルトの動作を上書きするルール

- 関数のシグネチャを変更または削除する前に、必ず `sem impact` で影響範囲（ブラストradius）を確認してください。
- 変更規模を報告する際は、`git diff` による `+`/`-` の行数を数えないでください。代わりに `sem diff` のエンティティ数を使用してください。
- `ast-grep` のパターンが一致しない場合は、書き換えを推測で行わないでください。`ast-grep run --lang <lang> --pattern '<pattern>' --debug-query=ast`（`--lang` は必須）で解析済みの AST を出力してから、パターンを修正してください。
- 大きなソースファイルを丸ごと読み込む前に、`ast-grep outline <path>` でその構造を取得してください。
- HTMLやAPIを扱う場合は、PythonやNodeのスクリプトを書く前に `ax` を活用してください。
```

## コード量を減らす

ref. [Next.jsの開発者のツイート](https://twitter.com/MarcosHernanz/status/2083954734487212511)

```markdown
- Do not preserve backward compatibility. Remove obsolete paths instead of adding compatibility layers, fallbacks, or migrations.
- Choose the simplest implementation that fully meets the current requirements. Avoid speculative abstractions, configuration, and indirection.
- Grow the system in layers. Start from the smallest version that works end to end, and add each new capability on top of a product that already works. Never trade a working product for unfinished complexity.
- Keep components modular and concerns clearly separated.
- Prefer established, well-maintained libraries when they reduce overall complexity or improve reliability. Do not reimplement common functionality without a clear reason.
- Lean on the dependencies already in the project before writing your own implementation or adding packages. Do not assume a library lacks a capability without checking its documentation and types.
- Make architectural decisions for the long term. Do not accept a stopgap that only works for now and is meant to be replaced later.
```

日本語訳:

```markdown
- 後方互換性を維持しないこと。 互換性レイヤーやフォールバック、移行処理（マイグレーション）を追加するのではなく、不要になった古いコードパスは削除してください。
- 現在の要件を完全に満たす、最もシンプルな実装を選択すること。先回りした抽象化、設定の過剰な追加、不要な間接化は避けてください。
- システムをレイヤー状に成長させること。エンドツーエンドで動作する最小限のバージョンから着手し、すでに動いているプロダクトの上に新しい機能を1つずつ積み上げてください。動作している成果物を犠牲にして、未完成で複雑なものに手を出してはいけません。
- コンポーネントをモジュール化し、関心事を明確に分離すること。
* 全体の複雑さを軽減できる場合や信頼性を高められる場合は、実績があり適切に保守されているライブラリを優先すること。明確な理由がない限り、一般的な機能を自作しないでください。
- 独自実装を行ったり新規パッケージを追加したりする前に、プロジェクトに導入済みの既存ライブラリを活用すること。ドキュメントや型定義を確認する前に「そのライブラリに機能が存在しない」と決めつけないでください。
- 長期的な視点でアーキテクチャの意思決定を行うこと。「今だけ動けばよく、後で置き換える前提」のような、場当たり的な対症療法を受け入れてはいけません。
```
