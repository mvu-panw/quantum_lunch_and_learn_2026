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

**Single shared stylesheet**: All pages link to `css/styles.css`. Brand tokens are defined in `:root` — use these variables for all styling, never hardcode colors:

| Token | Value | Use |
|---|---|---|
| `--orange` | `#FA582D` | Accents, active states, CTAs |
| `--navy` | `#0D1B2E` | Navbar, hero background, headings |
| `--grey` | `#F4F4F4` | Page background |
| `--text` | `#333333` | Body copy |
| `--muted` | `#888888` | Secondary labels |
| `--border` | `#E0E0E0` | Dividers |
| `--radius` | `8px` | Border radius |
| `--shadow` | `0 2px 12px rgba(0,0,0,.10)` | Card shadows |

**Two page layouts**:
- *Content layout* (`index.html`): standard scrolling page with `.content` / `.section` classes.
- *Embed layout* (`lab.html`, `slides.html`): `<body class="embed-page">` makes the page full-height with a sticky navbar, a thin `.embed-toolbar` strip, and a full-bleed `<iframe class="embed-frame">`.

**Agenda/menu component**: `index.html` uses a restaurant-menu style agenda built with `.menu-list` > `.menu-item` elements containing three spans: `.menu-name` (left), `.menu-dots` (flex-grow dotted line), `.menu-price` (right, styled in `--orange`). Use this pattern for any timed agenda.

**No templating**: The navbar HTML is copy-pasted into every `.html` file. When changing shared layout (navbar links, logo, etc.), update all HTML files. Mark the current page with `class="active"` on its nav link.

**Planned Google Sheets integration** (registration + login pages not yet built): Registration form submissions and login credential lookups will use a Google Apps Script Web App deployed with "Execute as: Me, Access: Anyone". Set the deploy URL as a JS constant `APPS_SCRIPT_URL` in the page. This is the same pattern used in `ctf_page` in the parent repo.

## Embedded Google Content

Google Docs and Google Slides use different embed URL patterns:

- **Google Doc** (`lab.html`): use `/preview` suffix — `https://docs.google.com/document/d/{ID}/preview`. Requires viewers to be signed in with access to the doc.
- **Google Slides** (`slides.html`): use `/embed` suffix with params — `https://docs.google.com/presentation/d/{ID}/embed?start=false&loop=false`.

Current embedded resources:
- `lab.html` — Google Doc `16Iw4L-gX5jNOeEvNaykjvZHUAIaqbk7GsyD5MLruzFM`
- `slides.html` — Google Slides `1Lo3YaBxOoE1F1tO_2cIVe5skudHiIhlPgLhKEdWYVmg`
