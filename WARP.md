# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview
**爱的守护者 (Love Guardian)** - An underwater-themed vertical scrolling shooter game built as a single HTML file with inline CSS and JavaScript. Players control a clownfish, shoot bubble bullets, collect power-ups, and battle through 20 stages or endless mode against various enemy fish.

## Development
This is a standalone HTML5 Canvas game with no build system or dependencies. Simply open `index.html` in a browser to run.

```bash
# Open in default browser (macOS)
open index.html

# Serve locally (optional, for development)
python3 -m http.server 8000
```

## Code Architecture
The entire game (~2100 lines) is contained in `index.html`:

**Structure:**
- Lines 1-313: HTML structure + CSS styles (screens, HUD, animations)
- Lines 375-2140: JavaScript game logic

**Key Globals:**
- `CONFIG` (line ~383): Game constants (speeds, durations, drop rates, boss timing)
- `WEAPON_CONFIG` (line ~414): Weapon types with cooldowns and parameters per upgrade level
- `STAGE_THEMES` (line ~1480): 20 stage themes defining enemy types and color palettes
- `BOSS_CONFIGS` (line ~1504): 20 unique boss configurations

**Core Systems:**
- `gameState`: Central state object (score, wave, power-ups, current stage, boss status)
- `player`, `bullets`, `enemies`, `powerUps`, `explosions`, `drones`: Game entity arrays

**Game Loop:**
- `gameLoop()` → `update()` + `draw()` at 60fps via `requestAnimationFrame`
- `update()`: Handles movement, shooting, spawning, collisions, game mode progression
- `draw()`: Renders all entities (bubbles, player fish, enemy fish, bullets, effects)

**Fish Rendering (all vector-based, no external images):**
- `drawPlayer()`: Draws player clownfish with bubble trail effect
- `drawEnemySmall()`: Draws enemy fish with configurable colors
- `drawBossLarge()`: Draws large boss fish
- `drawDrones()`: Draws helper fish companions

**Visual Theme:**
- Background: Deep ocean gradient (dark blue tones)
- Particles: Rising bubbles instead of falling stars
- Bullets: Bubble projectiles (blue for player, red for enemies)
- Shield: Bubble shield effect

**Game Modes:**
- Stage Mode: 20 levels, boss appears after defeating enough enemies + time threshold
- Endless Mode: Wave-based progression, bosses spawn at score thresholds

**Power-up System:**
- 5 weapon types (rapid, double, triple, spread, laser) with 3 upgrade levels each
- Shield (bubble invincibility) and Drone (helper fish) power-ups

**Controls:**
- Desktop: Arrow keys / WASD for movement, auto-fire enabled
- Mobile: Touch to move fish, auto-fire enabled

## Key Configuration Points
When modifying difficulty or gameplay:
- `CONFIG.STAGE_BOSS_BASE/PER_STAGE`: Controls when bosses appear in stage mode
- `CONFIG.ENDLESS_FIRST_BOSS_SCORE/NEXT_BOSS_ADD`: Boss spawn thresholds in endless mode
- `ENEMY_SHOOT_BASE`: Base enemy fire rates by type
- `WEAPON_CONFIG.*.cd`: Weapon cooldowns per level (lower = faster)
- `CONFIG.POWERUP_DROP_RATE`: Chance for enemies to drop power-ups
