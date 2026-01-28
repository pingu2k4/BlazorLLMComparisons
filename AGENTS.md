# AGENTS.md

This repository is designed to be worked on by coding agents across multiple
sessions. Continuity is critical.

## Non-negotiable: use the Work Diary

You MUST read and follow the diary process described in:

- `./.diary/README.md`

Treat it as the source of truth for how to resume work safely and how to record
progress so another session (or another agent) can continue without guesswork.

### Session start (required)
1) Read `./.diary/README.md`.
2) Determine the current git branch name.
3) Compute the diary filename from the branch name per the rules in
   `./.diary/README.md`.
4) If the diary file exists, read it fully before doing any work.

### During the session (required)
- After each **meaningful action** (code/config changes, important commands run,
  decisions made, constraints/bugs discovered, tasks created that affect next
  steps), you MUST update the diary file.
- Do **not** write diary entries for minor intermediate edits that are part of a
  single step. Update once per completed meaningful step, exactly as described.

### Session end (required)
- If you performed any meaningful actions in the session, ensure the diary is
  updated before you stop.
- Make sure “Next actions” in the diary are accurate and actionable.

---

If there is any conflict between this file and `./.diary/README.md`, the diary
README wins.
