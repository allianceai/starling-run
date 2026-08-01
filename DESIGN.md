# Starling Run — Design Document

```
 ____  _             _ _               ____
/ ___|| |_ __ _ _ __| (_)_ __   __ _  |  _ \ _   _ _ __
\___ \| __/ _` | '__| | | '_ \ / _` | | |_) | | | | '_ \
 ___) | || (_| | |  | | | | | | (_| | |  _ <| |_| | | | |
|____/ \__\__,_|_|  |_|_|_| |_|\__, | |_| \_\\__,_|_| |_|
                                 |___/
```

> A single-file HTML5 endless side-scroller. Flap. Dodge. Survive.

---

## 1. Core Mechanics

### 1.1 Flap Physics

| Parameter | Value | Notes |
|-----------|-------|-------|
| Gravity constant | `0.45 px/frame²` | Applied every frame to vertical velocity |
| Flap impulse | `-8.5 px/frame` | Negative = upward; applied on click / tap / spacebar |
| Terminal velocity (fall) | `12 px/frame` | Velocity clamped to prevent instant death on long drops |
| Terminal velocity (rise) | `-10 px/frame` | Clamp prevents double-tap rocket launches |
| Canvas height | `480 px` | Logical resolution (scaled to viewport) |
| Bird Y start | `canvas.height / 2` | Centre of screen at game start |

**Frame loop:** `requestAnimationFrame` at native refresh. All speeds are expressed in px/frame assuming 60 fps; on faster displays the delta-time multiplier `dt = elapsed / 16.67` keeps physics consistent.

### 1.2 Obstacle Gap

| Parameter | Value |
|-----------|-------|
| Initial gap height | `160 px` |
| Minimum gap floor | `90 px` |
| Gap reduction per obstacle | `1.5 px` (floored at minimum) |
| Obstacle width | `52 px` |
| Obstacle spawn X | `canvas.width + 10 px` |
| Horizontal spacing | `280 px` between leading edges |

Gap centre Y is randomised each spawn: `rand(gapHalf + 20, canvas.height - groundHeight - gapHalf - 20)`.

---

## 2. Difficulty Curve

### 2.1 Scroll Speed

| Parameter | Value |
|-----------|-------|
| Starting scroll speed | `2.8 px/frame` |
| Speed increment per obstacle cleared | `+0.08 px/frame` |
| Hard speed cap | `7.5 px/frame` |

Speed ramps smoothly — the player feels a meaningful surge around obstacle 15 (≈ 4 px/frame) and again near obstacle 40 (cap zone). The gap is narrowing simultaneously, so late-game difficulty comes from both axes.

### 2.2 Pacing Summary

| Obstacle count | Scroll speed | Gap height | Feel |
|----------------|-------------|------------|------|
| 0 | 2.8 | 160 px | Tutorial-gentle |
| 10 | 3.6 | 145 px | Comfortable |
| 25 | 4.8 | 122 px | Engaging |
| 50 | 6.8 | 90 px (floor) | Hard |
| 70+ | 7.5 (cap) | 90 px | Max difficulty |

### 2.3 Lives / Restart

Single life. Collision with obstacle or ground triggers `GAME OVER` state immediately. No invincibility frames. The restart input (click / tap / spacebar on game-over screen) resets all state and returns to the title screen.

---

## 3. Art Direction

### 3.1 Starling Sprite

- **Canvas size:** 16 × 16 px
- **Palette:** 5 colours — dark navy body (`#1a1a2e`), iridescent teal wing highlight (`#00d4aa`), warm white belly (`#f5f0e8`), amber beak/eye (`#f5a623`), soft black outline (`#0d0d1a`)
- **Frames:** 2-frame flap cycle
  - Frame 0 (glide): wings level, body horizontal
  - Frame 1 (flap): wings swept up ≈ 30°, body tilts nose-down 5°
- **Tilt:** Bird sprite rotates with velocity — capped at +25° (dive) / −20° (climb) for readability
- **Size on canvas:** Drawn at 32 × 32 px (2× nearest-neighbour upscale) for crisp pixel look

### 3.2 Obstacle Style — Branch Pipes

Inspired by natural perch obstacles rather than mechanical pipes:

