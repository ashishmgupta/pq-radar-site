# PQ Radar

**Harvest now, decrypt later.** The traffic you send today can be stored and broken tomorrow. PQ Radar is a small landing page that makes the case in about ten seconds: two looping terminal recordings, side by side, showing whether a TLS handshake actually negotiated post-quantum key exchange.

## What it shows

- **Green demo** — a handshake that negotiates hybrid ML-KEM. Protected against harvest-now-decrypt-later.
- **Red contrast** — a handshake that falls back to classical key exchange only. Not protected.

Both recordings are plain `openssl` handshake output — nothing staged or simulated. The page's point is that checking this is boring and mechanical, which is exactly why it's worth automating.

Why this matters:
- Harvest-now-decrypt-later is a real, present threat: encrypted traffic captured today can be decrypted once a cryptographically relevant quantum computer exists.
- The fix (hybrid ML-KEM) is free and safe to deploy — it fails back to classical automatically, so nothing breaks.
- The only remaining question is whether it's actually turned on and actually negotiating.

## Structure

```
index.html              Single-page site: banner, two-player demo, "why" section
styles.css               Page layout (colors for the verdict live separately, in player/theme.css)
casts/
  green.cast              Recorded handshake — PQ-negotiated
  red.cast                 Recorded handshake — classical fallback
player/
  asciinema-player.min.js  Vendored asciinema player (self-hosted, no CDN dependency at runtime)
  asciinema-player.css     Player's own base styles
  theme.css                 Red/green verdict theme, driven by ANSI codes in the recordings
```

The page is static — no build step, no server-side code, no dependencies to install. Serve the directory as-is with any static file host.

## Running locally

```bash
npx serve .
# or
python3 -m http.server 8000
```

Then open `http://localhost:<port>/`.
