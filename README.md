# Wi-Fry Studios

A full-stack website and studio admin portal for the Wi-Fry Roblox development team.

## Features

- Public landing page with About Us, Roblox games and Discord links
- Latest public announcements, including pinned announcements
- Admin login with hashed passwords and secure, HTTP-only session cookies
- Four-column Trello-style task board: To do, In progress, Review and Done
- Create, move and delete tasks, with priority and assignee
- Publish, pin and delete announcements
- Edit the public website's headline, tagline, About Us text and community links
- Add and remove administrator accounts
- Cloudflare D1 database persistence

## GitHub Pages website

The public website is in `index.html`. A GitHub Actions workflow at `.github/workflows/pages.yml` publishes it automatically whenever you push to `main`.

To turn on GitHub Pages for this repository:

1. Open the repository on GitHub and go to **Settings → Pages**.
2. Under **Build and deployment**, choose **GitHub Actions** as the source.
3. Open the **Actions** tab and allow workflows if GitHub asks.
4. Run the **Deploy Wi-Fry website to GitHub Pages** workflow, or push another commit to `main`.
5. GitHub will show the published URL in **Settings → Pages**. It should be `https://joelrobinson952-stack.github.io/wi-fry/`.

The static homepage reads public announcements from the Cloudflare Worker. The Worker allows read-only public API access from this GitHub Pages origin. Admin login remains on the Worker domain so secure session cookies stay same-origin.

## Cloudflare deployment

The Worker and D1 database have been created in the connected Cloudflare account.

1. Open **Cloudflare Dashboard → Workers & Pages → wi-fry-studios → Settings → Variables and Secrets**.
2. Add a **Secret** named `BOOTSTRAP_KEY` with the one-time setup value supplied to the site owner in this chat.
3. Confirm the Worker has the D1 binding `DB` pointing to database `wi-fry-portal` (already configured by the deployment).
4. Open the Worker URL: `https://wi-fry-studios.joel-robinson-952.workers.dev`.
5. The first visit will show the initial-admin setup screen. Enter the same setup key, your admin email and a strong password (12+ characters).
6. Once the first admin exists, the setup endpoint automatically locks. Sign in using your email and password.
7. Add other administrators from the Admin accounts tab. Give each person their own password.

Never commit the bootstrap key or passwords to GitHub. The public website and admin panel share the same Worker origin, so session cookies are HTTP-only and Secure.

## Local files

- `worker.js` — website, API, authentication and admin portal
- `schema.sql` — D1 database schema
- `wrangler.toml` — Worker and D1 binding configuration

## Notes

This is the first working foundation. It intentionally keeps public content and admin-only tools on the same origin. The task board currently supports statuses, priorities and assignees; richer project spaces, comments, file uploads and audit history can be added as the studio grows.
