# truck-queue-tracker

## Run the app locally

This project is a single-file frontend app.

### Option 1: Open directly in your browser
1. Open `/workspaces/truck-queue-tracker/index.html` in your browser.
2. Use the dashboard without any build step.

### Option 2: Run a local web server (recommended)
1. Open the terminal in this workspace.
2. Run:
   ```bash
   cd /workspaces/truck-queue-tracker
   python3 -m http.server 8000
   ``` 
3. Open this URL in your browser:
   ```
   http://localhost:8000/index.html
   ```

## How to use

1. Upload your PDF schedule in the top-left.
2. Click `Parse GE3A Schedule`.
3. Review the parsed schedule rows.
4. Click `Launch Day with Reviewed Schedule`.
5. Drag truck cards across schedule columns.
6. Click a truck card to edit details or change status.

## Notes

- The dashboard works locally without JSONBin.
- JSONBin is optional. Leave it blank if you don't have a bin ID or master key.
- The app saves state in your browser so you can refresh the page without losing data.

## Sharing with other users

If you want multiple people to use the same schedule and see updates from different phones or locations, use the `GitHub Gist` sync option in the app:

1. Create a GitHub account if you don't already have one.
2. Create a private gist at https://gist.github.com.
   - Click `+` and choose `New gist`.
   - Add any filename like `urea-ge3a-state.json`.
   - Paste `{}` as the content and save.
3. Create a GitHub Personal Access Token:
   - Go to https://github.com/settings/tokens.
   - Click `Generate new token`.
   - Select the `gist` scope.
   - Generate and copy the token.
4. In the app, choose `GitHub Gist` from the sync provider dropdown.
5. Paste your Gist ID and GitHub token into the fields.
6. Save the config.

This makes the app use your private gist as a shared database. All users using the same gist and token can see updates after a few seconds.

## GitHub Pages deployment

This repository now contains a GitHub Actions workflow that deploys `index.html` as a GitHub Pages site from the `main` branch.

### How to publish
1. Commit and push your changes to `main`.
2. GitHub Actions will automatically build and publish the site to the `gh-pages` branch.
3. Open your repository settings and enable Pages from the `gh-pages` branch if needed.
4. Your site URL will be:
   ```
   https://dispatch-controller.github.io/truck-queue-tracker/
   ```

### Notes
- The GitHub Pages site will host the app as a website URL.
- It still needs a shared sync backend (GitHub Gist or JSONBin) for multi-user live updates.
- If you want, I can also add a simple setup note to the app UI itself.

### If the Pages deployment fails with a 403 / write access error

GitHub Actions may be prevented from pushing the `gh-pages` branch in some repo settings. To fix this, create a Personal Access Token (PAT) and add it as a repository secret named `GH_PAGES_TOKEN`.

1. Create a PAT on GitHub:
   - Go to https://github.com/settings/tokens
   - Click **Generate new token** (classic)
   - Give it a name like `gh-pages-deploy`
   - Select the `repo` scope (or at minimum `repo:status` + `public_repo` / `repo` depending on private/public)
   - Generate and copy the token (you won't be able to see it again)

2. Add the token as a repo secret:
   - Go to your repository on GitHub → Settings → Secrets → Actions → New repository secret
   - Name: `GH_PAGES_TOKEN`
   - Value: *paste the PAT you copied*

3. Re-run the failed workflow or push a small change to `main` to trigger deployment again.

This repository's workflow now uses `GH_PAGES_TOKEN` and will fall back to the built-in `GITHUB_TOKEN` if the secret is not present.
