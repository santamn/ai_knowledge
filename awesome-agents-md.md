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

## 関数型プログラミングスタイルの採用

ref. [yuki氏のツイート](https://twitter.com/helloyuki_/status/2077409675931439272)

```markdown
# General Coding Guide

* Please do not worry about backward compatibility until I provide further instructions.
  * Prefer to make data immutable.
  * Specify three components: Actions, Calculation, Data (This principle is written in the book "Grokking Simplicity"). Specifically, carefully isolate Actions.
    * Actions: Depend on how many times or when it is run. Also called functions with side-effects, side-effecting functions, impure functions. Examples: Send an email, read from a database, including I/O operations.
    * Calculations: Computations from input to output. Also called pure functions, mathematical functions. Examples: Find the maximum number, check if an email address is valid.
    * Data: Facts about events. Examples: The email address a user gave us, the dollar amount read from a bank's API.
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
