# mattsapps

Source for [mattsapps.xyz](https://mattsapps.xyz): a static, dependency-free
site (plain HTML, CSS and a little inline JavaScript, no build step).

## What's here

| Page | What it is |
| --- | --- |
| `index.html` | The hub: links to the apps, the shop, ATTACTIX and FattyMatty 40K, plus the All-Access membership |
| `apps.html` | Launcher for the web apps (each app lives on its own subdomain, see below) |
| `shop.html` + `shop/products/*.html` | Digital-product shop: AI prompt packs, budget trackers, planners, resume templates and ATTACTIX games. Each card links to a product page; checkout is Lemon Squeezy or Gumroad depending on the product. Images live in `shop/img/` |
| `free-prompts.html` | Free AI prompt samplers (email sign-up through Lemon Squeezy) |
| `attactix.html`, `attactix/` | ATTACTIX card game landing page and the full rules page (`/attactix/`) |
| `fattymatty40k.html`, `daemons-tracker.html` | FattyMatty 40K channel page and the Chaos Daemons tournament list tracker |
| `privacy.html`, `terms.html` | Privacy Policy and Terms of Service (linked from every page footer) |
| `robots.txt`, `sitemap.xml` | Crawler rules and the sitemap of public pages; add new public pages to `sitemap.xml` |
| `favicon.svg` | Site favicon |
| `google*.html`, `pinterest-*.html` | Search Console / Pinterest site-verification files; leave as is |

Pages use Cloudflare Web Analytics (the beacon snippet sits just before
`</body>`); copy it from `shop.html` when adding a page.

## Deploying

`.github/workflows/deploy.yml` publishes the repo root to GitHub Pages on every
push to `main`, and `CNAME` points it at `mattsapps.xyz`. A push to `main` goes
live, so make changes on a branch and merge them through a pull request.

## Apps on subdomains

Each app lives in its own repo and gets its own subdomain
(`app-name.mattsapps.xyz`) rather than a subpath of this site. That gives every
app its own origin, which matters once an app is wrapped as an installable
PWA or a native app (Capacitor/TWA/etc.): separate origins mean separate
service worker scopes and separate local storage, avoiding collisions between
apps.

### Adding a new app

1. Create a new repo for the app (its own stack, its own build).
2. Give it a `CNAME` file (or `public/CNAME` if it has a build step that
   copies a `public/` dir into the build output) containing
   `app-name.mattsapps.xyz`.
3. Add a GitHub Actions workflow that builds it and deploys via
   `actions/upload-pages-artifact` + `actions/deploy-pages`. Copy
   `.github/workflows/deploy.yml` from the `dinner-bell` repo (or this repo,
   for a no-build static site) as a template.
4. In the new repo's GitHub Settings → Pages: set Source to "GitHub Actions",
   then set the custom domain to `app-name.mattsapps.xyz`.
5. Add a DNS record: `app-name` → `CNAME` → `fattymatty007.github.io`.
6. Add an entry to the `APPS` list in this repo's `apps.html` and open a pull
   request; the site redeploys when it is merged.
