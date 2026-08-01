# 🐦 Starling Run

An endless side-scrolling arcade game — 100% browser-native. Single self-contained HTML file, no frameworks, no CDN, no build step.

**[▶ Play Now](https://starling-run.vercel.app)**

---

## How to Play

Guide your starling through an endless gauntlet of branch obstacles. Every obstacle you clear scores a point. How far can you go?

### Controls

| Action | Input |
|--------|-------|
| Flap / Jump | `Spacebar` |
| Flap / Jump | Mouse click anywhere |
| Flap / Jump | Tap (mobile/touch) |
| Mute / Unmute | 🔇 button (top-right corner) |
| Restart (game over) | `Spacebar` / Click / Tap |

### Scoring

- **Score** — number of obstacles cleared in the current run
- **Best** — your personal best, saved in `localStorage` (persists between sessions)

### Difficulty Curve

The game gets harder as you clear more obstacles:
- Scroll speed increases with every obstacle cleared
- The gap between top and bottom obstacles shrinks gradually (floor: 90 px)
- No ceiling — survive as long as you can

---

## Run Locally

No server required. Just open the file:

```bash
# Double-click in Finder / File Explorer, or:
open starling-run.html

# Local dev server (avoids any file:// quirks with audio)
npx serve . -p 3000
# → http://localhost:3000/starling-run.html

# Python fallback
python3 -m http.server 3000
# → http://localhost:3000/starling-run.html
```

---

## Project Structure

```
starling-run/
├── starling-run.html      ← entire game (self-contained, ~5 MB with embedded assets)
├── DESIGN.md              ← mechanics spec, difficulty curve, art & audio direction
├── assets/
│   ├── starling.png       ← pixel-art bird sprite
│   ├── obstacle.png       ← branch/log obstacle pair
│   ├── background.png     ← 3-layer parallax sky/midground/ground strip
│   └── bgm.wav            ← original chiptune loop (140 BPM, C major)
└── screenshots/           ← gameplay screenshots
```

---

## Tech Stack

- **Vanilla JavaScript** — canvas 2D API, `requestAnimationFrame` game loop
- **HTML5 Canvas** — all rendering, zero DOM sprites
- **Web Audio API** — chiptune BGM with mute toggle
- **localStorage** — persistent best score
- **Zero external dependencies** — no npm, no CDN, no build step

---

## Design

See [`DESIGN.md`](./DESIGN.md) for the full mechanics spec:
- Flap physics (gravity constant, impulse, terminal velocity)
- Difficulty curve parameters (speed increment, gap shrink rate, floors)
- Art direction (pixel-art style, palette, parallax layers)
- Audio direction (chiptune key, BPM, loop structure, mute toggle UX)

---

## License

MIT
