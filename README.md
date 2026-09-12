# setup-lens-frontend (pure static, single index.html)

Pure static frontend for `setup-lens-backend`. No build. Works on GitHub Pages + `file://`.

- File: `index.html` (single file, HTML+CSS+JS inline, no npm)
- Backend dropdown (Local / Render), addresses from `ENV` env-vars at top of `index.html` (`LOCAL_BACKEND_URL`, `RENDER_BACKEND_URL`); current choice saved to localStorage. Fill `RENDER_BACKEND_URL` with `https://<backend>.onrender.com` after deploy
- Keeps desktop UI: Lens EllPowerLaw 4 + Shear 2 + Source 7 + Redshift 2 + Beam Bmaj/Bmin/PA + Overlay (critical/caustics/colorbar off by default) + Zoom + Update/Reset + status
- Dual `<canvas>` 600×600: image (lensed PNG from backend, Blues, origin upper) + source (intrinsic PNG)
- Drag red star on source canvas → `source_centre_x/y` (world coords, clamped to orig extent), low-res `150@0.04` during move, high-res `300@0.02` on release
- Wheel zoom per-plane centered at cursor (0.75/1.33, clamp orig*2 / orig*0.05), buttons + reset
- Overlays drawn frontend from backend `critical_curves`/`caustics` world `(y,x)` polylines: critical white+cyan/lime dashed, caustic white+orange/red; star + Re dashed circle + crosshair; beam ellipse `angle=90+PA` bottom-left; colorbar Blues gradient optional

## Deploy (GitHub Pages)

1. New repo (or `gh-pages` branch) with this `index.html` at root + `.nojekyll`
2. `Settings → Pages → Deploy from branch → main / (root)`
3. Or copy `../BuildYourOwnGames` Vite pattern — not needed for single file

All paths relative, no absolute `/`. No secrets in JS (backend URL is public).

## Local preview

```bat
REM any static server
python -m http.server 5173 --directory adhoc_jobs\setup_lens_frontend
REM open http://127.0.0.1:5173/index.html, pick backend (Local) → Test → Update
```
