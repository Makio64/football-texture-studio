# ⚽ Football — 3D Texture Studio

A single-file, in-browser 3D soccer-ball visualizer that **generates its own PBR textures procedurally** — albedo, normal, and bump maps — with one-click PNG export, World Cup 2026 team colourways, and a "download every team" zip.

**Live demo:** https://romainherve.github.io/football-texture-studio/

## What it does

- **Real geometry.** The ball is a *true truncated icosahedron* (60 vertices, 90 edges, 12 pentagons + 20 hexagons), built by walking the polyhedron's edge graph — not a Voronoi approximation, so the panels are correctly sized.
- **Procedural PBR maps.** For every texel of a 2:1 equirectangular map it finds the containing panel and builds:
  - **Albedo** — team colours + thin stitched seam line + ambient occlusion + leather grain.
  - **Bump** — the panel height field.
  - **Normal** — derived from the height gradient (with longitude-distortion correction).
- **Smoothly inflated panels.** Each panel is a radial dome × a product of per-edge ramps — curved across the whole panel with no flat facets and no medial-axis creases. Triangular-PDF dithering removes 8-bit banding even at high detail strength.
- **World Cup 2026 teams.** A floating team picker (confederation tabs + search) lets you recolour the ball in any of the 48 nations' kit/flag palettes (pentagons / hexagons / scattered accent panels).
- **Export.** Download individual maps, the current ball's full set, or **every team** as a single zip (shared normal + bump, one albedo per nation).

## Run locally

It's one file with no build step. Serve it (ES modules need http, not `file://`):

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

Three.js is loaded from a CDN via an import map.

## Tech

Vanilla JS + [Three.js](https://threejs.org/) (CDN). Textures are generated on `<canvas>`, applied to a `MeshStandardMaterial`, and exported with `canvas.toBlob`. The "all teams" zip uses a tiny built-in store-only ZIP writer.

🤖 Built with [Claude Code](https://claude.com/claude-code)
