# Peris & David wedding site — deployment

This folder is the static GitHub Pages frontend. It expects the Google Apps Script project to be deployed separately as a JSON API.

## 1. Configure the frontend

Open `index.html` and replace:

1. `PASTE_YOUR_APPS_SCRIPT_EXEC_URL_HERE` with the deployed Apps Script URL ending in `/exec`.
2. Every `https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO/` placeholder with the final GitHub Pages URL.

Keep `hero.jpg`, `favicon.svg`, and `index.html` in the same repository folder.

## 2. Deploy the Apps Script API

1. In Apps Script, keep the action router in `doGet(e)` and `doPost(e)`.
2. Confirm that every route returns JSON through `ContentService`.
3. Run `setupSheet()` once if the spreadsheet has not been initialized.
4. Choose **Deploy → New deployment → Web app**.
5. Set **Execute as: Me**.
6. Set **Who has access: Anyone**.
7. Deploy, authorize, and copy the URL ending in `/exec`.
8. Paste that URL into `API_BASE_URL` in `index.html`.

When backend code changes later, use **Deploy → Manage deployments → Edit → New version** so the existing `/exec` URL remains stable.

## 3. Deploy GitHub Pages

1. Create a GitHub repository.
2. Upload `index.html`, `hero.jpg`, and `favicon.svg` to the repository root.
3. In the repository, open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select the `main` branch and `/ (root)` folder, then save.
6. GitHub will publish a URL like `https://username.github.io/repository/`.
7. Put that exact URL into `og:url`, `canonical`, and the full `og:image` / `twitter:image` URL in `index.html`.

## 4. WhatsApp preview check

WhatsApp caches previews. After publishing updated Open Graph tags, test with a fresh URL query such as `?v=2`, or wait for the cached preview to refresh. The image URL must be public and absolute.

## API actions used by the frontend

Reads via GET:

- `getGiftRegistry`
- `getTillNumber`
- `getRecentActivity&limit=8`
- `checkExistingGuest&phone=...`

Writes via POST with `Content-Type: text/plain;charset=utf-8` and a JSON-string body:

- `submitRSVPOnly`
- `submitContribution`

The frontend accepts either raw JSON results or wrapped responses such as `{ "ok": true, "data": ... }`.
