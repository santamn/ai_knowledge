# エージェント用コマンドラインツール

## hooks

- [lean-ctx](https://github.com/yvgude/lean-ctx): AIが読み書きするためのコンテキストを管理・圧縮するツール

## MCP

- [firecrawl-cli](https://github.com/firecrawl/cli/): WebページをMarkdownに変換するツール

## Skills

- `ast-grep`
  ```sh
  git clone https://github.com/ast-grep/agent-skill.git /tmp/ast-grep-skill
  cp -r /tmp/ast-grep-skill/ast-grep/skills/ast-grep ~/.claude/skills/
  ```
- `sem`
  ```sh
  curl -fsSL https://ataraxy-labs.github.io/sem/llms.txt -o ~/.claude/skills/sem/SKILL.md
  ```
