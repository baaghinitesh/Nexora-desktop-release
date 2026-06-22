# Nexora Release & Deployment Guide

This guide explains how to compile, publish, and release new versions of the Nexora Desktop App to GitHub. Following these steps ensures that existing installed versions of the app will seamlessly auto-update to the new release.

---

## How Auto-Update Works in Nexora

Nexora uses `electron-updater` working in tandem with `electron-builder` to manage updates:
1. When you run the publish script, `electron-builder` compiles the executable and creates a tiny metadata file called `latest.yml`.
2. Both the compiled installer (`Nexora-Setup-vX.Y.Z.exe`) and `latest.yml` are uploaded to the GitHub Release.
3. Currently installed client applications check `latest.yml` in the background. If a newer version is listed, the app automatically downloads the installer and prompts the user to restart and install.

---

## Step-by-Step Release Process

### 1. Prerequisites (One-Time Setup)

You need a GitHub Personal Access Token (PAT) with write access to the release repository `baaghinitesh/Nexora-desktop-release`:

1. Go to **GitHub Settings** → **Developer Settings** → **Personal Access Tokens** → **Tokens (classic)**.
2. Click **Generate new token (classic)**.
3. Select the `repo` scope (this grants permissions to upload release assets).
4. Generate the token and copy it.

### 2. Configure Environment Variables

For the build scripts to upload files to GitHub, the token must be set in your terminal environment.

#### Windows (Command Prompt)
```cmd
# Set token for the current session
set GH_TOKEN=your_github_token_here

# Or set it permanently as a system environment variable
setx GH_TOKEN "your_github_token_here"
```

#### Windows (PowerShell)
```powershell
# Set token for the current session
$env:GH_TOKEN="your_github_token_here"
```

### 3. Prepare the Codebase

1. Open your terminal in the `desktop` project folder:
   ```bash
   cd desktop
   ```
2. Open `package.json` and increment the `"version"` field according to semantic versioning rules:
   ```json
   {
     "name": "nexora-desktop",
     "version": "1.0.3",
     ...
   }
   ```
   *Note: If the current version is `1.0.2`, changing it to `1.0.3` will trigger an update check pass on all installed `1.0.2` clients.*

### 4. Build and Publish

Execute the publish command from the `desktop` directory:
```bash
npm run release
```
This script runs several steps:
- Cleans previous build artifacts.
- Runs the CSS prebuilder to compile `nexora-styles.min.css`.
- Packages the application for Windows.
- Creates the `latest.yml` metadata descriptor.
- Automatically drafts a release on GitHub, uploads the compiled `.exe` and `latest.yml`, and publishes it.

### 5. Verify the Release on GitHub

1. Go to the [Releases page of baaghinitesh/Nexora-desktop-release](https://github.com/baaghinitesh/Nexora-desktop-release/releases).
2. Check the newly created release tag (e.g. `v1.0.3`).
3. Ensure the following assets are present:
   - `Nexora-Setup-1.0.3.exe` (The installer)
   - `latest.yml` (The auto-update descriptor)
   - `Nexora-Setup-1.0.3.exe.blockmap` (Speeds up block-based downloads)
4. Add release notes detailing new features, improvements, and fixes.

---

## Troubleshooting

### Error: "GitHub token not found"
- Verify that `GH_TOKEN` is loaded in the current terminal environment by typing `echo %GH_TOKEN%` (CMD) or `echo $env:GH_TOKEN` (PowerShell).
- If using `setx`, restart your code editor or terminal to reload the environment variables.

### Error: "Repository not found"
- Ensure that you have write access to the repository `baaghinitesh/Nexora-desktop-release`.
- Verify the `owner` and `repo` match under the `"publish"` block in `package.json`:
  ```json
  "publish": [
    {
      "provider": "github",
      "owner": "baaghinitesh",
      "repo": "Nexora-desktop-release",
      ...
    }
  ]
  ```

---

## Best Practices for Upgrades
- Always test the installer locally before publishing: `npm run build:win`. Check that the generated executable in `dist/` installs and launches correctly.
- Increment the patch version for bug fixes (e.g. `1.0.2` → `1.0.3`).
- Increment the minor version for new backward-compatible features (e.g. `1.0.2` → `1.1.0`).
- Increment the major version for breaking structural changes.
