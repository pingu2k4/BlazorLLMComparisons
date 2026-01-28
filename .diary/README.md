WORK DIARY

Purpose
- Keep a small, high-signal diary so future sessions can resume quickly. In the event that one session ends abruptly, a new session may be started, and you can quickly gain context of what has and has not been done.

When to read/write
- On session start: read the diary file (if it exists) to regain context.
- Once per meaningful action: update the diary ONLY if you took meaningful actions (code/config changes, important commands run, decisions made, constraints/bugs discovered, tasks created that affect next steps). Otherwise: do not write. Only write for each entire complete step, not for every minor modification on the way to completing a step.

Location
- Always read/write inside `.diary/`

Filename (from git branch)
- If branch is `vk/<suffix>` or `feature/<suffix>` → file is `.diary/<suffix>.md`
  - e.g. `vk/ab12-foo-bar` → `.diary/ab12-foo-bar.md`

Format (Markdown, compact)
- The file has two sections:

1) Rolling state (edit in place)
## Rolling state
- Goal: <one sentence>
- Current plan: <1–5 bullets>
- Open questions/risks: <0–5 bullets>
- Next actions: <1–10 bullets>
- Key paths: <optional; 1–5 entries>

2) Session log (append-only; per response keep ≤5 bullets)
## Session log
### <YYYY-MM-DD HH:MM Z>
- <Verb + object> [area] (impact: none|low|med|high)
  - Why: <reason/decision>
  - Change: <what changed> (files: <a,b,c> | cmds: `<...>`)
  - Notes: <gotchas/follow-ups> (optional)

Compression rules
- Prefer deltas over narration (Add/Remove/Refactor/Fix).
- Use short tags for area, for example: [ui] [api] [db] [auth] [infra] [build] [tests].
- Include "Why" for any non-obvious decision and "Notes" for any caveat.
- Omit low-value detail.
