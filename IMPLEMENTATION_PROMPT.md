You are working in an existing repository.

BEFORE doing anything:
1) Read `./AGENTS.md`.
2) Follow the Work Diary rules in `./.diary/README.md` (including reading the current diary file if it exists).
3) Read and follow: `./src/<FOLDER>/PLAN.md`

========================
ATTEMPT IDENTIFIERS
========================
Folder/prefix for this attempt: <FOLDER>
Kebab-case slug for this attempt: <SLUG>

========================
HARD CONSTRAINTS (MUST COMPLY)
========================
Language/runtime:
- Use .NET 10 and modern C# language features.
- Use Blazor "Auto" render mode for any Blazor UI you build.

Repo boundaries:
- All new work MUST live under `./src/<FOLDER>/`.
- Only allowed exceptions:
  - `./src/AppHost/` (Aspire integration)
  - `./BlazorLLMComparisons.slnx` (solution membership)

Solution membership:
- Every project you create MUST be added to `./BlazorLLMComparisons.slnx`.

Naming:
- All projects MUST start with prefix `<FOLDER>` in:
  - csproj file name
  - assembly name
  - root namespace (where applicable)

launchSettings.json (HARD):
- Every created ASP.NET project MUST have `Properties/launchSettings.json` with ONLY an `"http"` profile.
- No `"https"` profile anywhere.
- `applicationUrl` MUST be `http://...` using:
  `http://<SLUG>-<service>.dev.localhost:<port>`
  (You must keep the service names/ports consistent with PLAN.md.)

Build cleanliness (HARD):
- The entire repo MUST build with ZERO warnings and ZERO errors at the end.

========================
ASPIRE APPHOST INTEGRATION (HARD)
========================
In `./src/AppHost/` you MUST register:
- Every runnable project you created
- Every introduced resource (db/cache/queue/etc.)
- Dependencies between them (project-to-project and project-to-resource)

Rules:
1) HTTP endpoints:
   - Any runnable project that exposes an HTTP endpoint AND is meant to be visited
     MUST include in AppHost:
       `.WithUrlForEndpoint("http", url => { url.DisplayText = "..."; })` (Select a descriptive name for the display text. If this is for a project `FooBar.Api` then might give the display text `Foo Bar API` for example)
   - Always use endpoint name `"http"`.

2) Blazor UI:
   - Any Blazor UI project MUST be registered with:
     `.WithExternalHttpEndpoints()`
     `.WithExplicitStart()`

3) External endpoints guidance:
   - Use `.WithExternalHttpEndpoints()` for projects that you expect humans (or agent-browser)
     to open directly in a browser (UI, admin portals, docs UIs, swagger, etc.).
   - Internal-only services may omit external endpoints, but must still be correctly wired
     and reachable by dependent projects via Aspire references.

4) Dependencies:
   - If one project calls another over HTTP, wire that dependency in AppHost
     (e.g., `.WithReference(otherProject)` and any required config/env wiring).

You MUST follow the exact project/resource list and names from PLAN.md.

========================
IMPLEMENTATION REQUIREMENTS
========================
- Implement every feature described in `./src/<FOLDER>/PLAN.md`.
- Enforce authorization server-side for:
  - workspace scoping
  - role-gated actions
  - article workflow transitions
  - tag administration
  - comment moderation rules
- Safe markdown rendering: prevent script injection.
- Search/filter/sort/pagination must be end-to-end (UI + API/query + persistence).
  No in-memory-only paging/filtering for real lists.
- Use DTOs/contracts across boundaries; do not expose persistence entities directly.

UI/UX & branding:
- Implement the theme/branding plan from PLAN.md so the app looks intentionally designed.
- Avoid default template look; customize styles/components so it feels cohesive.
- Provide loading/empty/error states, responsive layout, keyboard-friendly forms,
  focus management, and confirm destructive actions.

Observability:
- Structured logging.
- Health endpoint(s).

