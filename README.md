# Anvil Setup

A simulated installer UI (welcome → options → installing → complete), packaged as an Electron desktop app so it can be built into a real Windows `.exe`.

## What's in this folder

```
anvil-setup-app/
├── index.html              the UI (all screens, styles, animation logic)
├── main.js                 Electron main process (creates the window)
├── preload.js              exposes safe minimize/maximize/close to the UI
├── package.json            app metadata + electron-builder config
├── assets/icon.ico         app icon
└── .github/workflows/
    └── build.yml           GitHub Actions workflow that builds the .exe
```

## Option A — Build the .exe automatically on GitHub (no local setup needed)

1. Create a new repository on GitHub (public or private — either works).
2. Upload **all the files in this folder**, keeping the folder structure exactly as-is
   (the `.github/workflows/build.yml` path matters — GitHub only picks up workflows
   that live at exactly that path).
   - Easiest way: on the repo page, click **Add file → Upload files**, drag the whole
     unzipped folder in, and commit.
3. Go to the **Actions** tab of your repository.
   - If it's your first workflow, click **"I understand my workflows, go ahead and
     enable them"**.
   - You should see a workflow run start automatically (triggered by the push).
     If it didn't, open the **Build Windows EXE** workflow and click **Run workflow**.
4. Wait for the run to finish (a few minutes — it installs Node, npm dependencies,
   and runs `electron-builder` on a Windows runner).
5. Open the finished run, scroll to **Artifacts**, and download **anvil-setup-windows**.
   Unzip it — inside you'll find `Anvil Setup Setup 3.1.0.exe` (the NSIS installer)
   and a portable `.exe` that runs without installing anything.

No local Node.js or Electron installation is required for this option — GitHub's
runner does all the building.

## Option B — Build locally, then just push the result

If you'd rather build on your own machine:

```bash
npm install
npm run dist
```

This requires Node.js 18+ installed locally. The output `.exe` files land in `dist/`.

## Notes

- This is a **simulated** installer: it doesn't actually copy files, register
  anything, or modify your system. The progress bar, log lines, and file sizes
  are all animated for show. It's a UI/demo shell, not a real software installer.
- The custom title bar (minimize / maximize / close) is wired to real window
  controls when running as a desktop app.
- To change the app name, icon, or version, edit the `build` section and top-level
  fields in `package.json`.
