# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Overview

`relay-demo` is a tiny TypeScript project (ESM, `"type": "module"`) used to try Relay end to end: tasks are created, and a Claude seat runs them on `claude/relay-*` branches. It is demo code, not production code.

## Commands

```bash
pnpm install   # install dependencies (pnpm; lockfile is pnpm-lock.yaml)
pnpm test      # vitest run; the standard check for code tasks
npx tsc        # typecheck (tsconfig has noEmit, strict mode)
```

## Layout

- `src/billing.ts`: `calcInvoice` totals invoices in cents; discount codes `WELCOME10`, `VIP25`; unknown codes ignored.
- `src/auth.ts`: `validateLogin`, intentionally naive (TODOs: email format, min password length, structured `{ field, code }` errors).
- `src/format.ts`: `formatMoney`, USD only (TODO: EUR, GEL).
- `tests/`: vitest tests, one `<module>.test.ts` per source module (currently only `format.test.ts`).
- `docs/api.md`: short API overview; update it when public function behavior changes.
- `.relay/results/`: Relay result files (`<relayTaskId>.json`) written by agent runs.

## Conventions

- TypeScript strict mode; import sources in tests without file extensions (e.g. `../src/format`).
- Money is always integer cents; rounding uses `Math.round`.
- Keep changes within the files a task names; add tests alongside behavior changes.
- Keep `docs/api.md` in sync with code.

## Relay workflow

- Work happens on branches named `claude/relay-<id>`; never push to the default branch or other branches.
- Do not modify files under `.github/`.
- Do not open PRs or comment on GitHub from the agent; Relay handles that.
- Each run ends with a result file in `.relay/results/<relayTaskId>.json` and a commit `relay(<relayTaskId>): <title>`.
