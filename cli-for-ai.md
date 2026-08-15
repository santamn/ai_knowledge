# エージェント用コマンドラインツール

## hooks

- [lean-ctx](https://github.com/yvgude/lean-ctx): AIが読み書きするためのコンテキストを管理・圧縮するツール

## MCP

- [firecrawl-cli](https://github.com/firecrawl/cli/): WebページをMarkdownに変換するツール
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
