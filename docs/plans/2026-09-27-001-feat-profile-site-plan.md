# feat: ITCS333 Assignment 1 — profile site + git workflow

**Depth:** Lightweight
**Target repo:** abdyas2510/itcs333-a1-html-github (fork of ITCS333/itcs333-a1-html-github)

## Summary

Build a real two-page profile site (`index.html`, `about.html`) that satisfies the
autograder in `tests/checks/run.mjs` (70 pts, content) and `tests/git_checks.sh`
(30 pts, git workflow), then merge it through an actual PR so the git-history
check finds a genuine `Merge pull request #N` commit. Remaining 30% of the
assignment grade is the in-class discussion — not buildable, just show up.

## Problem Frame

The starter repo (`index.html` / `about.html`) is all TODO comments. Nothing to
extend — this is a from-scratch build against a known, already-read grading
script, not an ambiguous spec. Confirmed by reading `tests/checks/run.mjs` and
`tests/git_checks.sh` directly rather than trusting the README's rubric table
(the table's numbering doesn't match the scripts' check IDs 1:1, but the
scripts are what actually decides the grade).

## Requirements (traced to exact grader checks)

| Grader check | Points | What satisfies it |
|---|---|---|
| `html_valid_boilerplate` | 15 | `<!DOCTYPE html>`, `<html lang="en">`, non-empty `<title>`, passes html-validate:recommended on both pages |
| `semantic_elements` | 15 | `header`, `nav`, `main`, `footer` present on **both** pages |
| `heading_hierarchy` | 10 | First heading is `h1`, no level skipped, on **both** pages |
| `links` | 10 | `index.html` → `about.html`, `about.html` → `index.html`, ≥1 `http(s)://` external link anywhere |
| `image_alt` | 10 | ≥1 `<img>` with non-empty `alt` anywhere across the two pages |
| `lists` | 10 | ≥1 `<ol><li>` and ≥1 `<ul><li>` anywhere across the two pages |
| `git_history` | 20 | ≥3 commits, messages not matching the trivial-message blocklist (`update`/`fix`/`wip`/etc.) |
| `merged_pr` | 10 | A merge commit whose message contains "merge pull request" — i.e. GitHub's default **merge commit** strategy, not squash/rebase |

## Key Technical Decisions

- **Content is real, not filler.** Profile is Abdulla "Kaizaar" Yasser — network
  engineering student, University of Bahrain (2026-2027 SemI) — not lorem ipsum.
- **One shared visual style** (`assets/style.css`) across both pages — cheap to
  add, and the 30-point in-class discussion is literally about presenting this
  page, so bare unstyled HTML undersells it.
- **Image = existing GitHub avatar** (`assets/avatar.png`), not a new asset to
  source — already his own image, satisfies `image_alt` with zero extra work.
- **List split:** unordered list (skills/interests) on `index.html`, ordered
  list (a "this semester" timeline) on `about.html` — the check only requires
  one of each *somewhere*, not both per page.
- **PR merge strategy: "Create a merge commit"**, not squash/rebase — squash
  and rebase don't produce the `Merge pull request #N` commit message
  `git_checks.sh` greps for.

## Implementation Units

### U1. `index.html` — home page

**Goal:** Semantic home page with real intro content, nav to About, and the
unordered list + image.
**Files:** `index.html`, `assets/style.css` (created here, shared by U2)
**Approach:** `header` (site title + `nav` linking `index.html`/`about.html`),
`main` with `h1` name/tagline, intro paragraph, `assets/avatar.png` with
descriptive `alt`, `h2` "What I'm into" + `ul` of interests/skills, `footer`
with a copyright line.
**Verification:** Open in a browser locally; visually matches intended layout,
nav link to About works.

### U2. `about.html` — about page

**Goal:** Semantic about page with real background content, nav back to home,
the ordered list, and the external link.
**Files:** `about.html`
**Approach:** Same `header`/`nav` shared shell as U1 (linking back to
`index.html`), `main` with `h1` "About Me", background paragraph (network
engineering @ University of Bahrain), `h2` "This semester" + `ol` of current
coursework/focus areas, external link to `https://www.uob.edu.bh` in body
copy, `footer`.
**Dependencies:** U1 (shares `assets/style.css`)
**Verification:** Nav link back to Home works; external link opens in a new
context; heading order is h1 then h2 only (no skipped levels).

### U3. Git workflow — branch, commits, PR, merge

**Goal:** Satisfy `git_history` (20 pts) and `merged_pr` (10 pts).
**Approach:** Work on `feature/profile-content` off `master`. Land at least 3
non-trivial commits as the two pages are built (e.g. one for shared
shell+home, one for about+timeline, one for style/asset polish) — not
mechanically split just to hit a count, each commit should be a real,
describable step. Open a PR from the feature branch into `master` in the
fork, merge via GitHub's **merge commit** option (`gh pr merge --merge`).
**Verification:** `git log --oneline` shows ≥3 non-trivial messages plus one
`Merge pull request #N` commit after merge.

### U4. Local verification before submission

**Goal:** Catch failures before Blackboard submission / class discussion.
**Approach:** Run `./run_tests.sh` (installs `tests/node_modules` on first
run, then runs `tests/checks/run.mjs`) and confirm `TOTAL 70/70`. Separately
re-derive the `git_checks.sh` logic against the real `git log` output since
`run_tests.sh` doesn't invoke it directly.
**Dependencies:** U1, U2, U3
**Verification:** `TOTAL 70/70` printed by the test runner; manual read of
`git log --oneline --graph` confirms ≥3 meaningful commits and a merge-pull-request commit.

## Scope Boundaries

- Not touching `tests/` or `.github/` — explicit academic-integrity rule in the
  assignment README.
- Not adding a build step, framework, or JS beyond what's already in the
  starter — this is a static HTML5 exercise.
- Blackboard submission (pasting the fork URL) and attending the class
  discussion are follow-up actions outside this repo, not implementation
  units.
