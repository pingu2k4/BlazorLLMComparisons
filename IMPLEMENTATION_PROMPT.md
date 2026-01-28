You are working in an existing repository. Read and follow:
`./src/<FOLDER>/PLAN.md`

Hard constraints (must comply):
- Use .NET 10 and modern C# language features.
- Use Blazor "Auto" render mode.
- All new work for this exercise must live under `./src/<FOLDER>/`.
  - Exceptions: you may modify the existing Aspire AppHost project under `./src/AppHost/` and the root `.slnx` as required for integration.
- There is a `.slnx` at the repo root (`./BlazorLLMComparisons.slnx`): every project you create MUST be added to it.
- There is an existing Aspire AppHost project under `./src/AppHost/`. You MUST:
  - Register your newly created projects and any resources/dependencies you introduce.
  - Register the Blazor project with explicit start. In the AppHost, the Blazor project registration MUST include `.WithExplicitStart()`, conceptually like:

    var blazorApp = builder
        .AddProject<Projects.YourBlazorApp>("<FOLDER>")
        .WithExplicitStart();

  (Use the correct generated `Projects.*` type and resource name for your project.)
- All projects you create MUST have names that start with the prefix `<PREFIX>` (project file names, assembly names, and root namespaces where applicable).
- You may use any NuGet packages you like. If you introduce Node tooling, prefer `pnpm`.

Build cleanliness requirement (hard):
- The entire repo MUST build and compile with ZERO warnings and ZERO errors at the end.

Implementation requirements:
- Implement every feature and requirement described in PLAN.md, and ensure it satisfies the minimum feature requirements listed there.
- Ensure authorization is enforced server-side for workspace scoping, roles, article workflow actions, and comment moderation.
- Ensure markdown rendering is safe from script injection.
- Ensure list search/filter/sort/pagination are implemented end-to-end (UI + API/query), not in-memory only.

UI/UX & branding requirements:
- Implement the theme/branding plan from PLAN.md so the app looks intentionally designed.
- Avoid "default template" look. Customize styles/components so it feels unique and cohesive.
- Use reusable components and consistent spacing/typography.

Documentation requirement:
- Add `./src/<FOLDER>/README.md` documenting:
  - Product name + what it does
  - Any required setup (persistence init/migrations, seed data, default accounts if you add any)
  - Key architectural choices and tradeoffs
  - Theme/branding notes (what you did to make it unique)

Execution rules:
1) Start by summarizing the plan you are about to implement and list the steps you will perform.
2) Implement fully:
   - Create projects, code, configuration, and any scripts/tooling needed.
   - Wire everything through Aspire AppHost as planned.
   - Add all created projects to the `.slnx`.
3) Provide exact commands to build/run/verify.
4) Output a concise “where to look” map: key files for API, auth, persistence, UI components, styling/theme, and Aspire config.

Output format in your response:
- "What changed" summary
- File/folder tree under `./src/<FOLDER>/`
- Requirement-to-implementation callouts (bullets with file references)
- Commands to run and verify

Stop when implementation is complete.
