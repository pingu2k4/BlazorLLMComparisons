# AGENTS.md

This repository is used to compare coding agents across repeated attempts.
Continuity, isolation, and reproducibility matter more than speed.

You MUST follow the repo boundaries below.

---

## 2) Repo boundaries + isolation (hard)

Each agent attempt will specify a folder name. You MUST:
- put all new work for that attempt under: `./src/<folder-name>/`
- avoid modifying other parts of the repository

Only allowed exceptions:
- `./BlazorLLMComparisons.slnx` (solution membership)

---

## 3) Solution membership (hard)

- The solution file is `./BlazorLLMComparisons.slnx`.
- Every new project you create MUST be added to the solution.

---

## 4) Project naming (hard)

All projects you create MUST be prefixed with `<FOLDER>.`

Examples (if `<FOLDER>` is `Foo`):
- `Foo.Web`
- `Foo.Api`
- `Foo.Domain`
- `Foo.Infrastructure`

This rule applies to:
- project names
- `.csproj` file names
- assembly names
- root namespaces (where applicable)

---

## 5) Blazor rendering requirements (hard)

If you build a Blazor UI:
- It MUST be a Blazor Web App using Blazor **Auto** interactivity.
- Every route/page must:
  - render with **static SSR first** (prerendered HTML response)
  - then hydrate to interactive (Server first, then transition to WASM when available)

Do not disable prerendering.

---

## 6) launchSettings.json rule (hard)

For every ASP.NET project you create:
- `Properties/launchSettings.json` MUST contain ONLY an `"http"` profile.
- No `"https"` profile anywhere.
- `applicationUrl` MUST be `http://localhost:<port>` (choose stable ports and document
  them in your attempt README).

---

## 7) Node tooling rule (hard)

If you introduce any Node tooling or npm packages:
- You MUST use `pnpm` for installation and scripts.

---

## 8) Build cleanliness + readiness (hard)

You MUST NOT stop until:
- the entire repository builds with ZERO warnings and ZERO errors, and
- the attempt is in a state that is ready for the testing stage to begin.
