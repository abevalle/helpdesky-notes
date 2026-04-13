# Helpdesky.net

**Fast incident logging for helpdesk reps who can't afford to slow down.**

Helpdesky is a zero-install, browser-based note-taking tool built for MSP technicians, helpdesk teams, and IT support reps. Open a tab, start logging. AI summarizes your notes when the call is done.

[Live App](https://helpdesky.net) | Single HTML file | No account required | Works offline after first load

---

## Why Helpdesky?

Helpdesk reps juggle 40+ tickets a day. You need a tool that loads instantly, captures everything, and doesn't require a login. Helpdesky runs entirely in your browser — no server, no subscription, no data leaving your machine.

- **Zero setup** — Open the URL. Start typing. That's it.
- **AI-powered summaries** — One click turns your raw notes into structured bullet points (issue, steps taken, resolution).
- **Smart Compose** — Ghost text suggestions appear as you type, powered by a local AI model running on your GPU. Tab to accept.
- **100% local & private** — Your notes never leave the browser. AI runs on-device via WebGPU. No cloud. No API keys. No telemetry.
- **Built for MSPs** — Caller name, callback number, and ticket ID are first-class fields. Copy Raw exports clean text ready to paste into ConnectWise, Autotask, Halo, or any PSA.

---

## Features

### Incident Logging
- Timestamped log entries — press Enter to send
- Paste screenshots directly into the log (auto-compressed for storage)
- Caller name, callback number, and ticket ID fields built into the log view
- Session management — create, switch, delete, and clear sessions
- Full session history in the sidebar, sorted by most recent

### AI Assistant (Local, Private)
- **Inline ghost text** — Predictive completions appear as you type, like Google Smart Compose. Press Tab to accept.
- **One-click summaries** — AI reads your session notes and generates a structured summary with issue description, troubleshooting steps, and resolution.
- **Choose your model** — Pick from built-in options (SmolLM 360M, Qwen 0.5B, Llama 1B, Phi 3.5 Mini) or enter a custom model ID for self-hosted compliance.
- **Model caching** — Downloads once, loads from browser cache on every subsequent visit.
- **GPU memory safe** — Automatically falls back to a smaller model if your GPU runs out of memory.

### Data & Privacy
- All data stored in IndexedDB (browser-local, no 5MB localStorage limit)
- Export all sessions as JSON backup with one click
- Copy Raw — formatted plain text ready to paste into your PSA/ticketing system
- No accounts, no cookies, no tracking, no server-side anything
- Self-hostable — deploy the single HTML file on your own infrastructure

### Keyboard Shortcuts
| Shortcut | Action |
|----------|--------|
| `Enter` | Send log entry |
| `Tab` | Accept AI suggestion |
| `Escape` | Dismiss suggestion |
| `Alt+N` | New session |
| `Alt+C` | Copy raw text |
| `Alt+S` | AI summary |

---

## Deployment

Helpdesky is a single `index.html` file. Deploy it anywhere that serves static files.

### CDN / Static Host
Upload `index.html` to Cloudflare Pages, Netlify, Vercel, GitHub Pages, S3, or any static host.

### Self-Hosted (MSP Intranet)
Drop the file on any internal web server. Your techs get a private, zero-dependency logging tool with AI that runs entirely on their workstations.

```bash
# Example: serve locally
npx serve .

# Example: deploy to Cloudflare Pages
npx wrangler pages deploy . --project-name helpdesky
```

### Requirements
- Modern browser with WebGPU support (Chrome 113+, Edge 113+)
- GPU with 512MB+ VRAM for AI features (SmolLM 360M works on integrated graphics)
- AI is optional — the app works fully without it on any browser

---

## Custom AI Models

On first launch, Helpdesky asks you to pick an AI model. You can also enter a custom web-llm model ID for:

- **Compliance** — Run a model hosted on your own infrastructure
- **Air-gapped environments** — Pre-cache models on managed workstations
- **Specialized models** — Use any model compatible with [web-llm](https://github.com/mlc-ai/web-llm)

Change your model anytime via the AI Settings button in the toolbar.

---

## Built For

- **MSP helpdesk teams** logging calls and tickets at speed
- **IT support reps** who need structured notes without structured tools
- **NOC technicians** tracking incident timelines
- **Service desk analysts** who want AI summaries without sending data to the cloud
- **Solo IT consultants** who need a fast, portable logging tool

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Frontend | Single HTML file, vanilla JS, CSS |
| Storage | IndexedDB (structured sessions + preferences) |
| AI Engine | [web-llm](https://github.com/mlc-ai/web-llm) via WebGPU |
| Font | Fira Code (Google Fonts) |
| Hosting | Any static file server / CDN |

---

## Analytics

This repo includes a Google Analytics tag (`G-PDHF20QMTB`) in `index.html` for the official helpdesky.net deployment. **If you are self-hosting or forking this project, remove the `<!-- Google tag (gtag.js) -->` script block from the `<head>` of `index.html`** so your users' traffic isn't reported to our analytics.

---

## Contributing

Helpdesky is a single-file app by design. PRs that keep it that way are welcome.

1. Fork the repo
2. Edit `index.html`
3. Test in a WebGPU-capable browser
4. Open a PR

---

## License

MIT

---

<sub>Built for the techs in the trenches. If you've ever typed "user states printer not working" 50 times in one shift, this is for you.</sub>
