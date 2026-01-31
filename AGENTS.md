# AGENTS.md

This repository is designed for repeated work by coding agents across multiple
sessions. Continuity and reproducibility matter more than speed.

You MUST follow the Work Diary and repo automation conventions below.

---

## 1) Non-negotiable: Work Diary

You MUST read and follow:

- `./.diary/README.md`

It is the source of truth for:
- how to resume work safely
- when/how to write diary entries
- how to name the diary file from the current git branch

### Session start (required)
1) Read `./.diary/README.md`.
2) Determine the current git branch name.
3) Compute the diary filename per rules in `./.diary/README.md`.
4) If the diary file exists, read it fully before doing any work.

### During the session (required)
After each **meaningful action** (code/config change, key decision, important command run,
constraint/bug discovered, tasks created that affect next steps), update the diary.

Do NOT write diary entries for minor intermediate edits.

### Session end (required)
If you performed any meaningful actions, ensure the diary is updated before stopping.
“Next actions” must be accurate and actionable.

If anything conflicts with this file, `./.diary/README.md` wins.

---

## 2) Repo boundaries + isolation (hard)

Each agent attempt will specify a folder name. You MUST:
- put all new work for that attempt under: `./src/<folder-name>/` (`<folder-name>` will be specified in the prompt)
- avoid modifying other parts of the repository

Only allowed exceptions:
- `./src/AppHost/` (Aspire integration)
- `./BlazorLLMComparisons.slnx` (solution membership)
- `./.diary/` (Work diary entries)

---

## 3) Solution + Aspire integration (hard)

### Solution membership
- The solution file is `./BlazorLLMComparisons.slnx`.
- Every new project you create MUST be added to the solution.

### Aspire AppHost
- There is an existing Aspire AppHost project at `./src/AppHost/`.
- You MUST register:
  - every runnable project you add
  - any resources you introduce (databases/caches/queues/etc.)
  - project-to-project dependencies (e.g., UI depends on API, API depends on DB)

### HTTP endpoint registration conventions (hard)
For any project that exposes an HTTP endpoint and is expected to be visited:
- Use endpoint name `"http"` (not `"https"`)
- In AppHost, set URL display text with:
  `.WithUrlForEndpoint("http", url => { url.DisplayText = "..."; })`

For any Blazor UI project:
- In AppHost, it MUST include:
  - `.WithExternalHttpEndpoints()`
  - `.WithExplicitStart()`

---

## 4) launchSettings.json rule (hard)

For every ASP.NET project you create:
- `Properties/launchSettings.json` MUST contain ONLY an `"http"` profile.
- No `"https"` profile anywhere.
- applicationUrl MUST be `http://...` and use a stable dev-local hostname convention.

---

## 5) Mandatory end-to-end verification using agent-browser (hard)

If you implement a web app, you MUST validate key flows using `agent-browser`
while the app is running.

Minimum expected commands:
- `agent-browser open <url>`
- `agent-browser snapshot -i`
- `agent-browser click @eX`
- `agent-browser fill @eX "text"`
- `agent-browser wait --url "**/..."` and/or `agent-browser wait --text "..."`

Tip: `agent-browser --help` shows the full command set.

You must record what you tested and the outcome in the Work Diary.

---

## 6) Build cleanliness (hard)

Before declaring done:
- The entire repository must build with ZERO warnings and ZERO errors.
