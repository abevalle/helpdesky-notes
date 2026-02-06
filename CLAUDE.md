# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Helpdesky.net is a single-file (`index.html`) browser-based incident logging tool for helpdesk reps. It uses in-browser AI via web-llm (WebGPU) for smart compose and session summaries. Deployed as a static file to CDN (helpdesky.net).

## Development

```bash
# Serve locally for testing
npx serve .

# Deploy to Cloudflare Pages
npx wrangler pages deploy . --project-name helpdesky
```

Test in a WebGPU-capable browser (Chrome 113+, Edge 113+). AI features require GPU; the app works without them.

There is no build step, no bundler, no tests, no linter. The entire app is `index.html`.

## Architecture

Everything lives in a single HTML file with three sections:

1. **`<style>`** — All CSS using custom properties (CSS variables) for theming. Every color is a `var(--...)` reference; the `:root` block defines defaults.

2. **`<body>` HTML** — Setup modal, confirm modal, toast, sidebar, main area (toolbar, log display, status bar, input). No templates or frameworks.

3. **`<script type="module">`** — Vanilla JS, structured as sequential sections:
   - **IndexedDB layer** (`db` object) — Two stores: `sessions` (structured entries, keyPath: `id`) and `preferences` (key-value, keyPath: `key`). Helper methods: `db.getPref(key)`, `db.setPref(key, value)`.
   - **Themes** — `THEMES` object with preset color maps, `applyTheme(name)` sets CSS variables on `document.documentElement.style` and updates favicon/meta.
   - **Utilities** — `debounce`, `compressImage` (800px/0.7 JPEG), `showToast`.
   - **Rendering** — XSS-safe via `textContent`/DOM APIs only. Never use `innerHTML` with user content.
   - **Session management** — Create, switch, delete, clear. Debounced saves (300ms).
   - **Timer** — Three modes: auto (start on keystroke), manual, disabled.
   - **AI** — Dynamic import of web-llm from `esm.run`. OOM auto-fallback to smaller models. Ghost text suggestions (debounced 600ms) and one-click summaries.
   - **Setup/Settings modal** — First-launch setup or settings mode. Handles theme, model selection, timer mode. Preferences persisted in IndexedDB.
   - **Init** — Loads saved theme, timer mode, then either shows setup modal or loads sessions + AI.

## Key Constraints

- **Single-file architecture** — No external JS/CSS files, no npm dependencies in the build. web-llm is loaded via ESM CDN import. Fira Code font from Google Fonts.
- **XSS safety** — All user content rendered via `textContent` or DOM element creation. Never `innerHTML` for user data.
- **Theming** — All colors must use CSS custom properties. When adding new UI, use existing variables (`--bg`, `--sidebar-bg`, `--text`, `--amber`, `--cyan`, `--border`, `--surface`, `--hover`, `--text-bright`, `--text-secondary`, `--text-muted`, `--text-dim`, `--placeholder`, `--input-bg`, `--danger`, `--ghost`, `--focus-glow`, `--ai-bg`). Never hardcode color values. New themes go in the `THEMES` JS object.
- **Images** — Compressed to 800px max width, 0.7 JPEG quality before IndexedDB storage.
- **Sidebar rebuilds** — Only on session create/delete/switch, not on every log entry (performance).
