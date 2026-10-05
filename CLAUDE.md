# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Commands

```bash
npm run dev     # next dev
npm run build   # next build
npm run start   # next start (after build)
npm run lint    # eslint (flat config, eslint.config.mjs)
```

No test runner configured. Path alias: `@/*` maps to repo root.

## Stack

Next.js 16.3.8 (App Router, `app/`), React 19, TypeScript strict, Tailwind v4 (via `@tailwindcss/postcss`). Per AGENTS.md, this Next.js has breaking changes — read `node_modules/next/dist/docs/` before writing Next code. Note `app/layout.tsx` uses the global `LayoutProps<"/">` type helper.

## State of the repo

`app/` is still the untouched Create Next App scaffold (default `page.tsx`, "Create Next App" metadata). The real product — Arcade Vault, a retro-arcade portal to play games and compete for high scores (UI text in Spanish) — exists only as a static prototype in `resources/templates/` (untracked) that must be ported into the Next app.

README says the project follows Spec Driven Design (`/spec` and `/spec-impl` workflow, skills from `Klerith/fernando-skills`).

## Prototype in `resources/templates/`

Standalone in-browser app: `Arcade Vault.html` loads React 18 UMD + Babel standalone from unpkg, then each `*.jsx` as `type="text/babel"` script sharing one global scope (no imports/exports). Hence hooks are aliased per file (e.g. `useStateApp` in `app.jsx`) to avoid global name collisions — drop these aliases when porting.

- `data.jsx` — mock data (`GAMES` etc.)
- `app.jsx` — root `App`; hash-based router, route is JSON in `location.hash` (`{name: "biblioteca" | "detalle" | "player" | "auth" | "salon", id?}`); no real routing
- Screens: `biblioteca.jsx` (library), `detalle.jsx` (game detail), `reproductor.jsx` (game player), `auth.jsx`, `salon.jsx` (hall of fame), `nav.jsx`
- Persistence is `localStorage` only: `av_user` (session), `av_scores` (array of score entries with `at` timestamp). No backend.
- `styles.css` — design tokens as CSS vars (`--line`, `--ink-faint`, `--mono`, ...) and `av-*` classes; neon/pixel look (Press Start 2P, Courier Prime, JetBrains Mono fonts)

When porting: hash routes become App Router routes, `localStorage` code needs `"use client"` components, and the CSS-var theme should be carried into `app/globals.css`.
