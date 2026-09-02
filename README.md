# Complete Beginner's Guide: Personalizing Your AI Portfolio

## Status / Handover

Teaching and demo repository for a Cloud Run portfolio template with a Gemini chatbot. Not a product launch.

Owner: **GDG PUP Technology** (incoming CTO). Handover **2026-09-02**.

Docs: [docs/state.md](docs/state.md) · [docs/index.md](docs/index.md) · [FLAGS.md](FLAGS.md) · [AGENTS.md](AGENTS.md)

## Portfolio Template on Cloud Run

Plain HTML/CSS/JavaScript portfolio template served by Cloud Run, with private media assets stored in Cloud Storage and served through the Cloud Run domain, plus a Gemini 2.5 Flash-Lite chatbot endpoint.

**Runtime facts (match the code):**

- Entry server: `server.js` (Express on Node `>=20`; Docker image `node:24-alpine`)
- Local and Cloud Run port: `8080` (`PORT` env, default `8080`)
- Frontend files under `public/`: `index.html`, `styles.css`, `main.js`, `context.md`, `assets/`
- Deploy helper: `deploy-cloudshell.sh` (defaults: region `asia-southeast1`, service `bryl`, Artifact Registry `bryllim`, model `gemini-2.5-flash-lite`)

## Architecture

- **Cloud Run** serves the portfolio, proxies `/assets`, owns `/api/chat`, and exposes `/healthz` and `/config.js`.
- **Cloud Storage** stores media assets privately; Cloud CDN can be added later via an HTTPS load balancer backend bucket.
- **Secret Manager** stores `GEMINI_API_KEY`.
- **Cloud Build** builds and pushes the container image to Artifact Registry.
- **Gemini API** uses `gemini-2.5-flash-lite` (override with `GEMINI_MODEL`) against the Markdown knowledge base.
- **Markdown knowledge base** lives at `public/context.md`.

---

## What is in this project?

Think of your portfolio like a house:

- **`public/index.html`**: The structure (walls, doors, windows). This is where your text and links live.
- **`public/styles.css`**: The paint and furniture.
- **`public/main.js`**: Browser-side behavior (including chat UI calls to `/api/chat`).
- **`public/context.md`**: The brain of your house. This file tells your AI chatbot who you are and what you do.
- **`public/assets/`**: Your photo album (profile picture, project images, favicons).
- **`server.js`**: The Cloud Run / local Node server that serves pages, assets, and chat.

---

## Glossary of Terms for Beginners

If you're new to coding, here are some terms you'll see in this guide:

- **Repository (Repo)**: A folder where your code is saved and tracked, often hosted on the internet (like GitHub).
- **Clone**: Downloading a complete copy of a repository from the internet to your local computer.
- **Terminal / Command Line**: A text-based interface where you type commands to talk to your computer instead of clicking with a mouse.
- **Node.js**: The engine that allows JavaScript to run on your computer. This project needs **Node.js 20 or newer**.
- **API Key**: A secret password that lets your website talk to an external service (in this case, the Gemini AI).
- **Markdown (`.md`)**: A simple way to format text using symbols (like `#` for headings or `-` for lists).
- **HTML**: The skeleton or structure of a website (where your text and links live).
- **CSS**: The styling of a website (colors, spacing, and animations).
- **WebP**: A modern image format that makes images smaller in file size so they load faster.
- **Deployment**: Moving your site from your local computer to a server on the internet so anyone can visit it.
- **Cloud Run / Cloud Storage**: Google Cloud services used to host your website and store your images securely.

---

## Prerequisites: Getting the Code & Git Basics

### 1. Clone the Repository

Open a terminal and clone the repository to your local machine:

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
```

### 2. Basic Git Commands (Pushing Your Changes)

```bash
git add .
git commit -m "Update portfolio content and styles"
git push origin main
```

---

## Phase 1: Setting Up Your Workshop

### 1. Install Node.js

Install **Node.js 20+** (LTS is fine) from [nodejs.org](https://nodejs.org/).

### 2. Open Your Project

Open this folder in your editor and open a terminal.

### 3. Install Dependencies

```bash
npm install
```

### 4. Add Your API Key (The Chatbot's Power)

Go to [Google AI Studio](https://aistudio.google.com/) and get a free API key. Then set it in your shell:

- **Windows (PowerShell):** `$env:GEMINI_API_KEY="PASTE_YOUR_KEY_HERE"`
- **Mac/Linux:** `export GEMINI_API_KEY="PASTE_YOUR_KEY_HERE"`

### 5. Start the Preview

```bash
npm run dev
```

Open `http://localhost:8080`. Keep that terminal open. Without `GEMINI_API_KEY`, the portfolio still loads and `/api/chat` returns a setup message.

---

