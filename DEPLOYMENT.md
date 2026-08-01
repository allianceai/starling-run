# Starling Run — Deployment

## Live URL
<!-- FILL_URL_HERE — update after `vercel --prod` completes -->

## Deploy Date
2026-08-01

## Platform
Vercel (static, no build step)

## Source
https://github.com/allianceai/starling-run

## Notes
- Single self-contained HTML5 file (~674 lines) with procedural canvas drawing and Web Audio API
- No CDN dependencies, no build process, no external assets required
- All sprites drawn with canvas 2D API; all audio generated with Web Audio API
- Deployed via `vercel --prod --yes --token "$VERCEL_TOKEN"`
- Assets directory (assets/) also served statically for reference
