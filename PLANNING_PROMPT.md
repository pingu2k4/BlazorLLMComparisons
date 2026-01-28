You are working in an existing repository.

Repo constraints you must follow:
- There is a `.slnx` file at the repo root `./BlazorLLMComparisons.slnx`. Any project(s) you create MUST be added to that `.slnx`.
- There is a `./src/` folder. Inside `./src/`, create a new folder named `<FOLDER>/` and put all of your work for this exercise inside it.
- Inside `./src/AppHost/` there is already an Aspire AppHost project. You MUST register your new projects and any resources/dependencies you introduce (datastores, caches, queues, etc.) with that AppHost.
- Use .NET 10 and modern C# language features.
- Use Blazor “Auto” render mode, and make full use of static SSR, and everything that auto mode gives us.
- You may use any NuGet packages you want. If you introduce Node tooling, prefer `pnpm` over `npm`.

Build cleanliness requirement (hard):
- At the end of implementation, the entire repo MUST build and compile with ZERO warnings and ZERO errors.

Naming constraints:
- All projects you create MUST have names that start with the prefix `<PREFIX>` (project file names, assembly names, and root namespaces where applicable).

What to build:
Build a multi-workspace “Knowledge Base” web application for teams. Users can create workspaces, invite members, and publish markdown articles with tags and comments. Think “internal wiki”, but lightweight and product-like.

Branding & theme requirement (very important):
- You MUST invent a product name (the "business/app name") and use it consistently.
- The UI must look appealing and intentionally designed (not like a stock template).
- You must plan a unique theme/branding style (colors, typography, spacing, component styling).
- If you use a UI component library, you must customize it to feel on-brand (not the default look).

Minimum feature requirements (must be implemented end-to-end):

A) Accounts, workspaces, and access control
1) Authentication:
   - Users can register and sign in/out.
   - Passwords must not be stored in plain text (use a secure approach).
2) Workspaces:
   - A signed-in user can create a workspace.
   - Workspace has a "switch workspace" UX (some visible notion of current workspace).
3) Workspace membership + roles:
   - Roles at minimum: Owner, Member.
   - Owner can manage members; Member cannot.
   - Authorization must be enforced server-side (not only hidden UI).

B) Invitations workflow (multi-step)
4) Invite flow:
   - Owner can invite a user to a workspace.
   - The app must provide an invite link/token that another user can use to join.
   - A recipient can accept the invite and becomes a member.
   - Include at least one non-happy-path (expired/invalid token) with a good UX.

C) Articles, workflow, and versions
5) Article authoring:
   - Create/edit an article with: title, summary, markdown body, tags.
   - Include a markdown preview experience (live preview or preview screen).
6) Draft → Review → Published workflow:
   - Articles start as Draft.
   - Member can submit for review.
   - Owner can approve and publish (members cannot publish).
   - Owner can unpublish back to Draft or Archive it (a distinct state).
7) Revision history:
   - When an article is published, create a revision snapshot.
   - Readers can view at least the latest 3 revisions (timestamp + who published).
8) Safe markdown rendering:
   - Render markdown in a way that does not allow script injection.

D) Discovery & information architecture
9) Article list page:
   - List articles with:
     - Search (title + summary + body)
     - Filter by tag
     - Filter by status
     - Sort (at least: recently updated, newest)
     - Pagination
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

F) Personal productivity (polish)
14) Bookmarks:
   - User can bookmark articles.
   - Provide a bookmarks page scoped to the current workspace.

G) Engineering requirements
15) Backend boundary:
   - Implement a clear server-side API boundary (Minimal APIs, controllers, etc.).
   - Do NOT expose persistence models directly; use request/response contracts (DTOs).
16) Persistence + evolution:
   - Use real persistence (you choose which).
   - Include a clear approach to schema evolution/migrations appropriate to your choice.
17) Reusable UI components:
   - Create at least 4 reusable Blazor components used in multiple places:
     - Confirmation modal/dialog component
     - Reusable input component (e.g., TagPicker, MarkdownEditor, ValidatedTextField)
     - Reusable list/table component used for at least two lists
     - Notification/toast/alert component
18) UX completeness:
   - Loading, empty, and error states for all key pages.
   - Responsive layout that works on mobile widths.
   - Keyboard-friendly forms with proper labels; focus management where needed.
   - Confirm destructive actions.
19) Observability:
   - Structured logging.
   - Health endpoint(s).

Planning deliverable:
- First, write an extensive plan describing exactly what you will build and how you’ll build it.
- Save it as `./src/<FOLDER>/PLAN.md`.

Your PLAN.md must include:
- Product name + short pitch
- Theme/branding plan:
  - Visual identity keywords (e.g., "calm, editorial, modern")
  - Color palette (primary/secondary/neutral + semantic colors)
  - Typography choices (system fonts acceptable; specify hierarchy)
  - Spacing/radius/shadow style rules
  - Component styling rules (buttons, inputs, cards, tables, alerts)
  - How you’ll ensure it doesn’t look "off-the-shelf"
- Roles/permissions table (Owner vs Member; include what actions each role can do)
- Page map (every page/route) and the main user flows step-by-step
- Architecture (projects you will create; boundaries; why)
- Data strategy (chosen persistence, key data shapes, indexing notes, schema evolution approach)
- API surface summary (routes/endpoints + key DTOs; include paging/filtering shapes)
- UI plan (the 4+ reusable components and where they are reused)
- Aspire integration plan:
  - What you will register in AppHost (projects + any resources)
  - Explicitly note that the Blazor project will be registered using `.WithExplicitStart()`
- Step-by-step execution checklist

Important workflow rule:
- After creating PLAN.md, STOP.
- Do not implement anything else.
- Do not modify any other files.
- Commit the PLAN.md file and provide a summary when you have completed this task.
