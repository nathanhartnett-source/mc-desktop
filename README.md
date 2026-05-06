# Mission Control Desktop

Tauri 2 wrapper around any Mission Control instance (`mc.allhart.com.au`, `obt.allhart.com.au`, etc).

## First launch

User enters MC URL → saved to localStorage → window navigates there. Subsequent launches go straight to the saved URL.

## Auto-updater

Checks `https://mc.allhart.com.au/desktop/latest.json` on launch. Manifest format:

```json
{
  "version": "0.1.1",
  "notes": "release notes",
  "pub_date": "2026-05-06T00:00:00Z",
  "platforms": {
    "darwin-x86_64": { "signature": "...", "url": "https://.../mc-desktop_0.1.1_x64.app.tar.gz" },
    "darwin-aarch64": { "signature": "...", "url": "..." },
    "linux-x86_64": { "signature": "...", "url": "..." },
    "windows-x86_64": { "signature": "...", "url": "..." }
  }
}
```

Signing keypair lives at `~/.tauri/mc-desktop.key{,.pub}` on Nathan's WSL.

## Building

### Local (Linux)
```
sudo apt-get install -y libwebkit2gtk-4.1-dev libgtk-3-dev libayatana-appindicator3-dev librsvg2-dev patchelf
npm install --include=dev
npm run build
```

### Windows / Mac
Push a tag (`git tag v0.1.0 && git push --tags`) — GitHub Actions builds `.exe` (NSIS + MSI) and `.dmg` (universal) and drops them on a draft release.

Required secrets in the repo:
- `TAURI_SIGNING_PRIVATE_KEY` — contents of `~/.tauri/mc-desktop.key`
- `TAURI_SIGNING_PRIVATE_KEY_PASSWORD` — empty string (key is unprotected)

## Code signing

Currently unsigned. First-launch will trigger SmartScreen (Windows) and Gatekeeper (Mac) warnings. Revisit when onboarding external clients.
