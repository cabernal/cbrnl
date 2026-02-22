# cbrnl

Minimal GitHub Pages hub for `cbrnl.com` with links to project web deployments.

## Files

- `index.html` + `styles.css`: static hub site
- `CNAME`: custom domain (`cbrnl.com`)
- `.github/workflows/deploy-pages.yml`: auto-deploy on push to `main`

## 1. Create and push this repo

```bash
cd /Users/bernal/git/cbrnl
git init
git add .
git commit -m "Add minimal cbrnl hub site"
git branch -M main
git remote add origin git@github.com:cabernal/cbrnl.git
git push -u origin main
```

## 2. Enable Pages in GitHub

In `cabernal/cbrnl`:

1. Open `Settings` -> `Pages`
2. Under `Build and deployment`, set `Source` to `GitHub Actions`
3. Wait for the workflow `Deploy GitHub Pages` to finish

## 3. DNS for apex domain (`cbrnl.com`)

At your DNS provider, set:

- `A` record: `@` -> `185.199.108.153`
- `A` record: `@` -> `185.199.109.153`
- `A` record: `@` -> `185.199.110.153`
- `A` record: `@` -> `185.199.111.153`
- `CNAME` record: `www` -> `cabernal.github.io`

Optional IPv6:

- `AAAA` record: `@` -> `2606:50c0:8000::153`
- `AAAA` record: `@` -> `2606:50c0:8001::153`
- `AAAA` record: `@` -> `2606:50c0:8002::153`
- `AAAA` record: `@` -> `2606:50c0:8003::153`

## 4. Project subdomains

Use one subdomain per project (simplest):

- `zequence.cbrnl.com` -> project repo (`cabernal/zequence`)
- `tilton.cbrnl.com` -> project repo (`cabernal/tilton`)

At DNS provider:

- `CNAME` `zequence` -> `cabernal.github.io`
- `CNAME` `tilton` -> `cabernal.github.io`

Then for each project repo:

1. Ensure repo deploys web static files to GitHub Pages
2. Add `CNAME` file in deployed output with the project domain
   - `zequence.cbrnl.com` (for `zequence`)
   - `tilton.cbrnl.com` (for `tilton`)
3. In each repo `Settings` -> `Pages`, set source to `GitHub Actions`

## Notes

- DNS can take a few minutes to propagate.
- GitHub may take a few minutes to issue TLS certificates for new custom domains.
