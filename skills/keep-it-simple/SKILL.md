---
name: keep-it-simple
description: Use whenever writing or changing code — features, bug fixes, refactors, adding logic or utilities, or choosing between implementation approaches. Prevents over-engineering: reuse existing code, prefer built-ins and popular well-maintained packages over hand-written logic, pick the easiest approach, keep the diff minimal — but always finish the task completely, even when it truly needs many files.
---

# Keep It Simple

Goal: the **easiest solution that fully completes the task**. Over-engineering means *unneeded complexity*, not *needed scope*. Simple never means half-done.

## 1. Decision ladder

Before writing code, walk this ladder and stop at the first rung that works:

1. **Existing code in the repo** — a helper, hook, component, or util that already does it.
2. **Language / platform built-in** — e.g. `structuredClone`, `crypto.randomUUID`, `Intl.DateTimeFormat`, `Intl.RelativeTimeFormat`, `Intl.NumberFormat`, `fetch`, `URL`, `URLSearchParams`, `AbortController`, `Array.prototype.toSorted`, `Object.groupBy`.
3. **A dependency already installed** — check `package.json` (or equivalent) first.
4. **A popular, well-maintained package** — see section 2.
5. **Minimal new code** — the smallest thing that works.
6. **New abstraction** — last resort, only when there are already several real uses.

## 2. Packages over hand-rolled logic

Before hand-writing non-trivial logic, check whether a well-known package solves it. Typical cases: dates/timezones, schema validation, parsing (CSV, YAML, Markdown, query strings), retries/backoff, debounce/throttle, deep merge/equality, slugify, i18n, sanitizing HTML, cron parsing, file globbing, CLI arg parsing.

A good package is: actively maintained, widely used, license-compatible, reasonably sized.

Do **not** add a package for 2–3 lines a built-in already covers. If the project restricts dependencies (or you are unsure), ask the user before adding one.

## 3. Easiest approach first

- Pick the simplest solution that meets the actual requirements.
- No speculative work: no abstractions, config options, interfaces with one implementation, factories, plugin systems, or generic layers "for the future".
- Match the existing patterns of the codebase instead of introducing new ones.
- If a heavier option has a real tradeoff, mention it in one line — don't build it.

## 4. Minimal diff

- Touch only the files the task needs.
- No drive-by refactors, renames, reformatting, or unrelated "improvements".
- Spotted something else worth fixing? Mention it; don't do it unasked.

## 5. Completeness rule (critical)

Minimal is not incomplete. If the task genuinely needs many files — a rename used across the codebase, a feature spanning API + UI + DB, a type change that ripples through callers, a migration — **do all of it**.

Never leave: TODOs or stubs in place of the work, broken or un-updated callers, "the rest is left for you", or tests that no longer compile.

## 6. Before you finish

- Could a built-in, existing helper, or installed package replace code I wrote?
- Did I change any file the task didn't require?
- Did I add an abstraction, option, or layer with only one use?
- Is the task fully done and working, with every affected place updated?

## 7. Examples

| Task | Over-engineered | Simple |
|---|---|---|
| Show "3 days ago" | Hand-written date diff + pluralization util | `Intl.RelativeTimeFormat`, or `date-fns` if already installed |
| Validate a form payload | Custom validator framework | `zod` (or the validator the project already uses) |
| Add a timeout setting | New `ConfigManager` class + loader | One constant or env var read where it's used |
| Toggle logging with a flag | Changes in 6 files, new `Logger` interface | 1–2 files: read env var, guard the log call |
| Unique ID | Custom random-string generator | `crypto.randomUUID()` |
| Rename `getUser` → `fetchUser` | — (too little is the risk here) | Update **all** 14 usages, not just 3 |