- **Shape:** Thick wooden log/branch, flat top and bottom sections that frame the gap
- **Palette:** Bark brown (`#5c3d1e`), highlight grain (`#8b5e3c`), dark shadow edge (`#2d1a0a`), moss accent tip (`#4a7c3f`)
- **Cap:** Each branch end has a rough torn-wood cap (3 px overhang each side) — no smooth pipe rims
- **Width:** 52 px; drawn as a pair (top branch hangs down, bottom branch rises up)

### 3.3 Parallax Background — 3 Layers

All layers scroll right-to-left. Scroll speed = `obstacle speed × layer multiplier`.

| Layer | Subject | Multiplier | Description |
|-------|---------|------------|-------------|
| **Far** (sky) | Gradient sky + distant clouds | `0.15×` | Deep twilight gradient `#0d1b4b → #1a3a6b → #2e6b8a`; soft white cloud puffs |
| **Mid** (treeline) | Silhouetted tree canopy | `0.40×` | Dark teal-black treetop silhouette strip, 80 px tall, bottom-anchored at ground level + 80 |
| **Near** (ground) | Ground strip + grass tufts | `1.0×` (same as obstacles) | 40 px brown earth strip; scattered pixel grass blades in `#3a7d3a` |

Each layer tiles seamlessly (canvas width + tile width, wraps on overflow).

---

## 4. Audio Direction

### 4.1 Chiptune Background Loop

| Parameter | Value |
|-----------|-------|
| Key | C major |
| BPM | 140 |
| Loop length | 8 bars (≈ 3.4 seconds at 140 BPM) |
| Waveform | Square wave (lead melody) + Triangle wave (bass) |
| Generation | Web Audio API — procedurally synthesised, no external file |

**Melody outline (8-bar loop):**

```
Bar 1-2:  C5 E5 G5 E5 | C5 D5 E5 —
Bar 3-4:  G4 B4 D5 B4 | G4 A4 B4 —
Bar 5-6:  A4 C5 E5 C5 | A4 B4 C5 —
Bar 7-8:  G4 E5 D5 C5 | G4 — C5 —
```

Each note = 1 eighth note at 140 BPM (≈ 107ms). Rests marked `—`.

**Bass line:** Root notes C3 / G3 / A3 / G3 on bar downbeats, triangle wave, volume 0.3.

**Flap SFX:** Short 80ms chirp — sine wave sweep `880 Hz → 1200 Hz`, volume 0.25.

**Hit SFX:** 120ms descending buzz — square wave `440 Hz → 110 Hz`, volume 0.4.

### 4.2 Mute Toggle

- **Icon:** Speaker emoji button (🔊 / 🔇) pinned top-right corner, 36 × 36 px tap target
- **Behaviour:** Toggles Web Audio `gainNode.gain.value` between `1.0` and `0.0` — no audio gap/click
- **Persistence:** Mute state saved to `localStorage` key `starling_muted`; restored on page load
- **Audio start rule:** AudioContext is created (or resumed) on the **first user gesture** (click/tap/spacebar) to comply with browser autoplay policy — music does not start until the player interacts

---

## 5. Single-File Architecture

```
starling-run.html
├── <style>         Canvas centering, body bg, HUD font (system monospace)
├── <canvas>        id="c", logical 480×480, CSS max-width 100vw
├── <button>        Mute toggle, position:fixed top-right
└── <script>
    ├── Assets      Base64 data URIs — sprite sheet PNG, obstacle PNG
    ├── Audio       Web Audio API synth (no external files)
    ├── State       TITLE | PLAYING | DEAD enum
    ├── Game loop   requestAnimationFrame + delta-time
    ├── Input       pointerdown + keydown (spacebar)
    ├── Renderer    Parallax layers → obstacles → bird → HUD
    └── Storage     localStorage best score + mute flag
```

No build step. No CDN. Open the file → play.

---

## 6. Scoring & HUD

| Element | Position | Style |
|---------|----------|-------|
| Current score | Top-centre, large | White monospace, 28px, shadow |
| Best score | Below current | Smaller, `#f5a623` amber |
| Mute toggle | Top-right | Fixed position button |

Score = number of obstacle pairs fully cleared (bird X passes obstacle right edge).
Best score persists via `localStorage` key `starling_best`.

---

*Design version 1.0 — Starling Run, 2026*