## Phase 2: Training Your AI Chatbot (`public/context.md`)

The chatbot answers questions based **only** on this file.

1. Open `public/context.md`.
2. **Profile:** Find `# Profile`. Change the sample name and describe your role in plain English.
3. **Knowledge base:** Add sections such as `## Experience` with dash lists.
4. **Tone:** Optional short instructions at the top of the file (funny, professional, direct).

> [!TIP]
> If someone asks something that is not in this file, the AI should say it does not know. If it matters, put it in the Markdown.

---

## Phase 3: Updating the Website (`public/index.html`)

### 1. Changing Your Name

Search for `<title>` and set it to `Your Name | Portfolio`.

### 2. Updating Sections

Look for sections like:

```html
<section id="about">
  <h2>About Me</h2>
  <p>I am a software engineer...</p>
</section>
```

Edit the text between the tags.

### 3. Adding Projects

Find the `id="projects"` section. Copy an existing project card block, paste below it, then change the title and link.

---

## Phase 4: Changing Colors & Style (`public/styles.css`)

1. Open `public/styles.css`.
2. Find the `:root` section near the top.
3. Useful variables in this template:

- `--accent`: main accent color (buttons, links)
- `--bg`: page background
- `--text` / `--muted` / `--faint`: text colors
- `--soft` / `--line`: soft surfaces and borders

Use a hex color picker for values like `#2563eb` or `#ef4444`.

---

## Phase 5: Swapping Images

1. Drop your photo into `public/assets/` (for example `profile.webp` or `profile.jpg`).
2. If you change the filename or extension, update the matching references in `public/index.html`.

Optional WebP conversion for PNGs under `public/assets` (skips favicons):

```bash
npm install
npm run assets:webp
```

---

## Phase 6: Going Live (GCP Deployment)

Recommended workflow: **Google Cloud Shell**.

### 1. Prepare the GCP project

