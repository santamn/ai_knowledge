# 現在の状態

## AGENTS.md

```markdown
## General Coding Guide

- Do not preserve backward compatibility. Remove obsolete paths instead of adding compatibility layers, fallbacks, or migrations.
- Follow functional programming style.
  - Prefer to make data immutable.
  - Specify three components: Actions, Calculation, Data (This principle is written in the book "Grokking Simplicity"). Specifically, carefully isolate Actions.
- Always attach comments **in Japanese** explaining the meaning of functions, structs, and any other semantically cohesive pieces of code

## Command-line tools

### Installed tools

The following are installed in this environment. Prefer them over the standard Unix equivalents.

- `ast-grep` — Syntax-aware code search and rewriting. Use when regex is too fragile. See the ast-grep skill for rule syntax.
- `ax` — HTTP fetching and HTML extraction. Use instead of `curl` plus a throwaway parsing script. Run `ax agent-context` to learn it.
- `fd` — File and directory search. Use instead of `find`.
- `rg` (ripgrep) — Text and regex search. Use instead of `grep -r`.
- `sem` — Entity-level diff, blame, and impact analysis (functions, classes). Use instead of `git diff` and `git blame`. See the sem skill for details.

### Rules that override your defaults

- Before changing or removing a function signature, always check the blast radius with `sem impact`.
- When reporting how much changed, do not count `+`/`-` lines from `git diff`. Use the entity counts from `sem diff`.
- Before reading a large source file in full, get its structure with `ast-grep outline <path>`.
- When working with HTML or an API, reach for `ax` before writing a Python or Node script.
```

## Skills

- sem
- ast-grep
- [caveman](https://caveman.so/products/caveman)

## Rules

- [ponytail](https://ponytail.dev/)

## MCP

- [Serena](https://oraios.github.io/serena/01-about/000_intro.html)
