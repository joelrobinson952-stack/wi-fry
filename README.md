# Wi-Fry Studios

Wi-Fry Studios public website and admin portal.

## Pages (hosted on GitHub Pages)

- Public homepage: `index.html`
- Admin portal: `admin/index.html`
- GitHub Actions publishing workflow: `.github/workflows/pages.yml`

The website is published at `https://joelrobinson952-stack.github.io/wi-fry/`. Enable **Settings → Pages → Build and deployment → GitHub Actions** in the repository if you have not already.

## Admin portal

The admin page is also hosted on GitHub Pages at `https://joelrobinson952-stack.github.io/wi-fry/admin/`. Admins authenticate with **Sign in with GitHub**. The Cloudflare Worker is only the private API/authentication backend; it redirects the browser back to the GitHub-hosted admin page after OAuth.

Features include:
- GitHub OAuth admin sign-in, restricted to usernames explicitly added by an existing admin
- Four-column Trello-style task board (To do, In progress, Review, Done)
- Create, assign, prioritize, move and delete tasks
- Publish, pin and delete announcements
- Edit public homepage copy and Roblox/Discord links
- Add and remove GitHub admin accounts

## One-time GitHub OAuth setup (required)

1. While signed into the GitHub account that owns the studio, open [GitHub Developer Settings → OAuth Apps](https://github.com/settings/developers).
2. Choose **New OAuth App**.
3. Set **Application name** to `Wi-Fry Studios`.
4. Set **Homepage URL** to `https://joelrobinson952-stack.github.io/wi-fry/`.
5. Set **Authorization callback URL** to `https://wi-fry-studios.joel-robinson-952.workers.dev/auth/callback`.
6. Register the app. Copy its **Client ID**.
7. In Cloudflare, open **Workers & Pages → wi-fry-studios → Settings → Variables and Secrets**. Add `GITHUB_CLIENT_ID` as a text variable and `GITHUB_CLIENT_SECRET` as a secret (generate a client secret in the GitHub OAuth App page).
8. Also make sure the existing `BOOTSTRAP_KEY` secret is set on the Worker. Never commit either secret to GitHub.
9. Redeploy the Worker after adding variables/secrets if Cloudflare does not activate them automatically.

## First admin setup

1. Visit `https://joelrobinson952-stack.github.io/wi-fry/admin/`.
2. On first setup, enter the one-time `BOOTSTRAP_KEY` and your GitHub username.
3. Then select **Continue with GitHub** and authorize the OAuth app.
4. Once signed in, add other admins by their exact GitHub usernames under **Admin accounts**.

The first-admin setup endpoint locks itself after the first GitHub admin is registered. Do not share the bootstrap key.

## Cloudflare backend

- Worker: `wi-fry-studios`
- Worker URL: `https://wi-fry-studios.joel-robinson-952.workers.dev`
- D1 database: `wi-fry-portal`
- D1 binding: `DB`

The D1 database has a `github_username` column and unique index for admin allowlisting. Public announcements and editable homepage settings are served by the Worker API. Admin API requests require a session issued after GitHub OAuth.

## Files

- `index.html` — public website
- `admin/index.html` — GitHub-hosted admin portal
- `worker.js` — Cloudflare API and GitHub OAuth callback
- `schema.sql` — D1 schema
- `wrangler.toml` — Worker/D1 configuration
