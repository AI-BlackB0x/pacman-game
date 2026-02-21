# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A browser-based Pac-Man game implemented as a single self-contained HTML file (`pacman.html`). No build tools, dependencies, or package manager — just open the file in a browser.

## Running

```bash
open pacman.html
```

## Architecture

Everything lives in `pacman.html`: HTML structure, CSS styles, and all game logic in a single `<script>` block.

**Game loop:** Uses `requestAnimationFrame` with a fixed-timestep accumulator (`TICK = 1000/10`, i.e. 10 game ticks/sec). Rendering runs every frame; game logic runs in `tick()`.

**Map:** A 21x21 grid defined as `BASE_MAP` (2D array of tile constants: `WALL=1`, `DOT=2`, `POWER=3`, `EMPTY=0`, `GHOST_HOUSE=4`). The active `map` is a mutable copy created at game start.

**Key state variables:** `map`, `pacman`, `ghosts[]`, `gameState` (`'start'`/`'playing'`/`'gameover'`/`'win'`), `frightenedTimer`.

**Ghost AI:** Each ghost (blinky, pinky, inky, clyde) has a unique targeting strategy in `moveGhost()`. Movement uses `moveGhostToward()` which picks the best non-reverse direction toward the target. Ghosts start in the ghost house with staggered `homeTimer` delays.

**Important pattern:** `draw()` must handle the pre-init state (before SPACE is pressed) by falling back to `BASE_MAP` and skipping entity rendering when `pacman`/`ghosts` are null.
