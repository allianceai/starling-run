# Starling Run — Deployment Record

## Live URL

**https://starling-run.vercel.app**

(Game file direct URL: https://starling-run.vercel.app/starling-run.html)

## Platform

Vercel — Static Site (no build command, output directory: repo root)

## Project

- **Vercel Project:** cameron-hamiltons-projects/starling-run
- **GitHub Repo:** https://github.com/allianceai/starling-run
- **Inspector:** https://vercel.com/cameron-hamiltons-projects/starling-run

## Deployments

| Date | ID | URL | Status |
|------|----|-----|--------|
| 2026-08-01 | dpl_4HJR9XR5xXCCmwJY52s9uNpWATQb | https://starling-run.vercel.app | ✅ LIVE |

## Verification

- HTTP 200 confirmed on https://starling-run.vercel.app/ ✅
- HTTP 200 confirmed on https://starling-run.vercel.app/starling-run.html ✅
- Canvas element `<canvas id="c" width="480" height="480">` present ✅
- 3 embedded base64 image data URIs present ✅
- Audio engine (27 `audio` refs, `bgm` + `mute` toggle) embedded ✅
- index.html redirect from root → starling-run.html ✅

## How to Redeploy

```bash
cd /home/cameron/halie/projects/build-and-ship-starling-run-a-playable-browser-arcade-game-with-__project_goal_1785606762358
vercel --prod --yes --token "$VERCEL_TOKEN"
```
