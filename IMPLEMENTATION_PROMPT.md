You are working in an existing repository.

BEFORE doing anything:
1) Read `./AGENTS.md`.
2) Follow the Work Diary rules in `./.diary/README.md` (including reading the current diary file if it exists).
3) Read and follow: `./src/<FOLDER>/PLAN.md`

========================
ATTEMPT IDENTIFIER
========================
Folder/prefix for this attempt: <FOLDER>

========================
GOAL REMINDER (READ CAREFULLY)
========================
You are implementing the full app as planned in `./src/<FOLDER>/PLAN.md`.

Minimum baseline requirements the finished system MUST satisfy (in addition to PLAN.md):
1) Habits CRUD
   - Create, edit, archive/unarchive, delete.
   - Fields: Name (required), Optional description, Color (or icon), TargetDaysPerWeek (1–7).
2) Daily check-in
   - For each habit, toggle completion for a chosen date (default: today).
   - Allow edits only for dates in the last 30 days (enforce server-side).
3) Dashboard
   - Shows all active habits with today's status.
   - Search by name and filter (Active / Archived).
   - Sort by Name or CurrentStreak.
4) Habit details
   - Computed stats: Current streak, last completed date, completions in last 7 days.
   - History list/table for the last 14 days.
5) Persistence
   - Use real persistence (you choose; SQLite + EF Core is acceptable).
   - Include a migrations approach and ensure the app applies migrations on startup.
6) UX baseline
   - Loading/empty/error states for key pages.
   - Confirm destructive actions (delete habit, clear a check-in).
   - Add stable selectors on key UI elements (e.g., `data-testid`) to enable robust browser automation later.
7) Observability
   - Health endpoint at `/health`.
8) Branding/theming ownership
   - You are responsible for a cohesive, intentionally designed UI (not an out-of-the-box template),
     implemented according to your PLAN.md theme/branding section.

========================
HARD CONSTRAINTS (MUST COMPLY)
========================
Repo boundaries:
- All new work MUST live under `./src/<FOLDER>/`.
- Only allowed exceptions:
  - `./BlazorLLMComparisons.slnx`
  - `./.diary/`

Solution membership:
- Every project you create MUST be added to `./BlazorLLMComparisons.slnx`.

Project naming (HARD):
- All projects MUST be prefixed with `<FOLDER>.` in:
  - project name
  - csproj file name
  - assembly name
  - root namespace (where applicable)

Runtime/rendering (HARD):
- Use .NET 10 and modern C#.
- Any Blazor UI MUST be a Blazor Web App using Blazor Auto interactivity.
- Every route/page must render static SSR first (prerendered HTML) and then hydrate.
- Do not disable prerendering.

Packages/tooling:
- You may use any NuGet packages.
- You may use npm packages/tooling, but if you do you MUST use `pnpm`.

launchSettings.json (HARD):
- Every created ASP.NET project MUST have `Properties/launchSettings.json` with ONLY an `"http"` profile.
- No `"https"` profile anywhere.
- `applicationUrl` MUST be `http://localhost:<port>` with stable ports documented in README.

Build cleanliness + readiness (HARD):
- You MUST NOT stop until the entire repository builds with ZERO warnings and ZERO errors.
- Keep working until ALL warnings/errors are resolved (including analyzers).
- The attempt MUST be ready for the testing stage to begin (runnable, documented, consistent).

========================
IMPLEMENTATION REQUIREMENTS
========================
Implement the app described in `PLAN.md`, ensuring the minimum baseline requirements above are fully met.

Branding/theming (required and important):
- Implement the full branding + theme plan from `PLAN.md` to a high standard.
- The app should look intentionally designed and cohesive (not like an out-of-the-box template).
- Apply the design system consistently across pages and states (loading/empty/error, dialogs, forms, lists).

Documentation (required):
- Add `./src/<FOLDER>/README.md` that includes:
  - Product name + what it does
  - Which project(s) need to be run (and in what order if >1)
  - Dependencies (e.g., database, connection strings, migration behavior)
  - Ports/URLs used by each runnable project (http only)
  - Any one-time setup steps
  - Notes on how SSR-first + Auto hydration is achieved
  - If npm tooling/packages are used: the `pnpm` commands used and where assets/build outputs live

========================
VERIFICATION (HARD)
========================
Before finishing:
1) Build the entire repository and ensure ZERO warnings and ZERO errors.
2) If issues exist, keep fixing until clean. Do not stop early.

In your final response, include:
- What changed (concise)
- File tree under `./src/<FOLDER>/`
- Exact commands to build and run (http only)
- Where to look for: persistence/migrations, UI pages, reusable components, styling/theme, health endpoint

Stop only when everything is clean and ready for the testing prompt.
