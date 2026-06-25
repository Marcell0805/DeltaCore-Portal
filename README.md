# DeltaCore Engineering Portal

Quick-start for viewing and updating the public documentation site.

## View locally

1. Open this folder in VS Code / Cursor.
2. Install **Live Server** extension (if prompted).
3. Run `portal/scripts/build-portal.ps1` if you changed JSON (or use committed `js/portal-data.js`).
4. Right-click `portal/index.html` → **Open with Live Server**.
5. Enter password from `data/portal-settings.json` (default: `deltacore`).

**Keyboard:** `Ctrl+K` to search.

## Public site

Built portal is deployed to the **public** repository (no product source code):

- Repo: [github.com/Marcell0805/DeltaCore-Portal](https://github.com/Marcell0805/DeltaCore-Portal)
- GitHub Pages URL (after setup): `https://marcell0805.github.io/DeltaCore-Portal/`

## Update content

1. Edit JSON files in `portal/data/`.
2. Run:
   ```powershell
   powershell -ExecutionPolicy Bypass -File portal/scripts/build-portal.ps1
   ```
3. Commit and push to **DeltaCore** (private). CI deploys only `portal/` to the public repo.

## Full reference

See [ENGINEERING_PORTAL.md](./ENGINEERING_PORTAL.md) for architecture, JSON schema, search, and deployment setup.
