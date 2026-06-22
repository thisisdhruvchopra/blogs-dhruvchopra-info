# CONTEXT.md

A running context file for this project. I (Claude) keep this updated as work happens so future sessions have a quick orientation. Most recent entries go at the top of the Worklog.

_Last updated: 2026-06-16_

## Project at a glance

- **What:** Dhruv Chopra's personal blog — a static, framework-free site with themed topic pages, each having its own visual identity and CSS/JS animations.
- **Stack:** Plain HTML + a shared `styles.css` + per-page `<style>` blocks. No build step. Vanilla JS only, no dependencies.
- **Hosting:** GitHub Pages. Custom domain via `CNAME` → `blogs.dhruvchopra.info`.
- **Branch:** `main` (also the deploy branch).

## Layout

```
index.html          Home: topic hub grid + recent posts
about.html          About page
blog.html           All posts with category filter
music.html          Music topic page (newest; untracked in git)
styles.css          Shared base stylesheet (nav, footer, post layout, typography)

Topic pages (each self-themed):
  travel.html       Flag strip, passport stamps, country cards
  astrophysics.html Animated starfield canvas, constellation SVG
  football.html     SVG pitch, animated rolling ball
  coffee.html       Crema ring, steam animation, flavour wheel
  cinema.html       Film strip sprockets, projector beam
  perfumes.html     Floating scent particles, olfactive pyramid
  history.html      Parchment aesthetic, animated timeline
  tech.html         CRT terminal, scanline effect, typing animation
  sports.html       (present in repo)

posts/
  hello-world.html
  three-trips-five-countries.html
  on-scale-and-silence.html
```

## Conventions

- Each topic page layers a page-specific `<style>` block on top of `styles.css`.
- Animations are CSS-only or lightweight vanilla JS.
- Post categories (`data-category` on `<li>`): `travel`, `astrophysics`, `cinema`, `coffee`, `football`, `history`, `perfumes`, `tech`, `personal`.
- Adding a post: duplicate a file in `posts/`, update title/tag/date/body, then add rows to `blog.html` and `index.html`.

## Current state (uncommitted)

Per git status at session start:
- Modified: `about.html`, `astrophysics.html`, `blog.html`, `index.html`, `perfumes.html`, `styles.css`, `travel.html`
- Untracked: `music.html`

## Open questions / notes

- `README.md` still lists eight topic pages, but the repo also has `sports.html` and `music.html` — README is slightly out of date.
- `README.md` mentions custom domain `dhruvchopra.info`, while `CNAME` is `blogs.dhruvchopra.info`.

## Worklog

- **2026-06-16** — Created this CONTEXT.md and seeded it with a project overview from `README.md`, the file listing, and git status.
