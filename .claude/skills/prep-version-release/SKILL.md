---
name: prep-version-release
description: Prepare a new version release — a RELEASE_NOTES.md and a draft release PR description, archived under documentation/history/. Use whenever the user is preparing/cutting a release PR or asks to draft release documentation for a version of this profile repo.
user-invocable: true
allowed-tools:
  - Bash(git log:*)
  - Bash(git --no-pager log:*)
  - Bash(git diff:*)
  - Bash(git --no-pager diff:*)
  - Bash(git branch:*)
  - Bash(git status:*)
  - Bash(git tag:*)
  - Bash(git merge-base:*)
  - Read
  - Edit
  - Write
---

# Prepare Version Release

The version to prepare a release for is passed as `args` (e.g. `5.3.0`). If not supplied, look at the latest git tag
and propose the next version to the user for confirmation before proceeding — don't just guess and continue silently.
The rest of this skill refers to the resolved value as `$VERSION`.

## 🔍 Gather current state

Before drafting, run these yourself and read their output:

1. `git branch --show-current`
2. `git tag -l --sort=-v:refname` (find the latest existing tag, used as the baseline for "what's new")
3. `git --no-pager diff --stat <latest-tag>...HEAD` (changes relative to the previous release)
4. `git log <latest-tag>..HEAD --oneline` (commit log relative to the previous release)
5. Read the current `README.md` and the bio files under `docs/` (e.g. `docs/github/bio.md`, `docs/linkedin/bio.md`,
   `docs/linkedin/about.md`) to understand what changed in tone/content.
6. Read the most recent file in `documentation/history/` (e.g. the prior `RELEASE_NOTES_v*.md` and
   `PR_DESCRIPTION_v*.md`) to match established structure, heading style and emoji conventions.

## 🚀 Instructions

This is a personal profile repo (README + bio content) — there is no build, test suite, package manifest, or
version file to bump. Follow this repo's established GitFlow-style branching: release branches (`release/v$VERSION`) are
cut from `develop`, merged back into `develop`, and `develop` is later merged into `main`
and tagged there as `v$VERSION`.

Steps:

1. **Confirm the diff against the latest tag** (gathered above) covers everything that changed for this release —
   re-run the `git log`/`git diff --stat` commands yourself if the branch has moved on since this skill started.
2. **Draft `documentation/history/RELEASE_NOTES_v$VERSION.md`.** Follow the established format:
    - `# 📦 Release Notes`
    - `## 🚀 v$VERSION`
    - A one-line theme sentence summarising what the release is about
    - A flat bullet list covering **everything** that changed for this version (not just the latest commit) — pulled
      from the commit log and diff, in plain language (e.g. "Rewrote the README bio…", "Fixed an incorrect LinkedIn
      URL…"). Group into `### Added` / `### Changed` / `### Fixed` / `### Removed` subsections only if the release is
      large enough that a flat list gets hard to scan; otherwise keep it a single flat list like prior releases.
    - End with a blank line then the standard Claude Code attribution footer:
      `🤖 Generated with [Claude Code](https://claude.com/claude-code)`
3. **Write `documentation/history/PR_DESCRIPTION_v$VERSION.md`** — the body text for the release pull request. Keep
   it small — a PR body, not a second release notes file: a few bullets per section, high-level only. Structure:
    - `# 🔀 Pull Request`
    - `## 📝 <short headline for the change> (v$VERSION)`
    - `### ✨ Summary` — a few bullets on what changed and why
    - `### ✅ Test plan` — a checklist of manual checks appropriate to a content-only repo, e.g. preview `README.md`
      rendering on GitHub, verify all links resolve, spot-check bio files for typos/formatting
    - End with a blank line then the standard Claude Code attribution footer:
      `🤖 Generated with [Claude Code](https://claude.com/claude-code)`
4. **Apply the reverse sync check**: re-read `README.md` and the `docs/` bio files once the release notes are
   finalised, and confirm they're internally consistent and don't contradict each other (e.g. same links, same tone)
   — these files should stay evergreen, with no version numbers or release-specific language leaking into them.

Commit the release notes and PR description together as one documentation commit, unless the user asks otherwise. Do
not run `git commit`, `git push`, tag the release, or open the PR yourself — draft the files and stop for review.

## 📤 Output

Once both files above are written, tell the user the release branch (`release/v$VERSION`) is ready to open as a PR
against `develop`, using `documentation/history/PR_DESCRIPTION_v$VERSION.md` as the PR body. Once that PR merges,
remind them a second PR promoting `develop` into `main` is still needed to actually ship the release — tag the
resulting commit on `main` as `v$VERSION`.
