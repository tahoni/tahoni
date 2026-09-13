---
name: generate-commit-message
description: Generate a commit message for the current working tree changes, matching this repo's existing commit history style. Use whenever the user is about to commit, asks for a commit message/summary, or asks how to describe the current changes.
user-invocable: true
allowed-tools:
  - Bash(git status:*)
  - Bash(git diff:*)
  - Bash(git --no-pager diff:*)
  - Bash(git log:*)
  - Bash(git --no-pager log:*)
  - Bash(git merge-base:*)
  - Bash(git branch:*)
  - Read
---

# Generate Commit Message

Optional scope narrowing may be passed as `args` when this skill is invoked (limit the message to specific files/areas;
leave blank for all staged/unstaged changes).

## 🔍 Gather current state

Before drafting, run these yourself and read their output:

1. `git status --short`
2. `git --no-pager diff --stat`
3. `git --no-pager diff HEAD` (full diff, staged and unstaged)
4. Commits already made on this branch — context only, may include commits made outside this session by anyone:
   ```
   base=$(git merge-base develop HEAD 2>/dev/null || git merge-base main HEAD 2>/dev/null)
   git --no-pager log --oneline "$base"..HEAD 2>/dev/null
   ```
5. `git --no-pager log -20 --oneline` on `main` — this repo's existing commit history is the style guide; there is no
   `AGENTS.md` or contribution doc to defer to.

## 🚀 Instructions

This is a personal profile repo (README + bio content under `docs/`) — there is no CHANGELOG.md, issue tracker
convention, or build/test detail to reference. Base the message purely on the actual diff and this repo's existing
commit style.

This skill drafts a message for **whatever is currently staged/unstaged** — it is not limited to changes made in the
current Claude session. Use the "commits already made on this branch" context above purely to stay consistent
(matching phrasing/scope naming) — never fold already-committed work into the new message.

1. **Inspect the changes above**, do not guess — review the actual diff hunks so the message describes real content
   changes, not assumptions. If scope narrowing was passed in `args`, only consider matching files.
2. **Compose the message** in this exact shape:

   ```
   <Brief, imperative-mood description>

   - <optional bullet of notable detail>
   - <optional bullet of notable detail>
   ```

    - This repository does **not** use Conventional Commits prefixes (`feat:`, `fix:`, `docs:`, etc.) — matching its
      existing history, commit messages are **plain, imperative-mood descriptions** of the change, e.g.
      `Add LinkedIn bio and about content` or `Display full URLs for GitHub and LinkedIn links in README`.
    - Lead with an imperative verb (Add/Fix/Update/Remove/Rewrite/Clean up…), name the specific file or section
      changed, optionally followed by a colon and further detail.
    - **Body bullets**: optional. Include them only when the change is non-obvious or touches multiple files; each
      bullet should state *what* changed, not restate the file list.
    - If the change closes a GitHub issue, add a trailer line `Closes #<issue>` — only when there genuinely is one;
      don't invent a reference.
3. **Group unrelated work**: if the diff contains clearly unrelated changes (e.g. a README edit plus an unrelated bio
   file addition), propose separate commits with a separate message for each rather than forcing one message.
4. **British English** spelling, grammar and punctuation throughout (e.g. "specialise", "colour", "organise"),
   matching the existing tone of `README.md` and the bio files.
5. **Sanity-check**: no secrets or personal contact details beyond what's already public in the repo, no vague
   messages such as "updated stuff" or "changes".

## 📤 Output

Do **not** run `git add` or `git commit` yourself — this skill only drafts, for the user to review and run.

1. The final commit message(s) as fenced code blocks, each followed by a ready-to-run `git commit` command
2. If proposing multiple commits, output one message block and one commit command per commit, in the order they
   should be made

Example output structure:

**Commit 1:**

```
Add generate-commit-message skill for drafting commit messages
```

```bash
git commit -m "Add generate-commit-message skill for drafting commit messages"
```
