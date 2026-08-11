# Red Heading and Profile Views Design

Date: 2026-08-12

## Purpose

Give the GitHub profile README a stronger visual identity by replacing the plain text heading with a static red SVG heading and adding a compact profile-view counter.

This revision extends the approved GitHub profile README without changing its introduction, interests, or technology claims.

## Heading asset

Create `assets/header.svg` with these properties:

- transparent background;
- `720 × 100` view box;
- centered `~/0xHavBay` text;
- red `#ff3b45` fill;
- bold monospace font stack with common system fallbacks;
- no animation, scripts, remote fonts, or embedded raster content; and
- an SVG title and description identifying the HavBay heading.

The SVG will be stored in the repository so the heading does not depend on an external image-generation service.

## README placement

The heading and view counter will appear together at the top of `README.md` in one centered HTML paragraph:

1. the local SVG heading at a maximum displayed width of `700` pixels;
2. a line break; and
3. the profile-view badge directly beneath the heading.

Both images will have descriptive Markdown alternative text. The existing `$ whoami` section will follow without other layout changes.

## Profile-view counter

Use this external badge endpoint:

`https://komarev.com/ghpvc/?username=havbay&style=flat-square&color=ff3b45&label=PROFILE+VIEWS`

The badge is an approximate third-party request counter. It must not be described as unique-user analytics or an exact measurement of individual visitors.

If the service is unavailable, only the counter image will be missing. The heading, introduction, interests, and technology labels must remain readable.

## Existing content

Keep these sections and their wording unchanged:

- `$ whoami`;
- `$ interests`; and
- `$ technologies`.

The visible technology set remains exactly:

- Python
- JavaScript
- Kubernetes
- Vercel
- Linux
- Git

Supabase and Firebase are not included. They can be reconsidered after hands-on application-building experience, but that future decision is outside this revision.

## Explicit exclusions

This revision will not add:

- animated or typing text;
- a meme or GIF;
- GitHub statistics, language cards, streaks, or trophies;
- portfolio, cheatsheet, or contact links;
- Supabase or Firebase badges; or
- JavaScript, custom web fonts, or other executable content.

## Validation

Implementation is complete when:

- `assets/header.svg` is well-formed XML;
- the SVG contains the exact `~/0xHavBay` text and `#ff3b45` fill;
- the SVG contains no script, animation, external URL, or raster image reference;
- GitHub's Markdown renderer preserves the centered heading and counter images;
- the counter endpoint returns HTTP `200`;
- the existing introduction, four interests, and six technology badges are unchanged;
- Supabase and Firebase remain absent;
- image alternative text remains meaningful if either image fails; and
- `git diff --check` reports no whitespace errors.

## Scope

Implementation is limited to creating `assets/header.svg` and updating the heading block at the top of `README.md`. No other repository content, profile metadata, pinned repository, deployment setting, or repository visibility will change.
