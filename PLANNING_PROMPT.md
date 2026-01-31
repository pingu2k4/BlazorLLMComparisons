You are working in an existing repository.

BEFORE doing anything:
1) Read `./AGENTS.md`.
2) Follow the Work Diary rules in `./.diary/README.md` (including reading the current diary file if it exists).

========================
ATTEMPT IDENTIFIERS
========================
Folder/prefix for this attempt: <FOLDER>

Kebab-case slug for this attempt: <SLUG>
- <SLUG> MUST be the kebab-case version of <FOLDER> (example: FooBar -> foo-bar).
- You must use <SLUG> as a prefix when naming Aspire resource names and dev-local hostnames.

========================
REPO CONSTRAINTS (HARD)
========================
- Solution file: `./BlazorLLMComparisons.slnx`
  - Any project(s) you create MUST be added to this solution.
- All work for this attempt MUST live under: `./src/<FOLDER>/`
- Existing Aspire AppHost project: `./src/AppHost/`
  - You MUST plan to register all runnable projects and all introduced resources in AppHost.
  - You MUST plan the dependency graph between projects/resources in AppHost (e.g., UI -> server, server -> db, etc.).
- Use .NET 10 and modern C# language features.
- Use Blazor "Auto" render mode where a Blazor UI is built, and make effective use of SSR + interactivity.
- You may use any NuGet packages you want. If you introduce Node tooling, prefer `pnpm`.

Build cleanliness requirement (hard):
- At the end of implementation, the entire repo MUST build with ZERO warnings and ZERO errors.

Naming constraints (hard):
- All projects you create MUST have names that start with prefix `<FOLDER>` (project file names, assembly names, root namespaces).

launchSettings.json constraints (hard):
- Every ASP.NET project you create MUST have `Properties/launchSettings.json` with ONLY an `"http"` profile.
- No `"https"` profile anywhere.
- `applicationUrl` MUST be `http://...` and use dev-local hostnames based on <SLUG>.
  - Convention: `http://<SLUG>-<service>.dev.localhost:<port>`
  - You decide `<service>` and ports; they must be consistent and documented in PLAN.md.

Aspire conventions (hard):
- Any project that exposes an HTTP endpoint AND is meant to be visited must be registered in AppHost with:
  - endpoint name: "http"
  - `.WithUrlForEndpoint("http", url => { url.DisplayText = "..."; })` (Select a descriptive name for the display text. If this is for a project `FooBar.Api` then might give the display text `Foo Bar API` for example)
- Any Blazor UI project MUST be registered with:
  - `.WithExternalHttpEndpoints()`
  - `.WithExplicitStart()`

(Do not overfit to a specific project layout. You decide what projects you need.)

========================
WHAT TO BUILD
========================
Build a multi-workspace "Knowledge Base" web application for teams:
- Users create workspaces, invite members, and publish markdown articles with tags and comments.
Think "internal wiki", but lightweight and product-like.

Branding & theme requirement (very important):
- Invent a product name (business/app name) and use it consistently.
- UI must look appealing and intentionally designed (not a stock template).
- Plan a unique theme/branding style (colors, typography, spacing, component styling).
- If you use a UI component library, customize it to feel on-brand (not the default look).

========================
MINIMUM FEATURE REQUIREMENTS (MUST BE PLANNED END-TO-END)
========================
A) Accounts, workspaces, and access control
1) Authentication:
   - Users can register and sign in/out.
   - Passwords must not be stored in plain text (use a secure approach).
2) Workspaces:
   - A signed-in user can create a workspace.
   - Workspace has a "switch workspace" UX.
3) Workspace membership + roles:
   - Roles at minimum: Owner, Member.
   - Owner can manage members; Member cannot.
   - Authorization must be enforced server-side.

B) Invitations workflow (multi-step)
4) Invite flow:
   - Owner can invite a user to a workspace.
   - Provide an invite link/token that another user can use to join.
   - Recipient can accept the invite and becomes a member.
   - Include at least one non-happy-path (expired/invalid token) with good UX.

C) Articles, workflow, and versions
5) Article authoring:
   - Create/edit article with: title, summary, markdown body, tags.
   - Include a markdown preview experience.
