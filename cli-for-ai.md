# エージェント用コマンドラインツール

## hooks

- [markdownlint-cli](https://github.com/igorshubovych/markdownlint-cli): AIが変なMarkdownを書かないようにするためのルール
  - 導入方法
    1. `markdownlint-cli` をインストール 
    2. ルールを設定: `~/.claude/hooks/markdownlint/markdownlint.jsonc`
      ```json
      {
        "default": false,

        // 見出しの意味構造
        "MD001": true,                          // ## の次に #### と飛ばさない
        "MD003": { "style": "atx" },            // === や --- の下線見出しを禁止
        "MD022": true,                          // 見出しの前後に空行
        "MD024": { "siblings_only": true },     // 同一階層内での見出し名の重複を禁止
        "MD025": true,                          // H1 は文書に一つ
        "MD036": { "punctuation": "" },         // 太字の見出し代用を禁止

        // 描画が壊れる書き方
        "MD029": { "style": "ordered" },        // 番号付きリストは 1. 2. 3.
        "MD031": true,                          // コードフェンスの前後に空行
        "MD032": true,                          // リストの前後に空行
        "MD040": true,                          // コードフェンスに言語指定
        "MD042": true,                          // 空リンクを禁止
        "MD051": true,                          // #アンカー のリンク先が実在するか検証
        "MD047": true                           // ファイル末尾に改行
      }
      ```
    3. スクリプトを設定: `~/.claude/hooks/markdownlint/script.sh`
      ```sh
      #!/usr/bin/env bash
      set -uo pipefail

      input=$(cat)
      file=$(jq -r '.tool_input.file_path // empty' <<<"$input")
      [[ "$file" == *.md && -f "$file" ]] || exit 0

      config="$HOME/.claude/hooks/markdownlint/markdownlint.jsonc"
      for name in .markdownlint.jsonc .markdownlint.json .markdownlint.yaml .markdownlint.yml; do
        candidate="${CLAUDE_PROJECT_DIR:-$PWD}/$name"
        [[ -f "$candidate" ]] && { config="$candidate"; break; }
      done

      result=$(markdownlint --config "$config" "$file" 2>&1)
      status=$?

      [[ $status -eq 0 ]] && exit 0
      if [[ $status -ne 1 ]]; then
        jq -nc --arg m "markdownlint failed (exit $status): $result" '{systemMessage: $m}'
        exit 0
      fi

      total=$(wc -l <<<"$result" | tr -d ' ')
      {
        echo "markdownlint reported $total issue(s) in $file."
        echo "Fix them without changing what the document says. Do not delete content to satisfy the linter."
        head -n 30 <<<"$result"
        (( total > 30 )) && echo "... and $(( total - 30 )) more"
      } >&2
      exit 2
      ```
    4. `~/.claude/settings.json` に次を追加
      ```json
      {
        "hooks": {
          "PostToolUse": [
            {
              "matcher": "Write|Edit",
              "hooks": [
                {
                  "type": "command",
                  "command": "$HOME/.claude/hooks/markdownlint/script.sh",
                  "timeout": 30
                }
              ]
            }
          ]
        }
      }
      ```

## MCP

- [Serena](https://oraios.github.io/serena/01-about/000_intro.html): AI向けランゲージサーバ
  - `serena setup claude-code` で使えるようになる
  - 注意点: 最新のClaude OpusはSerenaの指示を無視しがちなので、次の二点を実行する
    1. `claude --system-prompt="$(serena prompts print-cc-system-prompt-override)"`
    2. 次を `~/.claude/settings.json` に追加する
    ```json
    {
      "hooks": {
        "PreToolUse": [
          { "matcher": "", "hooks": [{ "type": "command", "command": "serena-hooks remind --client=claude-code" }] },
          { "matcher": "mcp__serena__*", "hooks": [{ "type": "command", "command": "serena-hooks auto-approve --client=claude-code" }] }
        ],
        "SessionStart": [
          { "matcher": "", "hooks": [{ "type": "command", "command": "serena-hooks activate --client=claude-code" }] }
        ],
        "SessionEnd": [
          { "matcher": "", "hooks": [{ "type": "command", "command": "serena-hooks cleanup --client=claude-code" }] }
        ]
      }
    }
    ```

## tool (pulugin)

- [ponytail](https://ponytail.dev/)