Documentation (required):
- Add `./src/<FOLDER>/README.md` documenting:
  - Product name + what it does
  - Setup steps (migrations/init/seed, default accounts if any)
  - Key architecture choices and tradeoffs
  - Theme/branding notes (what you did to make it unique)
- Add `./src/<FOLDER>/TESTING.md` documenting:
  - how to run the app (via AppHost)
  - the exact E2E checks you performed
  - any screenshots captured and where they are stored

========================
MANDATORY E2E VERIFICATION USING agent-browser (HARD)
========================
You MUST validate the core flows using `agent-browser` while the app is running.

Use `agent-browser --help` if needed. Minimum expected commands:
- `agent-browser open <url>`
- `agent-browser snapshot -i`
- `agent-browser click @eX`
- `agent-browser fill @eX "text"`
- `agent-browser wait --url "**/..."` and/or `agent-browser wait --text "..."`

You MUST execute and document tests that cover at least:
- register + sign in/out
- create workspace + switch workspace UX
- invite flow + accept flow
- invalid/expired invite token UX
- article authoring + workflow transitions + revision viewing
- tag directory + owner-only tag admin
- comments + moderation rules
- bookmarks + bookmarks page

(You decide the exact pages/URLs based on your implementation; it must match PLAN.md.)

========================
HOW TO RUN FOR E2E TESTING (CHOOSE ONE MODE)
========================
You may run the system in EITHER of these modes. Pick the one that matches your
architecture and document which mode you used in `./src/<FOLDER>/TESTING.md`
(including exact commands and URLs).

MODE A (preferred): Run via Aspire AppHost
- Start the orchestrator:
  - `dotnet run --project ./src/AppHost/AppHost.csproj`
- Wait until the Aspire dashboard is available, then use `agent-browser` to:
  1) Open the Aspire dashboard URL.
  2) Find the Blazor UI project in the dashboard and start it.
     - IMPORTANT: Blazor UI projects are registered with `.WithExplicitStart()`,
       so they will NOT start until you press the Start/Run control in the dashboard.
  3) Start any other runnable projects that are not auto-started (if needed).
  4) Open the UI endpoint (and any other visitable endpoints you need for testing)
     and perform the E2E flows using `agent-browser`.

Notes:
- If your solution exposes multiple HTTP endpoints, use the display text configured
  via `.WithUrlForEndpoint("http", ...)` to identify the correct one(s).
- Prefer testing through the UI, but if your system includes other visitable HTTP
  surfaces (admin portals, docs UIs, etc.) you may open them too.

MODE B: Run projects manually in separate processes
- Run each runnable project in its own terminal/process (plus any required resources).
- Use the http launch profile and http URLs only.
- Example pattern (you MUST substitute your actual project paths/names):
  - `dotnet run --project ./src/<FOLDER>/<ProjectFolder>/<ProjectName>.csproj --launch-profile http`
- Ensure all required dependencies are running (e.g., database/cache/queue) and that
  connection strings/service discovery are configured appropriately for this mode.
- Then perform the E2E flows using `agent-browser` against the UI’s http URL.

Hard requirement in both modes:
- You MUST execute and document E2E verification using `agent-browser`.
- You MUST ensure the UI is actually running before starting browser automation.

========================
EXECUTION RULES FOR YOUR RESPONSE
========================
1) Start by summarizing the plan you are about to implement and list the steps you will perform.
2) Implement fully:
   - Create projects, code, config, migrations, scripts/tooling needed.
   - Wire everything through Aspire AppHost as planned.
   - Add all created projects to the `.slnx`.
3) Verify:
   - clean build: zero warnings/errors
   - E2E flows via agent-browser, recorded in `./src/<FOLDER>/TESTING.md`
4) Provide exact commands to build/run/verify.
5) Output a concise “where to look” map for key files.

Output format in your final response:
- "What changed" summary
- File/folder tree under `./src/<FOLDER>/`
- Requirement-to-implementation callouts (bullets with file references)
- Commands to run and verify (including agent-browser examples)
- Where-to-look map (auth, persistence, API boundary, reusable UI components, styling/theme, Aspire config)

Stop when implementation is complete.
