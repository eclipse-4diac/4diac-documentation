---
name: eclipse-4diac-contribution-check
description: >-
  Use before every `git commit` and before creating or updating a pull
  request in any eclipse-4diac repository (4diac-ide, 4diac-forte,
  4diac-documentation) or a fork of one. Checks the commit message, PR
  description, file headers, code comments, and any new documentation
  files against the project's actual contribution guidelines (Eclipse
  Foundation Project Handbook + eclipse.dev/4diac/doc/development/contribute.html
  + the repos' own CONTRIBUTING.md). Trigger on: preparing a commit
  message, opening/editing a PR title or description, adding or editing
  a copyright/file header, writing a code comment of more than one line,
  creating a new `.md`, `.adoc`, or `.asciidoc` documentation file, or
  bumping a version number — in any of these three repos or their forks.
---

# Eclipse 4diac contribution compliance check

Run this checklist **before** every `git commit` and before creating/editing
a PR title or description in `eclipse-4diac/4diac-ide`,
`eclipse-4diac/4diac-forte`, `eclipse-4diac/4diac-documentation`, or a fork of
any of them. The goal is to catch guideline mismatches before a human
reviewer has to point them out.

Every rule below is either a **direct quote from an authoritative source**
(cited) or **observed project convention** from real review threads (noted
as such, since it isn't written down anywhere yet). Don't invent conventions
beyond what's here — if something isn't covered and you're unsure, ask
rather than guess, and consider whether the gap itself is worth a small doc
PR (see the last section).

## 1. Commit messages

Source: [eclipse.dev/4diac/doc/development/contribute.html](https://eclipse.dev/4diac/doc/development/contribute.html)
(4diac-specific, stricter than and takes precedence over the generic
Eclipse Handbook default of 72/72 below, for these three repos):

- **Subject line: max 50 characters.** Short, imperative, descriptive
  ("Fix X", "Add Y" — not "Fixed", not "Adds").
- **Body: wrap at ~72 characters per line.**
- **Self-contained and comprehensible without external links.** Explain the
  *why*, not just the *what* — the diff already shows the what.
- **No links to issues, discussions, or pull requests in the commit
  message.** Not `Fixes #1234`, not a bare URL, not a session-tracking
  link. Issue/PR references belong in the **PR description**, not the
  commit body. (This is the single most common thing to double-check —
  scan every commit message for a URL or `#<number>` before committing,
  every time.)
- **One topic per commit.** No `wip`, `fix typo`, `address review comment`
  commits surviving into the final PR — `git commit --amend` or squash
  before pushing, so the PR's history reads as a small number of clean,
  complete, single-purpose commits. (Multiple commits are fine when they are
  genuinely separable units of work, e.g. "add feature" + "add tests for
  feature" + "update docs for feature" — the bar is "each commit stands on
  its own and makes sense in isolation", not "exactly one commit".)
- **Don't over-explain.** A commit message is not the place to write a
  design essay. State what changed and the one or two non-obvious reasons
  why. If you find yourself writing more than ~4-6 short paragraphs, that's
  a sign to move detail into the PR description instead, or that the change
  itself should be split.

Per the Eclipse Foundation Handbook
([`#resources-commit`](https://www.eclipse.org/projects/handbook/#resources-commit),
generic default, still applies where 4diac doesn't say otherwise):
- Three-part structure: **summary line, description, footer.**
- `Author` field must be the real legal name + the email on file with the
  Eclipse Foundation account.
- Additional authors: `Also-by: Name <email>` or `Co-authored-by: Name
  <email>` — one entry per person, and per the Handbook this entry should be
  a real, ECA-covered contributor. This is attribution for a co-author, not
  AI-tool disclosure — see the `Assisted-by:` trailer below for that. (If
  you're using an AI coding assistant that appends its own fixed
  `Co-Authored-By:`-style trailer as a tool convention, that's not
  something to edit — just don't add anything *beyond* it, e.g. no extra
  session-link line, since that *is* a link.)
- **AI-tool disclosure — `Assisted-by:` trailer.** Source: [Eclipse
  Foundation Project Handbook, "Identifying an AI Assistant"](https://www.eclipse.org/projects/handbook/#ai)
  (an emerging de facto standard per the Handbook, not yet a hard
  requirement — "reasonable effort should be undertaken to disclose the
  use of AI in project content"; "if providing all of this information is
  especially onerous or impossible, specify as much of it as you can"):
  - Exact format: `Assisted-by: [Provider] [Model-Family] ([Version/ID])`.
    Handbook examples: `Assisted-by: Google gemini-2.5-pro (rev-1)`,
    `Assisted-by: GitHub Copilot (GPT-4o-2024-08-06)`.
  - This is a **separate trailer from** `Co-authored-by:`/`Also-by:` — it
    discloses which AI assisted, it doesn't claim ECA-covered co-authorship.
    Both can appear in the same commit footer.
  - For a file that is **largely or entirely AI-generated** (not just
    assisted), the Handbook also asks for an `Assisted-by:` line in the
    file's own copyright header, alongside an "AI Disclosure" note and an
    SPDX expression combining the project licence with `CC0-1.0` (AI-only
    output is likely not copyrightable) — see the Handbook section for the
    full header template. This is a different, stronger case than a
    human-authored change that merely used an AI assistant as a tool.
- No manual `Signed-off-by` needed — the ECA already covers DCO.

## 2. Copyright / file headers — **single year, not a range**

Source: [Eclipse Foundation Project Handbook, "Copyright Headers"](https://www.eclipse.org/projects/handbook/#ip-copyright-headers)
(this section **changed** at some point from an older "first year, last
year" range convention; do not follow examples elsewhere on the web or in
older files that show a year range):

> "*Copyright statements* take the form `Copyright (c) {year} {owner}`."
> "The `{year}` is the year in which the content was created (e.g. \"2004\")."
> "The `{year}` is the year of the initial creation."

Concretely:
- **Exactly one year** in a `Copyright (c) {year} {owner}` line: the year
  the file was **first created**. Do **not** add a second year, do **not**
  update the year on later edits, do **not** write `2023, 2026` or
  `2023-2026`.
- When you make a **significant** contribution to an **existing** file, add
  yourself to the `Contributors:` list below the copyright/license block
  with a short description of what you added — don't touch the copyright
  year at all for this. (See the header template in the Handbook section
  above, or any recently-touched file in these repos for the exact EPL-2.0
  block format.)
- `{owner}` is a legal entity (a person or a company) — "and others" may be
  appended if many contributors have touched the file (see the Handbook
  section for the full nuance); don't overthink this for a small change.

## 3. PR title and description

Source: contribute.html:
- Explain **what** changed and **why**.
- **Issue/PR references go here**, not in the commit message. Use
  `Closes #N` / `Fixes #N` / `Resolves #N` **only** if this PR fully
  resolves that issue — otherwise just reference it as `- #N` or in prose,
  without the auto-close keyword.
- Keep the description in sync with the actual diff. If a review comment
  causes you to change the approach (e.g. drop a helper, split out part of
  the change into another PR), **update the PR description in the same
  turn** — a stale description describing removed/moved code is itself a
  guideline mismatch and a likely source of the next round of review.
- Cross-links to sibling PRs (a fork mirror, a split-out follow-up PR, a
  backport) are fine in the **PR body** — the "no links" rule is specifically
  about the commit message.

## 4. Comments — code and file headers

- Match the length/density of comments already in the surrounding file. If
  every other comment in the file is 1-3 lines, don't write a 15-line essay
  for your addition — if the reasoning genuinely needs that much space, it
  probably belongs in the PR description or a linked design doc, not inline.
- Comment the **why**, not the what — the code already shows what it does.
- **English only, no exception**, for comments, commit messages, PR text,
  and documentation files in these repos — even if the surrounding
  conversation with the user is in another language. This is a firm,
  consistently expected project convention, even though it isn't spelled
  out in any CONTRIBUTING.md.

## 5. Documentation files

- File format follows the repo, for actual documentation content:
  `4diac-ide` and `4diac-forte` use Markdown (`.md`) for repo-local docs;
  `4diac-documentation`'s published doc site (everything under `src/`) is
  AsciiDoc (`.adoc`, with a handful of legacy `.asciidoc` files; see
  section 7) — don't add a `.md` file under `src/` there. This doesn't
  apply to root-level project metadata files that are `.md` by Eclipse
  Foundation convention regardless of repo (`README.md`, `CONTRIBUTING.md`,
  `SECURITY.md`, `CODE_OF_CONDUCT.md`, `NOTICE.md`, `LICENSE.md`).
- Keep new documentation files **scoped, dated, and referenced** — a clear
  home (e.g. under `doc/`), a clear single topic, code references where
  relevant, and a link from somewhere so it's discoverable, rather than a
  free-floating file that only makes sense with tribal knowledge, and
  written in English only (see section 4) — never a German-original-plus-
  English-translation pair; write one English file.
- If a new `.md`/`.adoc`/`.asciidoc` file is genuinely a **working/discussion** artifact
  rather than end-state documentation, say so explicitly in the PR
  description and consider whether it belongs in the PR at all versus being
  pasted into the PR/issue description instead of committed as a file.
- Before adding a new doc file, check whether the content already belongs in
  an existing doc (e.g. `4diac-documentation`) rather than a repo-local
  scratch file.

## 6. Version numbers

No formal per-type `VersionInfo`/`Version=` bump policy is written in any of
the three CONTRIBUTING.md files or contribute.html, but version numbers in
these repos follow semantic-versioning-style reasoning about the *size* of
the change:

- **Small changes** (comments, documentation, minor/cosmetic fixes with no
  interface or behavior change): bump the **patch** number, e.g.
  `3.0` → `3.0.1`.
- **Functional extensions** (new features, new parameters, behavior
  changes that stay backward-compatible): bump the **minor** number, e.g.
  `3.0` → `3.1`.
- **Massive, global changes**: bump the **major** number, e.g. `1.0` →
  `3.0`. This is rare and, as a rule, never happens within a single PR —
  don't bump a major version speculatively.

- Before bumping any version number (a `VersionInfo Version="X.Y.Z"` in a
  4diac-ide type-library XML file, a CMake `project(... VERSION ...)`, a
  plugin's `MANIFEST.MF` `Bundle-Version`, etc.), still find **at least two
  or three independent, unrelated, already-merged precedents** for the
  exact same kind of bump in the exact same kind of file, to confirm the
  scheme above actually applies to that file type — not just one example
  you happened to find (a single example may itself be non-standard or
  simply wrong).
- If you can't find solid precedent, don't bump the version speculatively —
  ask the maintainers rather than guessing.
- Double-check arithmetic: a patch bump is `X.Y.Z` → `X.Y.(Z+1)`, not a
  reused or decremented number, and must not collide with a version already
  used elsewhere for a different, unrelated change to the same file.

## 7. Code style

Source: contribute.html:
- Run the project's own formatter/checker before committing: Checkstyle for
  4diac-ide (Java), the C++ style guide / clang-format for 4diac-forte,
  AsciiDoc conventions for 4diac-documentation.
- Existing tests must keep passing; add tests for new features/fixes
  (JUnit for 4diac-ide, Boost.Test for 4diac-forte).

## 8. AI-authored contributions

Source: contribute.html: *"we expect that all code remains maintainable by
humans"* — the human author remains responsible for the entire content and
must be able to personally explain any part of a PR. This skill's checks
exist to reduce review back-and-forth, not to replace the human author's own
understanding of what's being submitted — flag anything you're not fully
confident about rather than silently including it.

Also add an `Assisted-by:` commit trailer disclosing the AI tool used — see
section 1's `Assisted-by:` entry for the exact format and Handbook source.
Don't skip this because a harness-level tool convention already adds its own
attribution trailer (e.g. `Co-Authored-By:`) — that trailer claims
co-authorship, it isn't the Handbook's AI-disclosure mechanism, so both
belong in the same commit footer when applicable.

## 9. If the guidelines themselves are wrong or contradictory

Genuinely check for this — don't just apply the rules, notice when a source
document is itself inconsistent or incorrect, and propose a fix:

- **Example already found, fix pending**: `4diac-documentation`'s intro text
  says "IEC 61499 extends IEC 61131-**1**", while both `4diac-forte` and
  `4diac-ide`'s CONTRIBUTING.md say IEC 61131-**3** (the correct reference —
  IEC 61131-3 is the programming-languages part; IEC 61131-1 is "General
  information", unrelated to this claim). A small, single-word-scope fix is
  open against `eclipse-4diac/4diac-documentation`
  ([#120](https://github.com/eclipse-4diac/4diac-documentation/pull/120));
  until it merges, `eclipse-4diac/4diac-documentation`'s `CONTRIBUTING.md`
  still has the wrong reference — don't report this specific mismatch as a
  new finding there while #120 is open. This suppression is scoped to that
  one upstream repo: a fork's own `CONTRIBUTING.md` is an independent copy
  that #120 doesn't touch, so if a fork hasn't synced from upstream, its
  copy staying wrong is a separate, still-worth-flagging fact about that
  fork being behind — not the same tracked issue.
- The three repos' `CONTRIBUTING.md` files are otherwise structurally
  identical (same template) with no contradictions between them, only
  repo-specific gaps (e.g. forte's is missing the "usability and UI
  improvements" contribution-type bullet that ide/documentation have) —
  that's not necessarily worth a PR on its own, just don't be surprised by
  the asymmetry.
- If you find a NEW contradiction or error while doing this check, propose
  the fix rather than silently working around it — a wrong guideline is
  itself worth reporting/fixing, same as a wrong code comment.

## Quick pre-commit / pre-PR checklist

Run through this explicitly, every time, in these repos:

1. Commit subject ≤ 50 chars, body wrapped ~72 chars?
2. Zero URLs, zero `#<number>` issue/PR references anywhere in the commit
   message (title or body)?
3. Any new/touched copyright header has exactly **one** year (the file's
   original creation year), never a range?
4. Comments are in **English**, and no longer than the surrounding file's
   own comment style?
5. No new speculative version bump without solid precedent?
6. No new loose `.md`/`.adoc`/`.asciidoc` file unless it's genuine, scoped,
   permanent documentation, in the format the target repo actually uses?
7. PR description matches the *current* diff — not a stale description of
   an earlier version of the change?
8. PR description (not commit) carries the issue/PR cross-references?
9. Commit footer has an `Assisted-by: [Provider] [Model-Family]
   ([Version/ID])` trailer disclosing the AI tool used, alongside any
   `Co-Authored-By:`/`Also-by:` co-author trailer?
