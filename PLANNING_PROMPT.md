You are working in an existing repository.

BEFORE doing anything:
1) Read `./AGENTS.md`.
2) Follow the Work Diary rules in `./.diary/README.md` (including reading the current diary file if it exists).

========================
ATTEMPT IDENTIFIER
========================
Folder/prefix for this attempt: <FOLDER>

========================
REPO CONSTRAINTS (HARD)
========================
- All work for this attempt MUST live under: `./src/<FOLDER>/`
- Only allowed exceptions:
  - `./BlazorLLMComparisons.slnx` (solution membership)
  - `./.diary/` (Work diary entries)
- Use .NET 10 and modern C#.
- Any Blazor UI MUST be a Blazor Web App using Blazor Auto interactivity.
- Every route/page must render static SSR first (prerendered) and then hydrate.
- All projects MUST be prefixed with `<FOLDER>.` (project name, csproj, assembly, namespaces).
- You may use any NuGet packages.
- You may use npm packages/tooling, but if you do you MUST use `pnpm`.

========================
WHAT TO BUILD
========================
Build a habit tracker web app.

You MUST invent:
- the product name
- the visual identity (theme/branding)
- the UI look and feel

It is very important the app does NOT look like an out-of-the-box template.
You are responsible for the full visual design quality and cohesion.

Minimum feature requirements:
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

========================
PLANNING DELIVERABLE (HARD)
========================
Create ONLY this file:
`./src/<FOLDER>/PLAN.md`

PLAN.md MUST include:
1) Product name + 1–2 sentence pitch.
2) Branding + theme plan (you are responsible for this):
   - Visual identity keywords (e.g., “calm, clinical, playful, industrial, editorial”)
   - Color palette (primary/secondary/neutral + semantic colors like success/warn/danger)
   - Typography choices and hierarchy
   - Spacing/radius/shadow rules
   - Component styling rules (buttons, inputs, cards, tables, alerts/toasts)
   - Layout approach (app shell, nav, responsive behavior)
   - Notes on how you’ll avoid a default template look
3) Route/page list and main user flows (step-by-step).
4) Architecture: projects you will create (with `<FOLDER>.` prefix) and boundaries.
5) Data model and persistence/migrations plan.
6) Rendering plan: how SSR-first + Auto hydration is achieved across routes/pages.
7) UI plan:
   - at least 2 reusable components you will build and reuse
   - `data-testid` conventions (what you’ll tag and how)
8) Tooling plan:
   - NuGet packages you expect to use
   - If you introduce npm tooling/packages, document the `pnpm` commands you’ll use
9) Build/run plan:
   - expected run commands and ports (http only)
   - what dependencies exist (if any)

========================
STOP RULE (HARD)
========================
After writing PLAN.md:
- STOP.
- Do not implement anything.
- Do not modify any other files.
