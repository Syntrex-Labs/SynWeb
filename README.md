SyntrexWeb — Local font usage and Loader

What I changed:
- Removed Google Fonts import and added a local custom font `KewlFont` via `@font-face` in `index.html`.
- Updated font-family usages to prefer `KewlFont` and provide fallbacks.
- Created `fonts/` folder with `kewlfont.ttf` placeholder and `fonts/README.md` explaining how to replace it.
- Added a full-screen site loader overlay that shows until the page finishes loading and then fades out.
- Replaced the Google Fonts-based type with a local KewlFont, and added a tooltip box used as the 'Get Notified' element.
- Added `assets/syntrexlabs.png` placeholder and a corner logo; added favicon and apple-touch-icon links.

How to use your real font:
1. Place your real `kewlfont.ttf` file into `fonts/kewlfont.ttf`.
2. Optionally add `kewlfont.woff2` for better performance.
3. Open `index.html` in a web browser to verify the font renders.

Loader:
- Loader markup is in `index.html` as `#siteLoader` (the loader block uses your provided animated letters and spinner).
- To disable/remove the loader, remove or comment out the `<div id="siteLoader" ...>` block at the top of `<body>` in `index.html`.
- To tweak fade duration or delay, edit `.loader-overlay` CSS and the JS delay in the `window.load` handler.

Horizon / blue line:
- The glowing blue line is implemented with `.horizon::before` and is now positioned flush to the bottom (bottom: 0) so that the glow sits at the base of the page.
- If you want to move it up above the bottom, edit the `bottom` value in the `.horizon::before` block (e.g., bottom: 40px) and adjust the `height` / `box-shadow` spreads to retain the glow.

Tooltip super-button:
- The “Get Notified” button is replaced with a tooltip-styled button that is centered and hover-only. To enable click/submit behavior, change the wrapper to `<form>` and the element to a `<button type="submit">`.

Badge spacing:
- The `Syntrex ✦ Coming Soon` badge spacing has been reduced so it sits closer to the header. You can tweak it by editing `.hero-badge { margin-bottom: 1rem; }` in `index.html` if you'd like it even tighter or looser. On smaller screens the spacing is reduced to 0.75rem so the layout remains compact.

Site Photo / Logo:
- I added a site photo `<img src="assets/syntrexlabs.png" class="site-photo">` above the badge in the hero. Replace `assets/syntrexlabs.png` with your real `syntrexlabs.png` image file.
- The CSS sets a default centered size (`width: 160px`, `max-width: 40vw`), and it uses `object-fit: contain` to keep it sharp and not cropped. Adjust `.site-photo` values in the top `style` block to tune size, rounding, or shadows.

Corner site icon & favicon:
- I added a floating corner icon that uses `assets/syntrexlabs.png` and links to `/` (the homepage). The element markup is:

    <a class="corner-logo" href="/" aria-label="Go to Syntrex home">
        <img src="assets/syntrexlabs.png" alt="Syntrex" />
    </a>

- I also added a `rel="icon"` and `rel="apple-touch-icon"` using `assets/syntrexlabs.png`. For best compatibility you can add multiple favicon sizes (`favicon-16x16.png`, `favicon-32x32.png`, `favicon-64x64.png`) and a `.ico` file.
- The CSS uses a `48px` default size for the corner logo and reduces to `40px` on smaller screens. Tweak `.corner-logo` in `index.html` if you want a different size or offset.

Troubleshooting assets & fonts on GitHub Pages:
- Common issues when deploying static assets (images, fonts) to GitHub Pages are:
  1) Incorrect resource paths: avoid absolute paths that begin with `/` (unless you host at root). Instead use relative paths like `assets/syntrexlabs.png` or `./assets/syntrexlabs.png`.
  2) Case sensitivity: macOS/Windows uses case-insensitive filenames locally, but GitHub Pages runs on Linux and is case-sensitive. Ensure filename case matches the path exactly (e.g., `kewlfont.ttf` vs `KewlFont.ttf`).
  3) Files not committed/pushed: verify the `assets/`, `fonts/` directories and their content are committed and present in the GitHub repo.
  4) Base path / repo name: GitHub Pages for repositories serves under `https://<user>.github.io/<repo-name>/` — absolute paths starting with `/` will point to the domain root. To fix either set a proper `<base href="/repo-name/">` or use relative paths. We added a client-side redirect script, but for assets prefer relative paths.
  5) Browser caching: browsers may cache icons and fonts. Force a refresh (Ctrl+Shift+R) or clear cache to see updates.
  6) Check the Network tab & console for requests returning 404 or CORS errors to identify wrong URLs.

Testing & quick checks:
- I added an in-page asset checker (runs when the page loads) that will display a warning if `assets/syntrexlabs.png`, `fonts/kewlfont.ttf`, or `fonts/kewlfont.woff2` are unreachable.
- To test locally (and reproduce a similar environment):

```powershell
cd c:\Users\user\Desktop\SyntrexWeb
python -m http.server 8000
Start-Process "http://localhost:8000"
```
- Then open DevTools (F12) → Network to see resource requests and confirm 200 OK statuses.
- If a resource returns 404, review the path being requested and ensure the file exists and has matching case.

Tips to make deployment robust:
- Commit `assets/` and `fonts/` to the branch your GitHub Pages site uses (e.g., `main` or `gh-pages`).
- Add a `.nojekyll` file to your repo root to avoid Jekyll processing (useful when file or folder names are unusual).
- Use relative asset paths in markup (no leading `/`) or set `<base href='/repo-name/'>` if you want absolute paths under a repo path.

Server-side redirects (recommended):
- For true canonicalization and SEO-friendly redirects, configure your webserver or hosting platform to redirect `/index.html` to `/` or `/home`. GitHub Pages supports `_redirects` on some platforms or requires setting up redirects via the hosting service; client-side redirects are a fallback.

CDN fallback option:
- This project includes a small JS fallback that will attempt to fetch missing assets from the jsDelivr CDN if you set the GitHub user/repo meta tags in `index.html` like:

  <meta name="github-user" content="your-username">
  <meta name="github-repo" content="your-repo">

- jsDelivr URL format used: `https://cdn.jsdelivr.net/gh/<user>/<repo>@latest/<path>`
- When the asset checker finds a failing asset it will try to fetch the file using the CDN. If the fetch succeeds, it will update the page to use the CDN-hosted file.
- This is a convenience for preview or fallback, but it's best to ensure your repo structure and paths are correct and committed to the branch used by GitHub Pages.
