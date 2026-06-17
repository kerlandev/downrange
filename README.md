# Downrange

A 2D physics-destruction artillery game, browser-based, no install. Aim a cannon, lob shells across a battlefield, and watch hand-sketched fortresses come apart block by block.

**[Play it live →](https://kerlandev.github.io/downrange)**

Inspired by *Worms* and *Angry Birds*, with a pencil-sketch / military-technical-illustration look — cool greys on off-white paper, CRT-green radar minimap, amber odometer-style HUD digits.

---

## What it is

- Single-player campaign across 6 levels, or local two-player (shared keyboard, mirrored battlefields)
- Real rigid-body physics via Matter.js — structures aren't sprites with health bars, they're physical blocks that crack, crumble, and topple
- Cluster-split ordnance: split your shell mid-flight into 3, then 9 sub-munitions for spread damage
- Ammo crates chain-react based on blast proximity — clip the wrong one and bring down a whole stockpile
- Everything renders to a single HTML5 Canvas, no game engine, no build step

## Built with

- HTML5 Canvas (2D context)
- [Matter.js](https://brm.io/matter-js/) for physics
- Web Audio API for sound
- One HTML file. That's the whole codebase.

## How this got built

This was built almost entirely through AI-assisted development — roughly **$120 total in Anthropic API credits**, prompt by prompt, across iterative sessions.

The best debugging story from the build: structures would sit fine after impact, but follow-up explosions near already-settled debris just... did nothing. No errors, no warnings, blocks visibly there but physically inert. Hours went into the explosion-force math before a single `console.log(block.body.isSleeping)` turned up the actual cause — Matter.js had quietly put resting bodies to sleep as a performance optimization, and sleeping bodies don't recompute physics until something hits them hard enough to wake them. A follow-up blast within a sleeping body's radius was, physically, shouting at someone with headphones on. Disabling sleep engine-wide fixed it in one line. The lesson stuck around longer than the bug did: when something "should" be reacting and isn't, check whether it's actually being asked to react at all.

## Known limitations

- Built and tuned for desktop. A mobile-responsive layout pass is in progress, but this isn't getting full touch controls — best experience is still desktop with a keyboard.
- The coffee/tip button is real but not yet pointed at a live account.

## Running it locally

```bash
git clone https://github.com/kerlandev/downrange.git
cd downrange
python3 -m http.server 8000
# open http://localhost:8000 — audio needs the local server, won't work from a raw file:// open
```
