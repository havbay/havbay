# GitHub Profile README Design

Date: 2026-08-12

## Purpose

Replace the generated badge wall and third-party statistics with a concise GitHub profile README that feels personal, technically focused, and easy to maintain.

The README should introduce HavBay without assigning a fixed professional title or implying a career change. It should communicate curiosity, enjoyment of cybersecurity, and hands-on learning.

## Audience

The primary audience is anyone visiting the `havbay` GitHub profile, including other security learners, CTF players, developers, and potential collaborators.

Within a few seconds, a visitor should understand:

- the profile belongs to HavBay;
- cybersecurity and continuous learning are central interests;
- the main areas of interest are web security, API security, reverse engineering, and CTF participation; and
- the technologies listed are a small, intentional selection rather than an exhaustive inventory.

## Content hierarchy

The README will contain four elements in this order:

1. A centered `~/0xHavBay` heading.
2. A `$ whoami` section with the personal introduction.
3. A `$ interests` section containing four focus areas.
4. A `$ technologies` section containing six technologies.

The README will not include a footer or additional promotional sections.

## Approved copy

The `$ whoami` section will use this text:

> I love cybersecurity, researching unfamiliar topics, and learning through hands-on experimentation. Curiosity keeps me exploring, building, and improving every day.

The README will use only the HavBay identity. It will not introduce the profile with the owner's real name.

## Interests

The interests will be presented with exactly these labels:

- Web Security
- API Security
- Reverse Engineering
- CTF Player

`CTF Player` is intentional and must not be changed to `CTF Challenges`.

## Technologies

The visible technology set will be limited to:

- Python
- JavaScript
- Kubernetes
- Vercel
- Linux
- Git

These will appear as compact, consistently styled flat badges. Labels must remain readable without relying on icon recognition alone.

## Visual direction

The design is "technical but human":

- dark-theme-compatible presentation;
- terminal-inspired section headings;
- restrained use of green as an accent;
- compact badges instead of a dense technology wall; and
- enough spacing to preserve a clear reading hierarchy.

The content must remain legible in both GitHub light and dark themes. The implementation must not depend on custom JavaScript, HTML styling unsupported by GitHub Markdown, or a custom animated asset.

## Explicit exclusions

The README will not include:

- `Hi, I'm Nara` or another real-name introduction;
- the `curiosity_driven · always_learning` tagline;
- portfolio, cheatsheet, or contact links already present in the GitHub profile bio;
- featured-project cards already represented by pinned repositories;
- GitHub statistics or language cards;
- streak, trophy, or profile-view counters;
- meme images or GIFs; or
- the generated-profile attribution currently at the bottom of the README.

## Dependencies and failure behavior

Only the six technology badges may depend on an external badge-rendering service. Each badge must include useful alternative text so the technology remains identifiable if an image does not load.

No core introduction or interest text will be embedded in images. A badge-service failure must therefore affect decoration only, not the meaning of the profile.

## Validation

Implementation is complete when:

- the rendered hierarchy matches this specification;
- the introduction and all ten approved labels are spelled and capitalized correctly;
- every excluded element has been removed;
- all badge URLs resolve successfully;
- the Markdown contains useful image alternative text;
- the README remains readable as plain Markdown if images fail; and
- `git diff --check` reports no whitespace errors.

## Scope

Implementation is limited to replacing `README.md`. No portfolio source, cheatsheet content, GitHub profile metadata, pinned repositories, deployment configuration, or repository visibility settings will be changed.
