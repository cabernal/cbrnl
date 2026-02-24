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
- `voxel-editor.cbrnl.com` -> project repo (`cabernal/voxel-editor`)

At DNS provider:

- `CNAME` `zequence` -> `cabernal.github.io`
- `CNAME` `tilton` -> `cabernal.github.io`
- `CNAME` `voxel-editor` -> `cabernal.github.io`

Then for each project repo:

1. Ensure repo deploys web static files to GitHub Pages
2. Add `CNAME` file in deployed output with the project domain
   - `zequence.cbrnl.com` (for `zequence`)
   - `tilton.cbrnl.com` (for `tilton`)
   - `voxel-editor.cbrnl.com` (for `voxel-editor`)
3. In each repo `Settings` -> `Pages`, set source to `GitHub Actions`

## 5. Add a new project to cbrnl

Use this flow for each new web project.

1. Pick a project slug and subdomain.
   - Example slug: `mygame`
   - Target URL: `mygame.cbrnl.com`
2. Make sure the project repo is public.
3. Add DNS:
   - `CNAME` `mygame` -> `cabernal.github.io`
4. In the project repo, add/update a Pages workflow that:
   - builds web output
   - writes `CNAME` with `mygame.cbrnl.com`
   - uploads the web output to Pages
5. Enable Pages with GitHub Actions and set the custom domain.
6. Add the new link in this hub (`index.html`) and deploy the hub.

CLI template:

```bash
# 1) repo visibility (required for Pages on your plan)
gh repo edit cabernal/mygame --visibility public --accept-visibility-change-consequences

# 2) initialize Pages with GitHub Actions source
gh api -X POST /repos/cabernal/mygame/pages -f build_type=workflow

# 3) set custom domain + HTTPS
gh api -X PUT /repos/cabernal/mygame/pages \
  -f build_type=workflow \
  -f cname=mygame.cbrnl.com \
  -F https_enforced=true
```

Hub update checklist:

1. Add project entry under `Projects` in `index.html`.
2. Add repo link under `Source` in `index.html`.
3. Commit and push `cbrnl`:

```bash
cd /Users/bernal/git/cbrnl
git add index.html
git commit -m "Add mygame to hub"
git push
```

## Notes

- DNS can take a few minutes to propagate.
- GitHub may take a few minutes to issue TLS certificates for new custom domains.
