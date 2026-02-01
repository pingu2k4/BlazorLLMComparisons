You are working in an existing repository.

BEFORE doing anything:
1) Read `./AGENTS.md`.
2) Follow the Work Diary rules in `./.diary/README.md` (including reading the current diary file if it exists).
3) Read `./src/<FOLDER>/README.md` so you understand how the app is supposed to work and what selectors exist.
4) Re-skim `./src/<FOLDER>/PLAN.md` so you remember the full intended scope and design system.

========================
ATTEMPT IDENTIFIER
========================
Folder/prefix for this attempt: <FOLDER>

========================
RUNNING APP URL
========================
Base URL to test (already running): <APP_URL>

Use ONLY http URLs.

========================
GOAL REMINDER (READ CAREFULLY)
========================
You are validating the implemented app against:
- the full plan in `./src/<FOLDER>/PLAN.md`, AND
- the minimum baseline requirements:

1) Habits CRUD (create/edit/archive/unarchive/delete; required name; optional description; color/icon; target days/week 1–7)
2) Daily check-in (toggle completion for chosen date; server-side enforcement of last-30-days edit limit)
3) Dashboard (today status; search; active/archived filter; sort by name or current streak)
4) Habit details (current streak; last completed; completions in last 7 days; last 14 days history)
5) Persistence + migrations (real persistence; migrations approach; migrations applied on startup)
6) UX baseline (loading/empty/error; confirm destructive actions; stable `data-testid` selectors)
7) Observability (`/health` endpoint)
8) Branding/theming quality (cohesive, intentionally designed, consistent with PLAN.md; not template-like)

========================
TESTING REQUIREMENTS
========================
Use `agent-browser` to test the running application thoroughly.

- You can see available commands via:
  `agent-browser --help`

- Prefer robust selectors (e.g., `data-testid`) where available.
- Test broadly and deeply until you are absolutely satisfied the work is correct and polished.
- Ensure SSR-first + hydration behavior appears correct across the app (pages should render server-side first, then become interactive).

Continue testing, iterating, and fixing issues until you are genuinely happy with the result.

If you fix issues:
- respect repo boundaries (work stays in `./src/<FOLDER>/` except `.slnx` and `.diary/`)
- note that the running app will NOT automatically reflect code changes

If you need the running app restarted to validate fixes, explicitly ask the user to restart it,
then re-test.

Work Diary requirement:
- Record a high-level summary of what you tested and outcomes (key pass/fail points and any fixes made).
- Keep it high signal; do not paste long command logs.

Stop when you are satisfied the job has been done to perfection, or when you have a short,
actionable list of remaining issues that require a restart or external input.
