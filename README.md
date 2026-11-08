# GitHub upload Worker

The Worker accepts public PNG uploads from the GitHub Pages site and commits them to the public `nabilafebriyani029/Malatcha-v1` repository:

- Photos: `uploads/frame-and-captures/`
- Uploaded frames: `uploads/frame-and-captures/` and `uploads/uploaded-frames/`

Uploaded images are public and may contain personal data. Anyone who can access the site can upload images. The Worker limits file sizes and has a best-effort per-IP rate limit; do not treat it as private storage or strong abuse prevention.

If the GitHub token expires, GitHub is rate-limited, or the Worker is unavailable, camera capture and local 4R download still work. Only the automatic repository upload and the shared-photo list are unavailable until the Worker is fixed.

## Deploy on the free Cloudflare Workers plan

1. Create a Cloudflare account and install Node.js/npm if they are not already installed.
2. In GitHub, create a **fine-grained personal access token** restricted to `nabilafebriyani029/Malatcha-v1`, with **Contents: Read and write** permission. Do not put this token in the HTML or commit it.
3. From this directory, run:

   ```powershell
   npx wrangler login
   npx wrangler secret put GITHUB_TOKEN
   npx wrangler deploy
   ```

   Paste the token only into the terminal prompt for `wrangler secret put`.
4. Copy the `workers.dev` URL shown after deployment. In `Malatcha Photo Corner.html`, set `UPLOAD_API_BASE` to that URL (without a trailing slash).
5. Publish the HTML and assets to the GitHub Pages site at `https://nabilafebriyani029.github.io/Malatcha-v1/`.

The Worker accepts browser requests only from the GitHub Pages origin configured in `ALLOWED_ORIGIN`. If the site uses a different domain, update that variable in `wrangler.toml` and redeploy.

The GitHub repository must have the `main` branch created before uploads can be committed. The first upload creates the `uploads/` folders automatically. Repository contents are public; visitors can browse uploaded files and use the saved-photo list on the site.
