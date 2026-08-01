# 🐦 Starling Run

> An endless side-scrolling browser game. Flap. Dodge. Survive.

**Single HTML file. Zero dependencies. No install. Just open and play.**

---

## 🎮 Play Now

👉 **[starling-run.vercel.app](https://starling-run.vercel.app)** *(live deployment)*

Or clone this repo and open `starling-run.html` directly in any browser.

---

## How to Play

A tiny starling is in freefall. Keep it airborne and guide it through branch obstacles that scroll faster as you survive longer.

- **Flap:** Tap the screen, click the mouse, or press `Space`
- **Avoid:** Branch obstacles scrolling right-to-left
- **Score:** Every obstacle pair cleared = +1 point
- **Best score** saved in your browser (localStorage) and shown on screen

Hit an obstacle or the ground → game over. Click/tap/space to restart instantly.

---

## Controls

| Action | Input |
|--------|-------|
| Flap up | `Space` / Click / Tap |
| Mute / Unmute music | 🔊 button (top-right) |
| Restart after game over | `Space` / Click / Tap |

---

## Running Locally

No build step. No server required.

```bash
git clone https://github.com/allianceai/starling-run.git
cd starling-run
open starling-run.html         # macOS
xdg-open starling-run.html    # Linux
# Or double-click starling-run.html in your file manager
```

Works in Chrome, Firefox, Safari, and Edge.

---

## Difficulty Curve

| Obstacles cleared | Scroll speed | Gap height | Feel |
|-------------------|-------------|------------|------|
| 0 | 2.8 px/frame | 160 px | Gentle warm-up |
| 10 | 3.6 px/frame | 145 px | Comfortable |
| 25 | 4.8 px/frame | 122 px | Engaging |
| 50 | 6.8 px/frame | 90 px | Hard |
| 70+ | 7.5 px/frame (cap) | 90 px (floor) | Maximum intensity |

---

## Architecture

Everything lives in one file: `starling-run.html`

- Pixel-art assets embedded as **base64 data URIs** — no external image files
- Background music embedded as a **base64 audio data URI** — plays on first input, loops indefinitely
- Mute state and best score persist via **localStorage**
- Physics: gravity `0.45 px/frame²`, flap impulse `-8.5 px/frame`, delta-time normalized for 60 fps displays

See [DESIGN.md](DESIGN.md) for the full mechanics, art direction, and audio spec.

---

## Project Files

| File | Purpose |
|------|---------|
| `starling-run.html` | The complete game — open this to play |
| `DESIGN.md` | Design document: mechanics, art, audio spec |
| `screenshots/` | Browser verification screenshots |

---

*Built with HTML5 Canvas + vanilla JS. No frameworks. No CDN. No build toolchain.*
