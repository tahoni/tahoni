# AGENTS.md

Conventions for any AI coding agent working in this repository — project overview, documentation and git workflow
conventions, and the release checklist. [`CLAUDE.md`](CLAUDE.md) is a thin pointer to this file, kept only because
Claude Code discovers that filename by convention; no Claude-Code-specific content is split out from it.

## Table of Contents

- [📖 Project Overview](#-project-overview)
- [📚 Documentation Conventions](#-documentation-conventions)
- [🗂️ Documentation File Map](#-documentation-file-map)
- [🧩 Claude Code Skills](#-claude-code-skills)
- [🔀 Git Workflow](#-git-workflow)
- [🚢 Release Checklist](#-release-checklist)
- [🌲 Evergreen Documentation](#-evergreen-documentation-readmemd)

---

## 📖 Project Overview

This is Leoni Lubbinge's (tahoni) personal GitHub profile repository. `README.md` renders directly as the bio shown
on the GitHub profile page; `docs/` holds the source content mirrored out to other platforms (GitHub's own profile
bio field, LinkedIn). There is no application code, build, or test suite — every change here is content/documentation.

---

## 📚 Documentation Conventions

### British English

All documentation prose uses British English spelling (e.g. "specialise", "colour", "organise", "favourite"), not
American English, matching the existing tone of `README.md` and the bio files. The one exception is third-party
product, library and API names quoted verbatim.

### Serial commas

Lists of three or more items don't take a comma before the final `and`/`or` (e.g. "clubs, competitors and matches",
not "clubs, competitors, and matches"), consistent with the British English convention above.

### Line wrapping

Wrap prose lines in every Markdown file between 100 and 120 characters. This doesn't apply to fenced code blocks or
GFM tables, which may run longer to keep columns aligned.

### Standard structure

Every documentation file **other than `README.md`** follows the same shape:

- An `H1` title, followed by a short introductory sentence or two.
- A Table of Contents for any document with more than roughly four sections.
- `##` sections, separated by a `---` horizontal rule between major sections.
- GFM tables for structured or tabular information (file maps, skill lists).

**`README.md` is the one exception**: it's a freeform GitHub-profile bio with no title or headings, rendered directly
on the GitHub profile page — it must not gain a heading structure.

### Icons in headings

Every heading listed in a Table of Contents is prefixed with an emoji, and its ToC entry uses the same emoji. Reuse
an icon already established for a concept rather than inventing a new one; only pick a new emoji when introducing a
genuinely new concept. Icons already established in this repository's documentation:

| Icon | Concept                        |
|------|--------------------------------|
| 📖   | Introduction / overview        |
| 📚   | Documentation                  |
| 🗂️   | Documentation file index       |
| 🧩   | Tooling / automation           |
| 🔀   | Git workflow / pull request    |
| 🚢   | Release process                |
| 🌲   | Evergreen documentation        |
| 📦   | Release notes / what's new     |
| 🚀   | Instructions / current version |
| 📝   | Notes / headline               |
| ✨   | Summary / features             |
| ✅   | Test plan / completed          |
| 🔍   | Current state / inspection     |

---

## 🗂️ Documentation File Map

| File        | Purpose                                                                |
|-------------|------------------------------------------------------------------------|
| `README.md` | GitHub profile bio — renders directly on the GitHub profile page       |
| `CLAUDE.md` | Thin pointer to `AGENTS.md`, kept for Claude Code's filename discovery |
| `AGENTS.md` | Cross-tool conventions for this repository (this file)                 |

Two documentation-only folders supplement these:

- **`docs/`** holds the source content mirrored out to other platforms:

  | File                     | Purpose                                                |
    |--------------------------|--------------------------------------------------------|
  | `docs/github/bio.md`     | Short bio mirrored into GitHub's own profile bio field |
  | `docs/linkedin/bio.md`   | Short bio mirrored into LinkedIn's headline/bio field  |
  | `docs/linkedin/about.md` | Longer About section mirrored into LinkedIn            |

- **`documentation/history/`** holds one of each of the following files per released version:

  | File                       | Purpose                                                    |
    |----------------------------|------------------------------------------------------------|
  | `RELEASE_NOTES_vX.Y.Z.md`  | Archived release notes for that version                    |
  | `PR_DESCRIPTION_vX.Y.Z.md` | The release pull request's body, archived for that version |

---

## 🧩 Claude Code Skills

`.claude/skills/` holds this repository's project-specific Claude Code skills, one `SKILL.md` per skill:

| Skill                     | Purpose                                                                                                           |
|---------------------------|-------------------------------------------------------------------------------------------------------------------|
| `generate-commit-message` | Draft a commit message for the working tree's changes, matching this repo's plain, imperative-mood commit history |
| `prep-version-release`    | Draft a version's `RELEASE_NOTES_vX.Y.Z.md` and `PR_DESCRIPTION_vX.Y.Z.md` under `documentation/history/`         |

---

## 🔀 Git Workflow

### Branching Model (GitFlow)

This repository follows the [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/) branching model:

- **`develop`** is the current development branch — all day-to-day work lands here first.
- **`main`** is the production branch. It is only ever updated by promoting `develop` after a `release/vX.Y.Z`
  branch has merged into it.
- **`feature/<short-description>`** — day-to-day content work (e.g. `feature/documentation`). Branch from, and PR
  back into, `develop`.
- **`release/vX.Y.Z`** branches are cut from `develop` once it's ready to ship — they carry the release-prep changes
  (see the Release Checklist below) and are opened as a PR against `develop`. Once that merges, a second PR promotes
  `develop` into `main`, which is then tagged `vX.Y.Z`.

### Conventions

- **Commit in logical chunks.** One concern per commit — do not bundle unrelated changes into a single commit.
- **Plain, imperative-mood commit messages.** This repository does not use Conventional Commits prefixes (`feat:`,
  `fix:`, `docs:`, etc.) — see the `generate-commit-message` skill.

---

## 🚢 Release Checklist

When cutting a new version, work through these steps **in order**:

1. **Confirm the diff against the latest tag** covers everything that changed for this release.
2. **Draft `documentation/history/RELEASE_NOTES_vX.Y.Z.md`** covering everything that changed for this version, not
   just the most recent commit — see the `prep-version-release` skill for the exact format.
3. **Write `documentation/history/PR_DESCRIPTION_vX.Y.Z.md`** — the release pull request's body.
4. **Apply the reverse sync check** (below) against `README.md` and the `docs/` bio files.

Commit the release notes and PR description together as one documentation commit, unless asked otherwise.

---

## 🌲 Evergreen Documentation (README.md)

`README.md` describes the durable, current state of the bio — not a version's release notes. It must:

- **Never contain references to specific versions** (e.g. `v5.2.0`) or release-specific language.
- **Never contain counts that drift** (e.g. "10+ years of experience" is fine as a rounded, slow-changing figure, but
  avoid anything that needs updating on every release).

**Reverse sync rule:** When drafting `RELEASE_NOTES_vX.Y.Z.md`, check whether any of the changes being documented are
relevant to `README.md` or the `docs/` bio files and update those too if so, keeping them internally consistent (same
links, same tone) and release-agnostic per the rules above.
