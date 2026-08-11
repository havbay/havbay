# Red Heading and Profile Views Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a repository-owned red SVG heading and a centered profile-view counter to the existing GitHub profile README.

**Architecture:** A static, transparent SVG provides the styled heading without scripts, animation, or remote fonts. `README.md` embeds that local asset and a Komarev view badge in one centered block while leaving all existing introduction, interest, and technology content unchanged.

**Tech Stack:** SVG 1.1-compatible XML, GitHub-flavored Markdown, supported inline HTML, Komarev profile-view badge, Python standard-library XML parser, GitHub CLI, `rg`, `curl`, Git

## Global Constraints

- Create only `assets/header.svg` and modify only the heading block at the top of `README.md`.
- Use a transparent `720 × 100` SVG view box.
- Render the exact text `~/0xHavBay` centered with fill `#ff3b45`, a bold monospace system-font stack, and no animation.
- Include SVG title and description metadata.
- Do not use scripts, remote fonts, raster images, external resource references, or executable content in the SVG.
- Use exactly this counter endpoint: `https://komarev.com/ghpvc/?username=havbay&style=flat-square&color=ff3b45&label=PROFILE+VIEWS`.
- Treat the counter as approximate; do not describe it as unique-user analytics.
- Keep `$ whoami`, `$ interests`, `$ technologies`, the introduction, all four interests, and all six existing technology badges unchanged.
- Do not add Supabase, Firebase, animations, memes, GIFs, statistics, language cards, streaks, trophies, or external profile links.

## File structure

- Create: `assets/header.svg` — repository-owned red heading artwork with accessible SVG metadata.
- Modify: `README.md` — centers and embeds the local heading followed by the view counter.
- Reference only: `docs/superpowers/specs/2026-08-12-red-heading-profile-views-design.md` — approved revision requirements.

---

### Task 1: Add the styled heading and profile-view counter

**Files:**

- Create: `assets/header.svg`
- Modify: `README.md:1`
- Reference: `docs/superpowers/specs/2026-08-12-red-heading-profile-views-design.md`

**Interfaces:**

- Consumes: GitHub's repository asset renderer and the Komarev badge endpoint for username `havbay`.
- Produces: A centered two-image heading block whose local heading still has useful alternative text if image rendering fails.

- [ ] **Step 1: Run fail-first checks against the current repository**

Run these commands individually:

```bash
test -f assets/header.svg
rg -F 'komarev.com/ghpvc/?username=havbay' README.md
```

Expected: both commands exit with status `1`. The asset and counter must be absent before implementation.

- [ ] **Step 2: Create `assets/header.svg`**

Create the file with exactly this content:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 720 100" role="img" aria-labelledby="title description">
  <title id="title">0xHavBay</title>
  <desc id="description">Red monospace heading reading ~/0xHavBay</desc>
  <text
    x="360"
    y="50"
    dominant-baseline="middle"
    text-anchor="middle"
    fill="#ff3b45"
    font-family="ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, 'Liberation Mono', 'Courier New', monospace"
    font-size="42"
    font-weight="700"
  >~/0xHavBay</text>
</svg>
```

- [ ] **Step 3: Replace only the first heading line in `README.md`**

Replace:

```html
<h1 align="center"><code>~/0xHavBay</code></h1>
```

With:

```html
<p align="center">
  <img src="./assets/header.svg" alt="~/0xHavBay" width="700">
  <br>
  <img src="https://komarev.com/ghpvc/?username=havbay&style=flat-square&color=ff3b45&label=PROFILE+VIEWS" alt="HavBay profile views">
</p>
```

- [ ] **Step 4: Validate the SVG and reject unsafe or unsupported elements**

Run:

```bash
python3 -c 'import xml.etree.ElementTree as ET; ET.parse("assets/header.svg"); print("valid_xml=1")'
rg -n 'viewBox="0 0 720 100"|fill="#ff3b45"|>~/0xHavBay</text>|<title id="title">|<desc id="description">' assets/header.svg
```

Expected: XML parsing prints `valid_xml=1`, and `rg` returns five matching lines for the view box, fill, exact heading text, title, and description.

Run:

```bash
rg -n -i '<script|<animate|<set|<image|<foreignObject|href=|@import|url\(' assets/header.svg
```

Expected: exit status `1` with no output because the SVG contains no script, animation, raster image, embedded HTML, external resource, or CSS URL reference.

- [ ] **Step 5: Verify README content and the view-counter endpoint**

Run:

```bash
rg -n '^<p align="center">$|header\.svg|komarev\.com/ghpvc/|^## `\$ (whoami|interests|technologies)`$|^I love cybersecurity|^- (Web Security|API Security|Reverse Engineering|CTF Player)$|^!\[(Python|JavaScript|Kubernetes|Vercel|Linux|Git)\]' README.md
```

Expected: exactly 17 matching lines—one centered block opener, one heading image, one counter image, three section headings, one introduction, four interests, and six technology badges.

Run:

```bash
rg -n -i 'Supabase|Firebase|readme-typing|\.gif|github-readme-stats|streak-stats|profile-trophy' README.md assets/header.svg
```

Expected: exit status `1` with no output.

Run:

```bash
curl --fail --silent --show-error --output /dev/null --write-out '%{http_code} %{content_type}\n' --head 'https://komarev.com/ghpvc/?username=havbay&style=flat-square&color=ff3b45&label=PROFILE+VIEWS'
```

Expected: HTTP `200` with an SVG image content type.

- [ ] **Step 6: Verify GitHub rendering and review the complete diff**

Run:

```bash
gh api --method POST markdown -f mode=gfm -f context=havbay/havbay -f text="$(< README.md)" | rg -o '<p align="center"[^>]*>|<img[^>]*alt="(~/0xHavBay|HavBay profile views)"[^>]*>|<h2[^>]*>'
git diff --check -- README.md assets/header.svg
git diff -- README.md assets/header.svg
git status --short
```

Expected:

- GitHub's renderer returns one centered paragraph, both images with their alternative text, and three section headings.
- `git diff --check` exits `0` with no output.
- The diff contains only the new SVG and the approved README heading-block replacement.
- `.superpowers/` remains untracked and must not be staged.

- [ ] **Step 7: Commit only the implementation files**

Run:

```bash
git add -- README.md assets/header.svg
git diff --cached --check
git diff --cached --name-only
git commit -m "docs: add red profile heading and view counter"
```

Expected:

- The staged-file list contains only `README.md` and `assets/header.svg`.
- The commit succeeds with message `docs: add red profile heading and view counter`.
- Nothing is pushed without a separate explicit publication instruction.
