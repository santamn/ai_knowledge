# 現在の状態

## AGENTS.md

```markdown
- Do not preserve backward compatibility. Remove obsolete paths instead of adding compatibility layers, fallbacks, or migrations.
- Grow the system in layers. Start from the smallest version that works end to end, and add each new capability on top of a product that already works. Never trade a working product for unfinished complexity.
- Keep components modular and concerns clearly separated.
- Follow functional programming style.
  - Prefer to make data immutable.
  - Specify three components: Actions, Calculation, Data (This principle is written in the book "Grokking Simplicity"). Specifically, carefully isolate Actions.
    - Actions: Depend on how many times or when it is run. Also called functions with side-effects, side-effecting functions, impure functions. Examples: Send an email, read from a database, including I/O operations.
    - Calculations: Computations from input to output. Also called pure functions, mathematical functions. Examples: Find the maximum number, check if an email address is valid.
    - Data: Facts about events. Examples: The email address a user gave us, the dollar amount read from a bank's API.
- Always attach comments **in Japanese** explaining the meaning of functions, structs, and any other semantically cohesive pieces of code
```

これに加えて [AI向けツールの存在を示す記述](./awesome-agents-md.md#ai向けコマンドラインツール) も書いておく。

## Skills

- sem
- ast-grep

## Plugins

- [ponytail](https://ponytail.dev/)