6) Draft → Review → Published workflow:
   - Articles start as Draft.
   - Member can submit for review.
   - Only Owner can approve/publish.
   - Owner can unpublish back to Draft or Archive it (distinct state).
7) Revision history:
   - When an article is published, create a revision snapshot.
   - Readers can view at least latest 3 revisions (timestamp + who published).
8) Safe markdown rendering:
   - Render markdown in a way that does not allow script injection.

D) Discovery & information architecture
9) Article list page:
   - Search (title + summary + body)
   - Filter by tag
   - Filter by status
   - Sort (at least: recently updated, newest)
   - Pagination
   (Must be end-to-end in implementation; not in-memory only.)
10) Tag directory:
   - View all tags with usage counts within the workspace.
   - Owner can create/rename/delete tags, handling existing tagged articles safely.
11) Article details:
   - Render markdown.
   - Show metadata: author, created, updated, tags, status, reading time estimate.

E) Comments + moderation
12) Comments:
   - Signed-in members can comment on published articles.
   - Comment list includes: author + timestamp.
13) Moderation:
   - Owner can delete any comment in the workspace.
   - Regular members can only delete their own comments.

F) Personal productivity
14) Bookmarks:
   - User can bookmark articles.
   - Provide bookmarks page scoped to current workspace.

G) Engineering requirements
15) Backend boundary:
   - Implement a clear server-side API boundary (Minimal APIs, controllers, etc.).
   - Do NOT expose persistence models directly; use DTOs.
16) Persistence + evolution:
   - Use real persistence (you choose).
   - Include schema evolution/migrations approach.
17) Reusable UI components:
   - Create at least 4 reusable Blazor components used in multiple places:
     - Confirmation modal/dialog
     - Reusable input component (e.g., TagPicker, MarkdownEditor, ValidatedTextField)
     - Reusable list/table used for at least two lists
     - Notification/toast/alert component
18) UX completeness:
   - Loading/empty/error states for key pages.
   - Responsive layout (mobile widths).
   - Keyboard-friendly forms with proper labels; focus management where needed.
   - Confirm destructive actions.
19) Observability:
   - Structured logging.
   - Health endpoint(s).

========================
PLANNING DELIVERABLE (HARD)
========================
Write an extensive plan describing exactly what you will build and how.

Save it as:
`./src/<FOLDER>/PLAN.md`

PLAN.md MUST include:
1) Product name + short pitch
2) Theme/branding plan:
   - Visual identity keywords
   - Color palette (primary/secondary/neutral + semantic colors)
   - Typography hierarchy
   - Spacing/radius/shadow rules
   - Component styling rules (buttons, inputs, cards, tables, alerts)
   - How you'll ensure it doesn't look "off-the-shelf"
3) Roles/permissions table (Owner vs Member)
4) Page map (every page/route) and main user flows step-by-step
5) Architecture:
   - The projects you will create (you decide)
   - Boundaries between them and why
6) Data strategy:
   - Persistence choice
   - Key data shapes/entities
   - Indexing notes
   - Schema evolution/migrations approach
7) API surface summary:
   - Endpoint list + DTOs
   - Paging/filter/sort contract shapes
8) UI plan:
   - The 4+ reusable components and where they're reused
9) Aspire integration plan:
   - List every runnable project and every resource to register
   - For each runnable HTTP project:
     - Aspire resource name (kebab-case using <SLUG>)
     - Endpoint name must be "http"
     - DisplayText to set via `.WithUrlForEndpoint("http", ...)`
   - Explicitly note: any Blazor UI project will be registered with
     `.WithExternalHttpEndpoints().WithExplicitStart()`
   - Dependency graph: which projects reference which (e.g., UI -> server; server -> db)
10) Execution checklist (step-by-step)
11) Verification plan (must include agent-browser):
   - Build commands
   - How you will run via AppHost
   - A concrete agent-browser test script outline for core flows

========================
IMPORTANT WORKFLOW RULE (HARD)
========================
After creating PLAN.md:
- STOP.
- Do not implement anything else.
- Do not modify any other files.
- Commit ONLY the PLAN.md file.
- Provide a short summary of what you planned.
