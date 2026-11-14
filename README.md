# Wi-Fry Studios

Wi-Fry Studios is hosted entirely on **GitHub Pages**. The public website, admin portal, content, task board and admin allowlist all live in this repository. There is no Cloudflare Worker, database, or external backend.

## Website

- Public: https://joelrobinson952-stack.github.io/wi-fry/
- Admin: https://joelrobinson952-stack.github.io/wi-fry/admin/
- Repository: https://github.com/joelrobinson952-stack/wi-fry

GitHub Pages is deployed by `.github/workflows/pages.yml`. In repository Settings → Pages, set the source to **GitHub Actions**.

## Admin access

The admin portal uses GitHub's API directly. Admins need a fine-grained Personal Access Token with access to this repository and **Contents: Read and write** permission.

1. On GitHub, open Settings → Developer settings → Personal access tokens → Fine-grained tokens.
2. Create a token restricted to this repository.
3. Grant **Contents: Read and write**. No other permissions are required.
4. Open the admin page and paste the token. It is held in browser session storage and is not committed to the repository.
5. Only usernames listed in `data/admins.json` are allowed into the dashboard.

The initial allowlist contains `joelrobinson952-stack`. Admins can add or remove usernames from the Admins tab. Anyone who has a token with write access to the repository can technically edit repository files directly, so only issue tokens to trusted people. Revoke a token in GitHub if it is lost.

## Features

- Trello-style task board: To do, In progress, Review and Done
- Create, assign, prioritise, move and delete tasks
- Publish, pin and delete announcements
- Edit homepage headline, tagline, about text and links
- Manage the approved admin username list
- Every change is committed to the GitHub repository and automatically published through GitHub Pages

## Data files

- `data/admins.json` — approved GitHub usernames
- `data/tasks.json` — studio task board
- `data/announcements.json` — public announcements
- `data/site.json` — editable homepage content

## Security note

This is a static GitHub Pages site, so there is no private server to hide credentials or enforce permissions. The admin token is never stored in the repository and is kept only in the current browser session. Use a fine-grained token limited to this repository, and do not share it. Repository collaborators with write access can bypass the UI and change files directly.
