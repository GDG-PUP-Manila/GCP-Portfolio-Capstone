# AGENTS.md

GCP Portfolio Capstone: teaching / demo Cloud Run portfolio template with Gemini chat.

## Read order (every session)

1. [docs/state.md](docs/state.md) - Teaching position and ownership.
2. [docs/index.md](docs/index.md) - inventory of docs that exist.
3. [FLAGS.md](FLAGS.md) - open improvement register.
4. [README.md](README.md) - beginner guide, deploy, cleanup.

Do not auto-load archive paths (none present).

## Operating mode

**Teaching.** Not a product launch. Prefer clarifying the workshop path over shipping product features.

Owner: GDG PUP Technology (incoming CTO). Handover 2026-09-02.

## Stack pins (from README / package)

- Node Express server (`server.js`), port `8080`
- Frontend under `public/`
- Deploy helper `deploy-cloudshell.sh` (sample defaults include service `bryl`)
- Gemini via `GEMINI_API_KEY` / Secret Manager

## Notes for agents

- Docs only changes belong in markdown; do not invent production requirements for this teaching template.
- Sample deploy names (`bryl`, `bryllim`) are teaching placeholders called out in FLAGS.

## FMD

**Built on FMD philosophy (v1.31.0)** - INDEX / STATE / FLAGS control plane for humans and AI; no FMD engine install.

Read order stays: docs/state.md then docs/index.md then FLAGS.md then task docs.
