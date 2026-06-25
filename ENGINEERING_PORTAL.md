# DeltaCore Engineering Portal — Maintainer Reference

This document describes the Engineering Portal: a **read-only, shareable documentation site** for vision, architecture, standards and roadmap. It does **not** contain product source code.

## Purpose

| Component | Role |
|-----------|------|
| **DeltaCore** (private repo) | Product source: API, MVC, database, scripts |
| **DeltaCore-Portal** (public repo) | Static documentation only — safe to share |
| **DeltaCore.MVC** | Operational admin UI (equipment, alerts) |
| **This portal** | Engineering knowledge base for investors, partners and team |

## Repository split (Option 1)

```
DeltaCore (PRIVATE)                    DeltaCore-Portal (PUBLIC)
├── src/                               ├── index.html
├── scripts/postgresql/                ├── sections/
├── portal/  ← edit here               ├── js/portal-data.js
└── .github/workflows/                 └── (no .cs, no SQL)
         deploy portal/ only ──────────►
```

### One-time setup

1. Create public GitHub repo: `Marcell0805/DeltaCore-Portal` (empty, no README).
2. Create a **Personal Access Token** (classic) with `repo` scope.
3. In **DeltaCore** private repo → Settings → Secrets → Actions:
   - Name: `PORTAL_DEPLOY_TOKEN`
   - Value: your PAT
4. Enable GitHub Pages on **DeltaCore-Portal**: Settings → Pages → Source: **Deploy from branch** → branch `main` → folder `/ (root)`.
5. Push a change under `portal/` to DeltaCore — workflow deploys automatically.

## Folder structure

```
portal/
├── index.html                 # Landing + search
├── data/                      # Source content (edit these)
│   ├── portal-settings.json   # Branding, password, URLs
│   ├── nav.json               # Sidebar navigation
│   └── *.json                 # One file per section
├── sections/                  # Generated HTML shells (do not edit by hand)
├── js/
│   ├── portal-data.js         # Generated
│   ├── search-index.js        # Generated
│   ├── portal.js              # Nav, rendering
│   ├── search.js              # Ctrl+K search
│   └── auth.js                # Password gate
├── css/portal.css
├── scripts/build-portal.ps1
├── README.md
└── ENGINEERING_PORTAL.md      # This file
```

## JSON content schema

Each section file (except `portal-settings.json` and `nav.json`):

```json
{
  "id": "integrations",
  "title": "Integrations and Partnership Strategy",
  "status": "planned",
  "tags": ["petroman", "ava"],
  "searchKeywords": ["J1939"],
  "summary": "One-line description for search snippets.",
  "sidebarNote": "Optional sidebar callout.",
  "blocks": [
    {
      "id": "petroman-phases",
      "heading": "PetroMan integration phases",
      "content": "Paragraph text.",
      "bullets": ["Phase 1 read-only API", "Phase 2 shadow-mode"]
    }
  ]
}
```

**Status values:** `live` | `in_progress` | `planned`

## Search

- Build script flattens all JSON into `search-index.js`.
- Browser uses **Fuse.js** for fuzzy client-side search.
- **Ctrl+K** opens search modal on any page.
- No server required — works on GitHub Pages.

## Build command

```powershell
powershell -ExecutionPolicy Bypass -File portal/scripts/build-portal.ps1
```

Regenerates `portal-data.js`, `search-index.js`, and all `sections/*.html`.

## v1 scope

**Included:** Read-only portal, password gate, search, full blueprint content, GitHub Pages deploy to public repo.

**Deferred (Phase 2):**
- In-browser pencil editing
- ASP.NET hosted portal with real auth
- CMS (Decap, etc.)

## Definition of done (documentation)

When changing product architecture or MVP scope, update the relevant `portal/data/*.json`, rebuild, and push so the public portal stays accurate.

## Security notes

- Do **not** put connection strings, API keys or internal URLs in portal JSON.
- Password gate is for casual visitors only — not cryptographic security.
- Public repo must never receive files from `src/`, `scripts/postgresql/` with sensitive patterns, or `.env` files.

## Related documents

- DeltaCore Technical Blueprint v0.3 (authoritative product spec)
- DeltaCore Engineering Portal Vision v1 (UX and section list)
- Huntress Cookbook (UI pattern reference)
