# エージェント用コマンドラインツール

## AGENTS.md

- **ast-grep (`sg`)**: AST-based structural code search and rewriting.
  - *When to use:* When regular expression search is too fragile (e.g., finding syntax patterns regardless of whitespace or formatting).
  - *Examples:*
    - `sg -p 'console.log($ARG)' -l ts` (structural search)
    - `sg -p 'foo($A)' -r 'bar($A)' -U` (in-place AST rewrite)

- **ax**: A local HTTP and HTML I/O utility optimized for AI agents.
  - *When to use:* Performing local web/API requests. Use this instead of writing throwaway curl commands or Python scripts. Run `ax agent-context` first to learn its capabilities.

- **fd (`fdfind`)**: Your primary tool for locating files or directories.
  - *When to use:* Finding specific files by name or extension. Prefer this over `find`. Respects `.gitignore` and uses smart case-matching.
  - *Examples:*
    - `fdfind config`
    - `fdfind -e py` (by extension)
    - `fdfind -t d src` (directories only)
    - `fdfind -H` (includes hidden files)

- **jq**: A command-line JSON processor for filtering, transforming, and extracting data.
  - *When to use:* Parsing or reshaping JSON output from APIs, CLIs, or config files. Prefer this over writing throwaway Python/Node scripts or asking the model to eyeball large JSON blobs. Pipe command output into `jq` to extract only what you need before reading it.
  - *Examples:*
    - `cat package.json | jq '.dependencies'` (extract a field)
    - `ax get localhost:3000/api/users | jq '.[] | {id, name}'` (map array to slim objects)
    - `jq -r '.items[].url' response.json` (raw strings, no quotes — for feeding into other commands)
    - `jq 'select(.status == "failed")' logs.json` (filter by condition)
    - `jq 'keys' config.json` (inspect structure before diving in)

- **ripgrep (`rg`)**: Your primary tool for fast text/regex search across the repository.
  - *When to use:* Searching for strings, patterns, or TODOs. Prefer this over `grep -r`. Respects `.gitignore` by default.
  - *Examples:*
    - `rg 'pattern'`
    - `rg -n --glob '*.ts' 'foo'`
    - `rg -l 'TODO'` (list files only)
    - `rg -F 'literal string'` (disables regex)

- **sem**: Entity-level semantic version control.
  - *When to use:* Analyzing impact, diffs, or git history at the code-entity level (functions, classes, structs) rather than raw lines. Prefer this over `git diff` or `git blame` when structural impact matters.
  - *Examples:*
    - `sem diff` / `sem diff --staged`
    - `sem impact [entity_name]` (simulate impact and dependencies)
    - `sem context [entity_name] --budget 4000` (retrieve token-budgeted context)

## hooks

- [lean-ctx](https://github.com/yvgude/lean-ctx): AIが読み書きするためのコンテキストを管理・圧縮するツール

## MCP

- [firecrawl-cli](https://github.com/firecrawl/cli/): WebページをMarkdownに変換するツール
