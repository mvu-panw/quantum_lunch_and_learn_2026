# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Customer-facing static web portal for a Palo Alto Networks "Quantum Lunch and Learn" event. Pages built and deployed: Home (`index.html`), Hands-on Lab (`lab.html`), Slides (`slides.html`). Pages planned but not yet built: Registration, Post-Registration, Login.

## Running Locally

```bash
python3 -m http.server 8080   # open http://localhost:8080
```

No build step or package manager. Changes are visible immediately on refresh.

## Deployment

Push to `main` → GitLab CI (`.gitlab-ci.yml`) automatically deploys to GitLab Pages. The CI job copies repo contents into `public/` and uploads as a Pages artifact.

## Architecture

**Single shared stylesheet**: All pages link to `css/styles.css`. Brand tokens are defined in `:root` — `--orange`, `--navy`, `--grey`, `--white`, `--text`, `--muted`, `--border`. Use these variables for any new styling; never hardcode colors.

**Two page layouts**:
- *Content layout* (`index.html`): standard scrolling page with `.content` / `.section` classes.
- *Embed layout* (`lab.html`, `slides.html`): `<body class="embed-page">` makes the page full-height with a sticky navbar, a thin `.embed-toolbar` strip, and a full-bleed `<iframe class="embed-frame">`.

**No templating**: The navbar HTML is copy-pasted into every `.html` file. When changing shared layout (navbar links, logo, etc.), update all HTML files. Mark the current page with `class="active"` on its nav link.

**Planned Google Sheets integration** (registration + login pages not yet built): Registration form submissions and login credential lookups will use a Google Apps Script Web App deployed with "Execute as: Me, Access: Anyone". Set the deploy URL as a JS constant `APPS_SCRIPT_URL` in the page. This is the same pattern used in `ctf_page` in the parent repo.

## Embedded Google Content

- `lab.html` — embeds Google Doc `16Iw4L-gX5jNOeEvNaykjvZHUAIaqbk7GsyD5MLruzFM` via `/preview` (requires viewer to be signed in and shared on the doc)
- `slides.html` — embeds Google Slides `1Lo3YaBxOoE1F1tO_2cIVe5skudHiIhlPgLhKEdWYVmg` via `/embed`