1. Open [Google Cloud Console](https://console.cloud.google.com/).
2. Select or create a project and enable billing.
3. Open **Cloud Shell**.
4. Confirm or set the project:

```bash
gcloud config get-value project
gcloud config set project "your-project-id"
```

### 2. Get the source code into Cloud Shell

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
```

Or upload a ZIP, unzip it, and `cd` into the folder. You need `Dockerfile`, `server.js`, `package.json`, `public/`, and `deploy-cloudshell.sh`.

### 3. Edit the template content

Customize before deploying:

- `public/index.html`
- `public/context.md`
- `public/assets/`
- `public/styles.css`
- `public/main.js` (only if you change client behavior)

```bash
cloudshell edit public/context.md
```

Do not place secrets in repo files. Store the Gemini API key in Secret Manager or enter it at deploy time.

### 4. Deploy to Cloud Run

Interactive (script prompts for the Gemini key if unset):

```bash
chmod +x ./deploy-cloudshell.sh && ./deploy-cloudshell.sh --project "your-project-id"
```

Non-interactive:

```bash
export GEMINI_API_KEY="your-gemini-api-key"
chmod +x ./deploy-cloudshell.sh && ./deploy-cloudshell.sh --project "your-project-id"
```

With custom settings:

```bash
export GEMINI_API_KEY="your-gemini-api-key"
export GEMINI_MODEL="gemini-2.5-flash-lite"
export CHAT_RATE_LIMIT="10"
export GLOBAL_RATE_LIMIT="500"

chmod +x ./deploy-cloudshell.sh && ./deploy-cloudshell.sh \
  --project "your-project-id" \
  --region "asia-southeast1" \
  --service "bryl" \
  --bucket "your-project-id-bryllim-assets"
```

### 5. What gets created

The deploy script creates or reuses:

- Cloud Run service: `bryl` (default)
- Cloud Storage bucket: `<project-id>-bryllim-assets`
- Secret Manager secret: `gemini-api-key`
- Artifact Registry repository: `bryllim`
- APIs: Cloud Run, Cloud Build, Artifact Registry, Secret Manager, Cloud Storage

It also uploads `public/assets` to the private bucket, serves them through Cloud Run at `/assets`, sets long-lived cache headers, deploys with public access, and injects `GEMINI_API_KEY` from Secret Manager.

### 6. Verify the deployment

The script prints the service URL. Check health:

```bash
curl "$(gcloud run services describe bryl --region asia-southeast1 --format='value(status.url)')/healthz"
```

Expected:

```json
{"ok":true}
```

Open the service URL and verify the page, gallery, and chat.

---

## Cleaning Up (Avoiding Costs)

### Option 1: Delete the whole project

If this portfolio used a dedicated GCP project:

```bash
gcloud projects delete "your-project-id"
```

This permanently deletes everything inside the project.

### Option 2: Delete specific services

```bash
gcloud run services delete bryl --region asia-southeast1
gcloud storage rm --recursive gs://your-project-id-bryllim-assets
gcloud artifacts repositories delete bryllim --location asia-southeast1
gcloud secrets delete gemini-api-key
```

If you added Cloud CDN / Load Balancing manually, delete those resources in the Console as well.

---

## Updating The Site

After changing repo files:

```bash
git pull
./deploy-cloudshell.sh --project "your-project-id"
```

Skip media re-upload:

```bash
./deploy-cloudshell.sh --project "your-project-id" --skip-assets
```

The script is idempotent: it reuses existing GCP resources, uploads latest assets (unless skipped), creates a new Gemini secret version when `GEMINI_API_KEY` is provided, rebuilds the image, and deploys a new Cloud Run revision.

## Add Cloud CDN for assets

1. Keep Cloud Run serving HTML and `/api/chat`.
2. Keep `public/assets` in the private Cloud Storage bucket.
3. Create an HTTPS load balancer with a **backend bucket** pointing to that bucket.
4. Enable **Cloud CDN** on the backend bucket.
5. Set `ASSET_BASE_URL` to the CDN hostname (for example `https://cdn.example.com`).
6. Redeploy so `public/index.html` and `/config.js` use the CDN URL for asset links.

Notes:

- Keep long-lived `Cache-Control` headers on asset objects.
- Do not make the bucket public if the CDN should remain the only public path.
- Invalidate CDN cache for paths you change later.

## Configuration

Runtime environment variables:

- `GEMINI_API_KEY`: Gemini API key (Secret Manager via deploy script)
- `GEMINI_MODEL`: defaults to `gemini-2.5-flash-lite`
- `ASSET_BASE_URL`: defaults to `/assets`
- `ASSET_BUCKET_NAME`: private Cloud Storage bucket used by Cloud Run to serve media (`BUCKET_NAME` also accepted by `server.js`)
- `GLOBAL_RATE_LIMIT`: defaults to `500`
- `GLOBAL_RATE_LIMIT_WINDOW_MS`: defaults to `900000`
- `CHAT_RATE_LIMIT`: defaults to `10`
- `CHAT_RATE_LIMIT_WINDOW_MS`: defaults to `60000`
- `CHAT_MAX_MESSAGES`: defaults to `8`
- `CHAT_MAX_MESSAGE_LENGTH`: defaults to `800`
- `ALLOWED_CHAT_ORIGINS`: optional comma-separated browser origins for `/api/chat`; same-origin is allowed automatically
- `PORT`: supplied by Cloud Run; defaults to `8080` locally

## Security Notes

- Gemini API keys are stored in Secret Manager and injected into Cloud Run only at runtime.
- Chat uses `public/context.md` as strict context, plus Gemini safety settings.
- `/api/chat` uses same-origin checks, JSON body size limits, global rate limiting, and chat-specific rate limiting.
- Rate limiting is in Cloud Run instance memory. Stronger multi-instance protection needs Cloud Armor, reCAPTCHA/Turnstile, or a shared Redis-backed limiter.
- Cloud Storage is private; Cloud Run reads media and serves it from the app domain.
- For stricter production IAM, replace the default Cloud Run runtime service account with a dedicated account that can access only the Gemini secret.

## Contributors

This project is made possible by the GDG PUP community:

| Role | Name |
| --- | --- |
| 💻 **Development** | [James Gabriele Torzar](https://www.linkedin.com/in/4regab/) - Cloud Solutions |
| 💻 **Development** | [Kyla Marie A. Agapito](https://www.linkedin.com/in/kyla-marie-agapito/) - Cloud Solutions |
| 💻 **Development** | [Justin Royse L. Solomon](https://www.linkedin.com/in/justin-royse-solomon) - Cloud Solutions |
| 🚀 **CTO** | [Carlos Jerico Dela Torre](https://www.linkedin.com/in/delatorrecj/) - Chief Technology Officer (2025-2026) |

## Troubleshooting

### Browser Console: Content Security Policy blocks inline script

The app uses a strict CSP with per-request nonces. Redeploy so Cloud Run serves `index.html` through `server.js`. Opening `public/index.html` directly or serving it from a plain static host leaves the `__CSP_NONCE__` placeholder unresolved.

### Chat returns `500`

```bash
gcloud run services logs read bryl --region asia-southeast1 --limit 50
```

Common causes:

- Missing/invalid `GEMINI_API_KEY`, or no Gemini API access
- `public/context.md` missing from the deployed container
- Selected Gemini model unavailable for the key/project
- Gemini quota or rate limits exceeded

### Update chatbot knowledge

Edit `public/context.md`, then redeploy with `./deploy-cloudshell.sh --project "your-project-id"`.
