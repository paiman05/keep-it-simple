# keep-it-simple

A Claude skill that stops over-engineering.

## Why

Common complaints when coding with Claude:

- It hand-writes logic that a built-in or a popular npm package already solves.
- It picks a hard approach, or changes a pile of files, when an easy one exists.

This skill makes Claude prefer the **easiest solution that fully completes the task**. It does *not* make Claude lazy: if a task truly needs changes across many files, it does all of them. Over-engineering means unneeded complexity, not needed scope.

## What it tells Claude

1. **Decision ladder** — existing code → built-in → installed dependency → popular package → minimal new code → new abstraction (last resort).
2. **Packages over hand-rolled logic** — dates, validation, parsing, retries, etc. — but no package for 2–3 lines a built-in covers.
3. **Easiest approach first** — no speculative abstractions, config options, or layers "for the future".
4. **Minimal diff** — no drive-by refactors.
5. **Completeness rule** — never leave TODOs, stubs, or un-updated callers.

See [`skills/keep-it-simple/SKILL.md`](skills/keep-it-simple/SKILL.md) for the full text.

## Install

### Claude Code plugin (recommended)

```
/plugin marketplace add <owner>/keep-it-simple
/plugin install keep-it-simple@keep-it-simple
```

### Manual

Copy the skill folder into your personal or project skills directory:

```bash
git clone https://github.com/<owner>/keep-it-simple.git
cp -r keep-it-simple/skills/keep-it-simple ~/.claude/skills/
# or, per project:
cp -r keep-it-simple/skills/keep-it-simple .claude/skills/
```

### Claude.ai / Claude Desktop

Zip the `skills/keep-it-simple` folder and upload it under **Settings → Capabilities → Skills**.

## Examples

| Task | Over-engineered | Simple |
|---|---|---|
| Show "3 days ago" | Hand-written date diff util | `Intl.RelativeTimeFormat` |
| Add a timeout setting | New `ConfigManager` class | One constant or env var |
| Toggle logging | 6 files, new `Logger` interface | 1–2 files |
| Rename `getUser` → `fetchUser` | — | Update **all** usages, not just some |

## Contributing

Issues and PRs welcome. Keep `SKILL.md` short — it loads into context.

## License

MIT
