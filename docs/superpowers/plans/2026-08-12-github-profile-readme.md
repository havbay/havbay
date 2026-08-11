# GitHub Profile README Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the generated GitHub profile README with a concise, personal introduction to HavBay's interests and six selected technologies.

**Architecture:** This is a single-file GitHub-flavored Markdown change. Human-readable text carries all essential meaning, while six flat Shields.io badges provide optional visual enhancement with descriptive alternative text.

**Tech Stack:** GitHub-flavored Markdown, supported inline HTML, Shields.io badges, `rg`, `curl`, Git

## Global Constraints

- Modify only `README.md`; do not change portfolio source, cheatsheet content, GitHub profile metadata, pinned repositories, deployment configuration, or repository visibility.
- Use only the HavBay identity; do not introduce the owner's real name.
- Preserve this introduction verbatim: `I love cybersecurity, researching unfamiliar topics, and learning through hands-on experimentation. Curiosity keeps me exploring, building, and improving every day.`
- Use exactly these interest labels: `Web Security`, `API Security`, `Reverse Engineering`, and `CTF Player`.
- Use exactly these technology labels: `Python`, `JavaScript`, `Kubernetes`, `Vercel`, `Linux`, and `Git`.
- Do not add a tagline, portfolio links, cheatsheet links, contact links, featured projects, statistics, language cards, streaks, trophies, view counters, memes, GIFs, or generated-profile attribution.
- Keep all essential introduction and interest text as Markdown text, not images.
- Keep the README readable if every badge image fails to load.

## File structure

- Modify: `README.md` — the complete public GitHub profile introduction.
- Reference only: `docs/superpowers/specs/2026-08-12-github-profile-readme-design.md` — the approved requirements and exclusions.

No new runtime, asset, style, test, or configuration files are required.

---

### Task 1: Replace and validate the GitHub profile README

**Files:**

- Modify: `README.md`
- Reference: `docs/superpowers/specs/2026-08-12-github-profile-readme-design.md`

**Interfaces:**

- Consumes: GitHub's Markdown renderer and six HTTPS badge URLs from `img.shields.io`.
- Produces: A standalone profile README whose essential meaning remains available as plain Markdown.

- [ ] **Step 1: Run structural checks against the current README and confirm they fail**

Run these commands individually:

```bash
rg -F '<h1 align="center"><code>~/0xHavBay</code></h1>' README.md
rg -F 'I love cybersecurity, researching unfamiliar topics, and learning through hands-on experimentation. Curiosity keeps me exploring, building, and improving every day.' README.md
rg -F 'CTF Player' README.md
```

Expected: each command exits with status `1` because the current generated README does not contain the approved heading, introduction, or interest label.

- [ ] **Step 2: Replace `README.md` with the approved content**

Use this exact content:

```markdown
<h1 align="center"><code>~/0xHavBay</code></h1>

## `$ whoami`

I love cybersecurity, researching unfamiliar topics, and learning through hands-on experimentation. Curiosity keeps me exploring, building, and improving every day.

## `$ interests`

- Web Security
- API Security
- Reverse Engineering
- CTF Player

## `$ technologies`

![Python](https://img.shields.io/badge/Python-161B22?style=flat-square&logo=python&logoColor=3FB950)
![JavaScript](https://img.shields.io/badge/JavaScript-161B22?style=flat-square&logo=javascript&logoColor=3FB950)
![Kubernetes](https://img.shields.io/badge/Kubernetes-161B22?style=flat-square&logo=kubernetes&logoColor=3FB950)
![Vercel](https://img.shields.io/badge/Vercel-161B22?style=flat-square&logo=vercel&logoColor=3FB950)
![Linux](https://img.shields.io/badge/Linux-161B22?style=flat-square&logo=linux&logoColor=3FB950)
![Git](https://img.shields.io/badge/Git-161B22?style=flat-square&logo=git&logoColor=3FB950)
```

- [ ] **Step 3: Verify the required content and exclusions**

Run:

```bash
rg -n '^<h1 align="center"><code>~/0xHavBay</code></h1>$|^## `\$ (whoami|interests|technologies)`$|^I love cybersecurity|^- (Web Security|API Security|Reverse Engineering|CTF Player)$|^!\[(Python|JavaScript|Kubernetes|Vercel|Linux|Git)\]' README.md
```

Expected: exactly 15 lines—one heading, three section headings, one introduction, four interests, and six technology badges.

Run:

```bash
rg -n -i 'Nara|curiosity_driven|always_learning|0xhavbay\.dev|gitbook\.io|github-readme-stats|streak-stats|profile-trophy|ghpvc|komarev|meme|\.gif|Proudly created' README.md
```

Expected: exit status `1` with no output because every excluded element is absent.

- [ ] **Step 4: Verify every badge endpoint**

Run each request individually:

```bash
curl --fail --silent --show-error --head 'https://img.shields.io/badge/Python-161B22?style=flat-square&logo=python&logoColor=3FB950'
curl --fail --silent --show-error --head 'https://img.shields.io/badge/JavaScript-161B22?style=flat-square&logo=javascript&logoColor=3FB950'
curl --fail --silent --show-error --head 'https://img.shields.io/badge/Kubernetes-161B22?style=flat-square&logo=kubernetes&logoColor=3FB950'
curl --fail --silent --show-error --head 'https://img.shields.io/badge/Vercel-161B22?style=flat-square&logo=vercel&logoColor=3FB950'
curl --fail --silent --show-error --head 'https://img.shields.io/badge/Linux-161B22?style=flat-square&logo=linux&logoColor=3FB950'
curl --fail --silent --show-error --head 'https://img.shields.io/badge/Git-161B22?style=flat-square&logo=git&logoColor=3FB950'
```

Expected: every request returns an HTTP `200` response. If an endpoint fails, correct only that badge URL and rerun its request; do not replace textual content with images.

- [ ] **Step 5: Perform final local validation**

Run:

```bash
git diff --check -- README.md
git diff -- README.md
git status --short
```

Expected:

- `git diff --check` produces no output and exits with status `0`.
- The diff shows only the approved replacement of `README.md`.
- Status shows `README.md` modified plus any pre-existing untracked visual-companion files; those companion files must not be staged.

- [ ] **Step 6: Commit only the README implementation**

Run:

```bash
git add -- README.md
git diff --cached --check
git diff --cached --name-only
git commit -m "docs: rebuild GitHub profile README"
```

Expected:

- The staged-file list contains only `README.md`.
- The commit succeeds with message `docs: rebuild GitHub profile README`.
- Nothing is pushed; publishing requires a separate explicit instruction.
