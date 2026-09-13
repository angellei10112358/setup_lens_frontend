# setup-lens-frontend (pure static, single index.html)

Pure static frontend for `setup-lens-backend`. No build. Works on GitHub Pages + `file://`.

- File: `index.html` (single file, HTML+CSS+JS inline, no npm)
- Backend dropdown (Render / Self-host-by-lei / Self-host-by-lei-backup / Local, Local last); current choice saved to localStorage. Real URLs are NOT in code: `Render` / `Self-host-by-lei` / `Self-host-by-lei-backup` come from the GitHub Actions Secrets `RENDER_BACKEND_URL` / `SELF_HOST_BACKEND_URL` / `SELF_HOST_BACKEND_BACKUP_URL`, injected at deploy time (`.github/workflows/deploy.yml` replaces the `__*_BACKEND_URL__` placeholders in `index.html`); picking `Local` reveals a text box for any custom backend URL (defaults to `http://127.0.0.1:8000`)
- Keeps desktop UI: Lens EllPowerLaw 4 + Shear 2 + Source 7 + Redshift 2 + Beam Bmaj/Bmin/PA + Overlay (critical/caustics/colorbar off by default) + Zoom + Update/Reset + status
- Dual `<canvas>` 600×600: image (lensed PNG from backend, Blues, origin upper) + source (intrinsic PNG)
- Drag red star on source canvas → `source_centre_x/y` (world coords, clamped to orig extent), low-res `150@0.04` during move, high-res `300@0.02` on release
- Wheel zoom per-plane centered at cursor (0.75/1.33, clamp orig*2 / orig*0.05), buttons + reset
- Overlays drawn frontend from backend `critical_curves`/`caustics` world `(y,x)` polylines: critical white+cyan/lime dashed, caustic white+orange/red; star + Re dashed circle + crosshair; beam ellipse `angle=90+PA` bottom-left; colorbar Blues gradient optional

## Deploy (GitHub Pages, via Actions so the secret is injected)

1. Push this folder to the repo (`index.html` at root + `.nojekyll` + `.github/workflows/deploy.yml`)
2. Repo `Settings → Secrets and variables → Actions → New repository secret`
   - Name: `RENDER_BACKEND_URL`, Value: `https://<your-service>.onrender.com` (no trailing slash)
   - Name: `SELF_HOST_BACKEND_URL`, Value: `https://<self-hosted-domain>` (no trailing slash)
   - Name: `SELF_HOST_BACKEND_BACKUP_URL`, Value: `https://<backup-domain>` (no trailing slash)
3. Repo `Settings → Pages → Build and deployment → Source: GitHub Actions`
4. Push to `main` (or Run workflow manually); open the Pages URL, pick **Render** → Test

All paths relative, no absolute `/`. No secrets in JS (backend URL is public).

## Local preview

```bat
REM any static server
python -m http.server 5173 --directory adhoc_jobs\setup_lens_frontend
REM open http://127.0.0.1:5173/index.html, pick backend (Local) → Test → Update
```
