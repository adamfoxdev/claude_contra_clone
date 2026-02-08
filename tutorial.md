# CONTRA FORCE — Developer Guide & Code Walkthrough

> A comprehensive guide to understanding, modifying, and extending the **CONTRA FORCE** HTML5 arcade shooter.  
> File: `contra_opus4_6.html` (~12,900 lines, single-file architecture)

---

## Table of Contents

1. [Architecture Overview](#1-architecture-overview)
2. [File Structure Map](#2-file-structure-map)
3. [The Game Loop](#3-the-game-loop)
4. [Game States & Flow](#4-game-states--flow)
5. [HTML & CSS Layer (Lines 1–1444)](#5-html--css-layer)
6. [Core Settings & Constants (Lines 1456–1612)](#6-core-settings--constants)
7. [Audio Engine (Lines 1613–1855)](#7-audio-engine)
8. [Music Engine (Lines 1856–2033)](#8-music-engine)
9. [Save / Load System (Lines 2034–2201)](#9-save--load-system)
10. [Level File I/O (Lines 2202–2256)](#10-level-file-io)
11. [Shareable Leaderboard Codes (Lines 2257–2319)](#11-shareable-leaderboard-codes)
12. [Touch Controls (Lines 2320–2432)](#12-touch-controls)
13. [Gamepad Support (Lines 2433–2558)](#13-gamepad-support)
14. [Input System (Lines 2559–2604)](#14-input-system)
15. [Mobile Touch Input (Lines 2605–2708)](#15-mobile-touch-input)
16. [Game Modes (Lines 2714–2973)](#16-game-modes)
17. [Mode Logic Functions (Lines 2974–3192)](#17-mode-logic-functions)
18. [Story Mode & Cutscene Engine (Lines 3193–3529)](#18-story-mode--cutscene-engine)
19. [Achievements & Leaderboard (Lines 3530–3933)](#19-achievements--leaderboard)
20. [Visual Effects Systems (Lines 3934–4272)](#20-visual-effects-systems)
21. [Title Screen, Demo, Credits, Tutorial, Particle Editor (Lines 4274–5079)](#21-title-screen-demo-credits-tutorial-particle-editor)
22. [Stage Themes (Lines 5080–5187)](#22-stage-themes)
23. [Level Generation (Lines 5202–5712)](#23-level-generation)
24. [Weapons, Vehicles & Characters (Lines 5713–5984)](#24-weapons-vehicles--characters)
25. [Player, Enemies & Entities (Lines 5985–6355)](#25-player-enemies--entities)
26. [Drawing Helpers & Tile Rendering (Lines 6356–6762)](#26-drawing-helpers--tile-rendering)
27. [Sprite Sheet System (Lines 6763–7343)](#27-sprite-sheet-system)
28. [Background & Tile Drawing (Lines 7344–7727)](#28-background--tile-drawing)
29. [Collision System (Lines 7728–7747)](#29-collision-system)
30. [Game Init (Lines 7748–7797)](#30-game-init)
31. [Update Loop (Lines 7798–9973)](#31-update-loop)
32. [Draw Function (Lines 9974–11147)](#32-draw-function)
33. [Overlay System (Lines 11148–11166)](#33-overlay-system)
34. [Online Multiplayer — WebRTC (Lines 11167–11803)](#34-online-multiplayer)
35. [Game Loop & Start Functions (Lines 11804–11955)](#35-game-loop--start-functions)
36. [Character Select Screen (Lines 11956–12213)](#36-character-select-screen)
37. [Admin Panel (Lines 12264–12918)](#37-admin-panel)
38. [How-To Recipes](#38-how-to-recipes)

---

## 1. Architecture Overview

CONTRA FORCE is a **single-file** HTML5 Canvas game. Everything — HTML, CSS, JavaScript — lives in one `.html` file. This makes it trivially deployable (just open in a browser) but means you need to navigate by line numbers and section comments.

### Key Technologies
| Tech | Usage |
|------|-------|
| **HTML5 Canvas** | All game rendering (800×450 native, scaled to fit window) |
| **Web Audio API** | Procedural sound effects (27 sounds) and chip-tune music (5 tracks) |
| **WebRTC** | Peer-to-peer online co-op multiplayer |
| **localStorage** | Save/load game state, achievements, leaderboards, settings |
| **Gamepad API** | Controller support for up to 2 players |
| **requestAnimationFrame** | 60fps game loop |

### Rendering Pipeline
```
gameLoop() → update() → draw()
                          ├── drawBackground()
                          ├── drawTiles()
                          ├── drawEntities (enemies, player, bullets, particles, etc.)
                          ├── drawHUD()
                          ├── applyCRT() (post-processing)
                          └── drawWeather() (overlay)
```

### Core Global Objects
| Variable | Type | Purpose |
|----------|------|---------|
| `canvas` / `ctx` | Canvas/Context2D | Main rendering surface |
| `player` | Object | Player 1 state (position, velocity, weapon, lives, etc.) |
| `player2` | Object | Player 2 state (co-op mode) |
| `enemies[]` | Array | All active enemies including bosses |
| `bullets[]` | Array | Player bullets |
| `enemyBullets[]` | Array | Enemy projectiles |
| `particles[]` | Array | Visual particle effects |
| `powerups[]` | Array | Weapon/life pickups |
| `objects[]` | Array | Destructible environmental objects |
| `level` | Object | Current level data (`map[][]`, `cols`, `rows`, `groundRow`) |
| `stage` | Number | Current stage number (1–10) |
| `gameState` | String | Current state (`title`/`playing`/`gameover`/`victory`/`charSelect`/`demo`) |

---

## 2. File Structure Map

The file is organized in this order (approximate line ranges):

```
Lines 1–1444       HTML + CSS (DOM layout, styles, admin panel HTML, overlay, HUD)
Lines 1445–1470    Script start, canvas/context setup
Lines 1471–1612    Difficulty system, core settings & constants
Lines 1613–2033    Audio engine (SFX) + Music engine (procedural chip-tune)
Lines 2034–2432    Save/Load, Level I/O, Share codes, Touch controls
Lines 2433–2708    Gamepad support, Input system, Mobile touch
Lines 2714–2973    Game modes (variables, game-over handler, leaderboards)
Lines 2974–3192    Mode logic (Endless, Boss Rush, Time Attack, NG+, Challenges)
Lines 3193–3529    Story mode data + Cutscene renderer
Lines 3530–3933    Achievements + Leaderboard system
Lines 3934–4272    Visual effects (CRT shader, Weather, Transitions, Lighting, Animated tiles)
Lines 4274–5079    Title screen, Demo mode, Credits roll, Tutorial, Particle editor
Lines 5080–5200    Stage themes (10 themes with colors, tile styles, terrain flags)
Lines 5202–5712    Level generation (procedural)
Lines 5713–5984    Weapons, Vehicles, Characters definitions
Lines 5985–6355    Player creation, Enemy types & spawning, Powerups, Bullets, Objects
Lines 6356–6762    Drawing helpers (drawPlayer, drawEnemy, drawBoss, drawBullet, etc.)
Lines 6763–7343    Animated sprite sheet system (procedural generation)
Lines 7344–7727    Background rendering, Tile drawing (with animated water/lava)
Lines 7728–7797    Collision detection (AABB, tile, ray)
Lines 7798–9973    UPDATE function (player physics, enemy AI, bullet logic, boss patterns)
Lines 9974–11147   DRAW function (full frame rendering pipeline)
Lines 11148–11166  Overlay show/hide helpers
Lines 11167–11803  Online multiplayer (WebRTC P2P)
Lines 11804–11955  Game loop + Start game function
Lines 11956–12263  Character select screen
Lines 12264–12918  Admin panel logic, slider bindings, editor, minimap, boot code
```

---

## 3. The Game Loop

**Location:** ~Line 11804

The heart of the game. Runs at 60fps via `requestAnimationFrame`.

```javascript
function gameLoop() {
  pollGamepads();              // Read controller input
  updateGpHud();               // Update gamepad HUD overlay

  // Gamepad button handling (Start to begin, A to advance cutscene)
  
  if (csActive) { ... return; }         // Cutscene playing — skip game logic
  if (freezeFrames > 0) { ... return; } // Hit-freeze — skip update, still draw
  if (paused) { ... return; }           // Paused — draw with overlay

  // Slow motion handling (skip updates on certain frames)

  update();                    // ALL game logic
  updateShake();               // Screen shake
  updateFlash();               // Screen flash
  updatePopups();              // Floating text popups
  updateMusic();               // Music engine tick
  updateWeather();             // Weather particles
  updateTransition();          // Screen transition effects
  updateLights();              // Dynamic light sources
  updateAnimatedTiles();       // Water/lava animation
  updateTutorial();            // Tutorial step checking
  mpTick();                    // Multiplayer sync

  // State-based rendering:
  if (gameState === 'playing')    draw() + drawTutorialHUD()
  if (gameState === 'charSelect') drawCharSelect()
  if (gameState === 'title')      drawTitleScreen()
  if (gameState === 'demo')       updateDemoAI() + draw() + drawDemoOverlay()
  if (creditsActive)              updateCredits() + drawCredits()

  requestAnimationFrame(gameLoop);
}
```

### Important: `update()` guard
The `update()` function (line ~7798) returns immediately unless `gameState === 'playing'` or `gameState === 'demo'`:
```javascript
function update() {
  if (gameState !== 'playing' && gameState !== 'demo') return;
  // ...
}
```

---

## 4. Game States & Flow

```
                    ┌──────────────────────────────────────┐
                    │                                      │
   BOOT ──► TITLE ──┬──► startGameSession() ──► CHAR_SELECT ──► PLAYING
                    │                                              │
                    │   (idle 10s)                                 │
                    ├──► DEMO (auto-play) ──── (any key) ──► TITLE │
                    │                                              │
                    │   (tutorial button)                          │
                    └──► TUTORIAL (playing state with tutorialActive=true)
                                                                   │
                                         ┌─────────────────────────┤
                                         │                         │
                                     GAME OVER                  VICTORY
                                         │                         │
                                         │                   CREDITS ROLL
                                         │                         │
                                         └────── showOverlay() ────┘
                                                     │
                                                  TITLE (loop)
```

### State values for `gameState`:
- `'title'` — Title screen with menu overlay
- `'charSelect'` — Character selection grid
- `'playing'` — Active gameplay
- `'demo'` — Demo auto-play mode
- `'gameover'` — Game over (via overlay)
- `'victory'` — Victory (via overlay)
- `'cutscene'` — Cutscene playing (handled by cutscene engine)

---

## 5. HTML & CSS Layer

**Lines 1–1444**

### Key DOM Elements
| Element ID | Purpose |
|-----------|---------|
| `#gameWrapper` | Container div, scaled with CSS transform |
| `#game` (canvas) | 800×450 canvas — all game rendering |
| `#overlay` | Title/menu/gameover overlay (DOM, not canvas) |
| `#hud` | In-game HUD (health, score, weapon — DOM overlay) |
| `#adminPanel` | Admin/debug panel with dozens of tweakable settings |
| `#adminToggleBtn` | Floating gear icon to open admin panel |
| `#achievementToast` | Pop-up notification for achievements |
| `#achievementsPanel` | Full achievement list panel |
| `#leaderboardPanel` | Leaderboard display |
| `#mpModalContainer` | Multiplayer connection modal |

### Admin Panel Sections (Lines ~1147–1430)
The admin panel has collapsible sections:
- Physics (gravity, speed, jump force)
- Combat (damage, fire rate, bullet speed)
- Achievements (reset)
- Keybinds (rebindable controls)
- Weapons (give any weapon)
- Game (god mode, stage select, respawn)
- Spawn (place enemies interactively)
- Level Editor (brush tools, tile placement)
- Save/Load (full game state)
- Level File I/O (export/import levels as JSON)
- Audio (music volume, SFX volume)
- Touch Controls (opacity, vibration, size)
- Online Multiplayer (host/join)
- Gamepad (deadzone, debugging)
- Sprites (toggle sprite vs. procedural rendering)
- Story Mode (toggle, skip to cutscene)
- Characters, Vehicles & Secrets
- Visual Effects (CRT, weather, lighting, animated tiles)
- Particle Editor (live-tweakable particle presets)

---

## 6. Core Settings & Constants

**Lines 1456–1612**

```javascript
const GAME_W = 800;          // Native canvas width
const GAME_H = 450;          // Native canvas height
const TILE = 32;             // Tile size in pixels
let gravity = 0.55;          // Gravity acceleration
let playerSpeed = 3;         // Horizontal movement speed
let jumpForce = -10;         // Jump velocity (negative = up)
let bulletSpeed = 10;        // Default bullet speed
let bulletDamage = 1;        // Default bullet damage
let maxBullets = 30;         // Max bullets on screen
let invincibleTime = 120;    // Frames of invincibility after respawn
let godMode = false;         // Admin: can't die
```

### Difficulty System (Lines 1471–1612)

Five presets: EASY, NORMAL, HARD, NIGHTMARE, CUSTOM.

```javascript
const DIFFICULTY_PRESETS = {
  easy:      { lives: 7, enemyHpMult: 0.6, scoreMult: 0.5, ... },
  normal:    { lives: 3, enemyHpMult: 1.0, scoreMult: 1.0, ... },
  hard:      { lives: 2, enemyHpMult: 1.5, scoreMult: 1.8, ... },
  nightmare: { lives: 1, enemyHpMult: 2.5, scoreMult: 3.0, ... },
  custom:    { /* inherits current diffSettings */ },
};
```

The active settings live in the `diffSettings` object. `applyDifficulty()` copies a preset into `diffSettings`.

**To add a new difficulty preset:** Add a new key to `DIFFICULTY_PRESETS`, add a button in the overlay HTML `#diffSelector`, and handle it in `setDifficulty()`.

---

## 7. Audio Engine

**Lines 1613–1855**

Procedural sound effect synthesis using Web Audio API. No audio files needed.

```javascript
function playSound(name) { ... }
```

### Sound Names
`shoot`, `hit`, `explosion`, `die`, `pickup`, `jump`, `boss_hit`, `boss_die`, `laser`, `spread`, `machine_gun`, `flame`, `rocket_launch`, `homing_lock`, `shield_hit`, `game_over`, `combo`, `achievement`, `wave_fire`, `lightning`, `boomerang`, `blackhole`, `mine_place`, `mine_explode`, `dig`, `treasure`, `victory`

Each sound is generated by creating oscillator/gain nodes with carefully tuned frequency sweeps, noise bursts, and envelopes.

**To add a new sound:** Add a new `case` in the `playSound()` switch statement. Create oscillator nodes with `audioCtx.createOscillator()`, set frequency/type, connect to gain node, and schedule with `.start()` / `.stop()`.

---

## 8. Music Engine

**Lines 1856–2033**

Procedural chip-tune music with 5 tracks, generated entirely via Web Audio API.

```javascript
function startMusic(trackNum) { ... }
function stopMusic() { ... }
function updateMusic() { ... }  // Called each frame in gameLoop
```

### Tracks
1. **Track 1** — Jungle/Action (default for stages 1-2)
2. **Track 2** — Mysterious/Ruins (stages 3-4)
3. **Track 3** — Intense/Boss (boss encounters)
4. **Track 4** — Space/Ethereal (stages 5-6)
5. **Track 5** — Final/Epic (stages 7+)

Music auto-selects based on `stage` number. Each track defines note sequences for melody, bass, and percussion channels.

---

## 9. Save / Load System

**Lines 2034–2201**

Full game state serialization to localStorage.

```javascript
function saveGameState() { ... }    // Serializes everything to JSON
function loadGameState() { ... }    // Restores from JSON
```

Saves: player position/state, enemies, bullets, particles, level map, score, stage, game mode, achievements, settings.

Quick save/load hotkeys: **F5** (save) / **F9** (load).

---

## 10. Level File I/O

**Lines 2202–2256**

Export/import levels as JSON files:
```javascript
function downloadLevelFile() { ... }   // Downloads .json file
function uploadLevelFile() { ... }     // Uploads and applies .json file
```

The JSON includes: `map[][]`, `cols`, `rows`, `groundRow`, `enemies`, `powerups`, `objects`.

---

## 11. Shareable Leaderboard Codes

**Lines 2257–2319**

Base64-encoded score sharing:
```javascript
function encodeScoreCode(entry) { ... }  // Entry → base64 string
function decodeScoreCode(code) { ... }   // base64 string → entry
```

Players can export/import scores as text codes for sharing.

---

## 12–15. Input Systems

### Touch Controls (Lines 2320–2432)
Virtual D-pad and buttons for mobile. Touch zones mapped to game inputs.

### Gamepad Support (Lines 2433–2558)
Uses Gamepad API with per-frame polling. Gamepad 0 → Player 1, Gamepad 1 → Player 2.

```javascript
function pollGamepads() { ... }   // Called every frame
function gpPressed(idx, btn) { ... }
function gpJustPressed(idx, btn) { ... }
function gpRumble(idx, duration, weak, strong) { ... }
```

Button constants: `GP_A`, `GP_B`, `GP_X`, `GP_Y`, `GP_START`, etc.

### Keyboard Input (Lines 2559–2604)
```javascript
// Player 1: Arrow keys / WASD + Space/Z (shoot) + X (jump) + C (grapple)
let isLeft, isRight, isUp, isDown, isJump, isShoot, isGrapple;

// Player 2: IJKL + U (shoot) + O (jump) + P (grapple)
let isLeft2, isRight2, isUp2, isDown2, isJump2, isShoot2, isGrapple2;
```

**Rebindable controls** are handled via the admin panel "Keybinds" section.

---

## 16. Game Modes

**Lines 2714–2973**

Six game modes with unique rules:

| Mode | Variable | Description |
|------|----------|-------------|
| Campaign | `gameMode = 'campaign'` | Classic 10-stage progression |
| Endless | `gameMode = 'endless'` | Infinite waves, increasing difficulty |
| Boss Rush | `gameMode = 'bossrush'` | Boss after boss, timed |
| Time Attack | `gameMode = 'timeattack'` | Speed-run with countdown timer |
| New Game+ | `gameMode = 'newgameplus'` | Harder replay with scaling difficulty |
| Challenge | `gameMode = 'challenge'` | Specific objectives (no-death, pistol-only, etc.) |

### Key mode variables:
```javascript
let gameMode = 'campaign';
let endlessWave = 0, endlessKills = 0, endlessKillsNeeded = 10;
let bossRushIdx = 0, bossRushTimer = 0;
let timeAttackTimer = 0;
let ngPlusLevel = 0, ngPlusWeapon = 'NORMAL';
let challengeState = { ... };
```

**To add a new game mode:**
1. Add mode variable and state tracking in the game modes section
2. Add an init function (e.g., `initMyMode()`) in mode logic
3. Add a button in the overlay mode selector (both static HTML and `showOverlay()`)
4. Handle in `actuallyStartGame()` to call your init function
5. Handle in `handleModeGameOver()` for scoring/leaderboard
6. Handle in `update()` for mode-specific logic (e.g., wave progression)
7. Handle in `registerKill()` for kill-based progression
8. Add to `getModeDesc()` for UI description

---

## 17. Mode Logic Functions

**Lines 2974–3192**

Implementation of each mode's game logic:

- **Endless Mode** (`initEndlessMode`, `spawnEndlessWave`, `checkEndlessWaveComplete`)
- **Boss Rush** (`initBossRush`, `spawnBossRushBoss`, `advanceBossRush`)
- **Time Attack** (`initTimeAttack`, `updateTimeAttack`)
- **New Game+** (`initNGPlus`, `canStartNGPlus`)
- **Challenges** (`showChallengeSelect`, `startChallenge`, `checkChallengeObjective`)

---

## 18. Story Mode & Cutscene Engine

**Lines 3193–3529**

### Story Data (Lines 3201–3302)
```javascript
const STORY = {
  intro: { scenes: [ { bg: '...', text: '...', speaker: '...' }, ... ] },
  stage1: { scenes: [...] },
  // ... one entry per stage + epilogue
  epilogue: { scenes: [...] },
};
```

### Cutscene Renderer (Lines 3303–3529)
```javascript
function playCutscene(key, callback) { ... }  // Start a cutscene
function csAdvance() { ... }                   // Next scene/line
function csRender() { ... }                    // Draw current frame
```

Cutscenes use a letterbox-style presentation with typed text, speaker names, and gradient backgrounds. Player can advance with Enter/Space/A-button or skip with Escape.

**To add a new cutscene:** Add a new key to the `STORY` object with a `scenes` array. Each scene has `bg` (background description for gradient), `text` (dialogue), and optional `speaker`.

---

## 19. Achievements & Leaderboard

**Lines 3530–3933**

### Achievement Definitions (Line 3573)
```javascript
const ACHIEVEMENT_DEFS = {
  first_blood:  { name: 'First Blood', desc: 'Kill your first enemy', icon: '🩸' },
  untouchable:  { name: 'Untouchable', desc: 'Complete a stage without dying', icon: '🛡️' },
  // ... 25 total achievements
};
```

### Key Functions
```javascript
function unlockAchievement(id) { ... }  // Unlock + show toast + save
function checkAchievements() { ... }    // Check all conditions
function submitScore(outcome) { ... }   // Add to leaderboard
```

### Run Stats (Line 3534)
Reset each game session:
```javascript
let runStats = {
  kills: 0, bossKills: 0, deaths: 0,
  weaponsCollected: new Set(),
  treasuresFound: 0, maxCombo: 0,
  startTime: 0, stagesCleared: 0,
  stageDeaths: 0, noDeathRun: true,
};
```

**To add a new achievement:**
1. Add entry to `ACHIEVEMENT_DEFS` with `name`, `desc`, `icon`
2. Add check condition in `checkAchievements()` or call `unlockAchievement('id')` where the condition is met

---

## 20. Visual Effects Systems

**Lines 3934–4272**

### CRT / Retro Shader (Line 3938)
Post-processing effect using an offscreen canvas:
```javascript
let crtEnabled = false;
let crtScanlines = true;
let crtChromaticAberration = true;
let crtCurvature = true;
```
Applied in `applyCRT()` function — called at the end of `draw()`.

### Weather Effects (Line 4018)
Per-stage atmospheric particles:
```javascript
let weatherEnabled = true;
let weatherParticles = [];
```
Types: rain, snow, sandstorm, embers, spores — auto-selected based on stage theme.

### Screen Transitions (Line 4150)
Between-stage transitions:
```javascript
function startTransition(type, callback) { ... }  // 'fade', 'wipe', 'circle'
```

### Dynamic Lighting (Line 4231)
Additive blending light sources:
```javascript
function addLight(x, y, radius, color, intensity, life) { ... }
```
Auto-added on muzzle flash and explosions.

### Animated Water/Lava Tiles (Line 4266)
Enhanced tile rendering with flowing animation, ripples, foam, bubbles.

---

## 21. Title Screen, Demo, Credits, Tutorial, Particle Editor

**Lines 4274–5079**

### Animated Title Screen (Line 4278)
Canvas-rendered title with parallax stars, floating mountains, animated logo:
```javascript
function drawTitleScreen() { ... }
function drawTitleSoldier(x, y, facing) { ... }  // Marching silhouettes
```

### Demo Mode (Line 4456)
Auto-play AI that starts after 10 seconds idle on title:
```javascript
function startDemoMode() { ... }
function stopDemoMode() { ... }    // Any key returns to title
function updateDemoAI() { ... }    // Simple AI: move, shoot, jump
```

### Credits Roll (Line 4574)
Cinematic scrolling credits with live stats:
```javascript
function startCredits(callback) { ... }
function updateCredits() { ... }
function drawCredits() { ... }
```
Triggered after victory. Skip with Enter/Escape.

### Tutorial Stage (Line 4758)
Interactive stage 0 with 8 guided steps:
```javascript
function startTutorial() { ... }
function initTutorialLevel() { ... }   // Hand-crafted level
function updateTutorial() { ... }      // Step completion checking
function drawTutorialHUD() { ... }     // Instruction banner
```

### Particle Editor (Line 5026)
Admin panel integration for live particle tweaking:
```javascript
let particlePresets = {
  explosion: { count: 30, speed: 6, life: 35, color: '#ff4400', ... },
  death:     { ... },
  glow:      { ... },
  ring:      { ... },
  debris:    { ... },
};
function particleEditorTestEffect(presetName) { ... }
function updateParticleEditorUI() { ... }
```

---

## 22. Stage Themes

**Lines 5080–5187**

10 unique themes defining visual appearance:

```javascript
const THEMES = {
  1:  { name: 'JUNGLE',        cols: 140, sky: [...], mountains: [...], tileTop: '...', ... },
  2:  { name: 'RUINS',         cols: 155, ... },
  3:  { name: 'WATER BASE',    cols: 160, water: true, ... },
  4:  { name: 'SPACE STATION', cols: 170, space: true, ... },
  5:  { name: 'DESERT',        cols: 175, desert: true, ... },
  6:  { name: 'SNOW FORTRESS', cols: 180, snow: true, ... },
  7:  { name: 'FINAL ASSAULT', cols: 210, ... },
  8:  { name: 'VOLCANO',       cols: 190, lava: true, ... },
  9:  { name: 'DEEP SEA',      cols: 200, water: true, ... },
  10: { name: 'ALIEN HIVE',    cols: 220, ... },
};
```

Each theme provides:
- `name` — Display name
- `cols` — Level width in tiles
- `sky` — Array of `[color, stop]` for sky gradient
- `mountains` — 3 parallax layer colors
- `tileTop`, `tileFill`, `tileInner`, `tileLine` — Tile colors
- `stars`, `starBright` — Star field density/brightness
- Boolean flags: `water`, `space`, `desert`, `snow`, `lava`

**To add a new stage:**
1. Add a new entry to `THEMES` (next number key)
2. Increase `MAX_STAGE` constant (search for it)
3. Optionally add story cutscene in `STORY` data
4. Optionally add a weather type in `initWeather()`
5. Boss difficulty scales automatically via the formula in `spawnEnemies()`

---

## 23. Level Generation

**Lines 5202–5712**

Procedural level generation with seeded random:

```javascript
function generateLevel(stageNum) { ... }
```

The level is a 2D grid (`level.map[row][col]`):
- `0` = air
- `1` = solid tile
- `2` = ladder
- `3` = one-way platform (pass-through from below)
- `4` = water tile
- `5` = lava tile

Generation creates:
1. Ground surface
2. Underground solid fill (50 rows deep)
3. Gaps/pits  
4. Platforms (floating, staircase, bridge)
5. Ladders
6. Theme-specific terrain (desert dunes, water pools, lava pits, snow ice, etc.)
7. Tunnels and cave systems (underground)
8. Secret areas with treasures

The level uses a seeded PRNG (`seedRandom(stageNum * 12345)`) so the same stage always generates the same layout.

**To modify level generation:** Edit `generateLevel()`. The function runs top-to-bottom creating terrain features. Each section is relatively independent — you can add new feature types with new `for` loops.

---

## 24. Weapons, Vehicles & Characters

### Weapons (Line 5713)
```javascript
const WEAPONS = {
  RIFLE:     { fireRate: 8,  damage: 1,   bullets: 1, spread: 0,    type: 'normal' },
  SPREAD:    { fireRate: 12, damage: 1,   bullets: 5, spread: 0.3,  type: 'normal' },
  LASER:     { fireRate: 4,  damage: 2,   bullets: 1, spread: 0,    type: 'pierce' },
  MACHINE:   { fireRate: 3,  damage: 1,   bullets: 1, spread: 0.05, type: 'normal' },
  FLAME:     { fireRate: 2,  damage: 0.5, bullets: 1, spread: 0.15, type: 'flame' },
  ROCKET:    { fireRate: 30, damage: 5,   bullets: 1, spread: 0,    type: 'rocket' },
  HOMING:    { fireRate: 14, damage: 2,   bullets: 1, spread: 0,    type: 'homing' },
  WAVE:      { fireRate: 10, damage: 1.5, bullets: 2, spread: 0,    type: 'wave' },
  LIGHTNING: { fireRate: 12, damage: 1.5, bullets: 1, spread: 0,    type: 'lightning' },
  BOOMERANG: { fireRate: 20, damage: 2,   bullets: 1, spread: 0,    type: 'boomerang' },
  BLACKHOLE: { fireRate: 40, damage: 0.5, bullets: 1, spread: 0,    type: 'blackhole' },
  MINE:      { fireRate: 25, damage: 4,   bullets: 1, spread: 0,    type: 'mine' },
};
```

**To add a new weapon:**
1. Add entry to `WEAPONS` with all required fields
2. Add its name to the powerup type selection in `spawnPowerups()` (line ~6160)
3. Add bullet behavior in `updateBullets()` for your `type`
4. Add firing logic in `fireWeapon()` if the type needs special handling
5. Add a button in the admin panel "Weapons" section
6. Add a sound in `playSound()` if desired

### Vehicles (Line 5729)
```javascript
const VEHICLES = {
  HOVERBIKE: { hp: 8, speed: 6, jumpForce: -9, ... },
  TANK:      { hp: 20, speed: 2.5, jumpForce: -6, ... },
};
```

### Characters (Line 5746)
6 playable characters with unique abilities:

| Character | Ability | Start Weapon | Speed | Damage |
|-----------|---------|-------------|-------|--------|
| COMMANDO | None | RIFLE | 1.0× | 1.0× |
| SCOUT | Double Jump | MACHINE | 1.35× | 0.85× |
| HEAVY | 30% Death Resist | SPREAD | 0.8× | 1.25× |
| SNIPER | Wall Cling | LASER | 0.9× | 1.4× |
| GHOST | Invisible (ability) | MACHINE | 1.2× | 0.9× |
| DEMO | Explosions | ROCKET | 0.95× | 1.1× |

Characters unlock via gameplay achievements (beat certain stages, get kill counts, etc.).

**To add a new character:**
1. Add entry to `CHARACTERS` object with all stats
2. Add unlock condition in `checkCharacterUnlocks()`
3. It will automatically appear in the character select screen
4. Optionally add special ability handling in `updatePlayer()`

---

## 25. Player, Enemies & Entities

### Player Object (Line 5985)
Created by `createPlayer(startX, charId)`. Key properties:
```javascript
{
  x, y, w, h,              // Position and hitbox
  vx, vy,                  // Velocity
  onGround, facing,        // State
  aimX, aimY,              // 8-directional aiming (-1/0/1)
  weapon,                  // Current weapon object
  fireTimer,               // Cooldown counter
  lives, dead, respawnTimer, invincible,
  prone, shooting, onLadder, climbing,
  wallClimbing,            // Wall crawl state
  grapple, grappleCooldown,// Grapple hook
  inVehicle, vehicleType, vehicleHP,
  charId, speedMult, damageMult, ability,
}
```

### Enemy Types (Line 6039)
12 enemy types with unique behaviors:

| Type | Behavior |
|------|----------|
| SOLDIER | Basic shooter, patrols |
| RUNNER | Fast melee, charges player |
| TURRET | Stationary, rapid fire |
| SNIPER | Long range, slow fire |
| HEAVY | Tanky, slow, rapid fire |
| BOMBER | Throws grenades (arcing shots) |
| JUMPER | Leaps at player |
| SHIELD | Front shield, must flank |
| DRONE | Flying enemy |
| TELEPORTER | Teleports around |
| CLOAKER | Turns invisible |
| SHIELDPAIR | Paired shields |

### Boss
Spawned at the end of each stage. Has phases, special attack patterns, and high HP scaling with stage number.

---

## 26. Drawing Helpers & Tile Rendering

**Lines 6356–6762**

Procedural drawing functions for all entities:

```javascript
function drawPlayerFigure(p, cx, cy) { ... }    // Player body/limbs
function drawEnemyFigure(e) { ... }              // Enemy body based on etype
function drawBossFigure(e) { ... }               // Boss multi-phase rendering
function drawBullet(b) { ... }                   // Bullet types (normal, flame, rocket, etc.)
function drawExplosionEffect(x, y, r) { ... }    // Explosion circles
```

These are the fallback renderers when sprite sheets aren't available. The sprite system (next section) generates pre-rendered sheets for better performance.

---

## 27. Sprite Sheet System

**Lines 6763–7343**

Generates animated sprite sheets on initialization using offscreen canvases:

```javascript
// Sheet infrastructure
function createSheet(id, cols, rows, frameW, frameH, drawFn) { ... }
function getFrame(sheetId, col, row) { ... }

// Generators
function generatePlayerSheet() { ... }     // Run cycle, jump, prone, climb, shoot
function generateEnemySheet(type) { ... }  // Per enemy type
function generateBossSheet() { ... }       // Boss phases
function generateExplosionSheet() { ... }  // Explosion animation

// Init all
function initSpriteSheets() { ... }

// Draw wrappers
function drawSprite(sheetId, frame, x, y, flip) { ... }
```

Toggle between sprite and procedural rendering via admin panel "Sprites" section.

---

## 28. Background & Tile Drawing

### Background (Line 7344)
```javascript
function drawBackground(stageNum) { ... }
```
Renders: sky gradient → stars → 3-layer parallax mountains → ground strip. All based on the current theme's colors.

### Tile Drawing (Line 7502)
```javascript
function drawTiles() { ... }
```
Renders the tile map with:
- Solid tiles (with top highlight, inner shading, edge lines)
- Ladders (rungs)
- Platforms (thin top surface)
- Water tiles (animated waves, ripples, foam, bubbles)
- Lava tiles (animated glow, streaks, bubbles)

---

## 29. Collision System

**Lines 7728–7747**

```javascript
function aabb(a, b) { ... }          // Axis-aligned bounding box overlap
function tileAt(col, row) { ... }    // Get tile value at grid position
function isSolid(col, row) { ... }   // Is tile solid (1 or context-dependent)
```

Collision is grid-based for terrain (tile map lookup) and AABB for entity-entity interactions.

---

## 30. Game Init

**Lines 7748–7797**

```javascript
function initGame() {
  level = generateLevel(stage);        // Create level
  spawnEnemies(level, stage);          // Populate enemies
  spawnPowerups(level, stage);         // Place pickups
  spawnObjects(level, stage);          // Place destructibles
  spawnDecorations(level, stage);      // Visual decorations
  spawnVehicles(level, stage);         // Vehicle pickups
  spawnSecretExits(level, stage);      // Hidden exits
  spawnTreasures(level, stage);        // Treasure items
  player = createPlayer(60, selectedCharacter);
  // ... set initial position, camera, clear arrays
}
```

Called when starting a new game or advancing to the next stage.

---

## 31. Update Loop

**Lines 7798–9973**

The `update()` function is the largest in the codebase (~2,175 lines). It handles:

1. **Player physics** — gravity, movement, collision with tiles
2. **Player actions** — shooting, jumping, climbing, wall crawl, grapple hook, digging
3. **Enemy AI** — patrol, chase, shoot, special behaviors per type
4. **Boss AI** — multi-phase patterns, special attacks
5. **Bullet updates** — movement, collision with tiles/enemies, special types (homing, wave, etc.)
6. **Enemy bullet updates** — movement, player hit detection
7. **Powerup collection** — weapon pickups, life pickups
8. **Object interaction** — destructible crates, barrels
9. **Vehicle logic** — entering/exiting, vehicle combat
10. **Camera tracking** — smooth follow with dead zone
11. **Stage advancement** — reaching end of level triggers boss/next stage
12. **Combo system** — kill streak tracking
13. **Game mode updates** — time attack timer, endless wave checks, challenge objectives

### Key sub-functions called within update:
```javascript
function updatePlayer(p, left, right, up, down, jump, jumpKey, shoot, grapple) { ... }
function updateEnemiesAndBullets() { ... }
function updateParticles() { ... }
function updateCombo() { ... }
function registerKill(enemy) { ... }    // Score + combo + mode tracking
function killPlayer(target) { ... }     // Death logic + effects
function fireWeapon(p) { ... }          // Spawn bullets based on weapon type
```

---

## 32. Draw Function

**Lines 9974–11147**

The `draw()` function renders a complete frame (~1,173 lines):

```javascript
function draw() {
  ctx.clearRect(0, 0, GAME_W, GAME_H);
  ctx.save();
  ctx.scale(zoom, zoom);
  ctx.translate(shakeX, shakeY);

  drawBackground(stage);              // Sky, stars, mountains
  
  // Camera transform
  ctx.save();
  ctx.translate(-camX, -camY);
  
  drawTiles();                        // Terrain
  drawDecorations();                  // Visual decoration objects
  drawSecretExits();                  // Secret exit indicators
  drawTreasures();                    // Treasure items
  drawObjects();                      // Destructible objects
  drawVehicles();                     // Vehicle pickups
  drawPowerups();                     // Weapon/life pickups
  drawEnemies();                      // All enemies + bosses
  drawBullets();                      // Player bullets + enemy bullets
  drawPlayer(player);                 // Player 1
  if (player2) drawPlayer(player2);   // Player 2
  drawParticles();                    // Particle effects
  drawPopups();                       // Floating text
  drawGrapple();                      // Grapple hook line
  
  ctx.restore();                      // End camera transform

  drawHUD();                          // Score, lives, weapon, combo, timer
  drawLights();                       // Dynamic lighting (additive blend)
  drawWeatherOverlay();               // Weather particles
  drawTransitionOverlay();            // Screen transitions
  applyCRT();                         // CRT post-processing
  drawFlash();                        // Screen flash effect

  ctx.restore();                      // End zoom/shake transform
}
```

---

## 33. Overlay System

**Lines 11148–11166**

```javascript
function showOverlay(title, subtitle, action) { ... }
```

Dynamically rebuilds the `#overlay` div innerHTML with:
- Title/subtitle text
- Stats display (kills, combo, time) for gameover/victory
- Difficulty selector
- Game mode selector
- Start button
- Achievement/Leaderboard/Tutorial buttons
- Story mode toggle
- Share/Import buttons (gameover/victory only)
- Save game continue button

The overlay has a static version (initial HTML) and a dynamic version (`showOverlay()`) that's called when returning to menu.

---

## 34. Online Multiplayer

**Lines 11167–11803**

WebRTC peer-to-peer co-op. Host-authoritative architecture.

### Flow
1. **Host** creates an offer (SDP encoded as base64)
2. **Guest** pastes the offer, generates an answer (also base64)
3. **Host** pastes the answer to complete connection
4. **Data channel** carries: host→guest state snapshots, guest→host input data

### Key Functions
```javascript
function createRTCConnection() { ... }
function createOffer() { ... }        // Host creates
function acceptAnswer(code) { ... }    // Host completes
function joinWithOffer(code) { ... }   // Guest joins
function mpSend(data) { ... }          // Send over data channel
function mpTick() { ... }             // Per-frame sync (in gameLoop)
function getCompactState() { ... }    // Serialize game state for sync
function applyRemoteState(s) { ... }  // Deserialize on guest
```

---

## 35. Game Loop & Start Functions

**Lines 11804–11955**

### `gameLoop()` — See [Section 3](#3-the-game-loop)

### `startGameSession()` (Line 11916)
Entry point when player clicks START:
```javascript
function startGameSession() {
  // Validation (already playing? NG+ requirements?)
  // Stop demo mode if active
  // Apply difficulty
  // Reset run stats
  // Show character select screen
}
```

### `actuallyStartGame()` (called after character select)
```javascript
function actuallyStartGame() {
  // Initialize selected game mode
  // Call initGame()
  // Play stage intro cutscene (if story mode)
  // Set gameState = 'playing'
}
```

---

## 36. Character Select Screen

**Lines 11956–12213**

Grid display of all characters with stats, abilities, and unlock status:
```javascript
function showCharSelect() { ... }
function drawCharSelect() { ... }     // Canvas rendering
function handleCharSelectKey(e) { ... }
```

---

## 37. Admin Panel

**Lines 12264–12918**

### Opening/Closing
Toggle with backtick (`` ` ``) key or the floating gear button.

### Slider Bindings (Line 12302)
Maps admin panel `<input>` elements to game variables:
```javascript
document.getElementById('s_gravity').oninput = function() { gravity = parseFloat(this.value); };
// ... many more
```

### Level Editor (Line 12525)
Mouse-based tile painting:
```javascript
// Brush types: solid(1), air(0), ladder(2), platform(3), water(4), lava(5)
// Click/drag on canvas to paint tiles
```

### Minimap (Line 12739)
Real-time level overview drawn on a small canvas, visible when admin panel is open.

### Boot Code (Line ~12850)
The final lines wire up event listeners and start the game loop:
```javascript
// Wire start button
startBtn.addEventListener('click', startGameSession);

// Sync settings from localStorage
// Initialize HUD, achievement counters, particle editor

gameLoop();  // GO!
```

---

## 38. How-To Recipes

### Add a New Enemy Type

1. **Define the type** in `ENEMY_TYPES` (~line 6044):
   ```javascript
   MYENEMY: { w: 20, h: 28, hp: 3, speed: 1.5, score: 200, color: '#ff00ff',
              fireRate: 60, shootRange: 300, etype: 'myenemy' },
   ```

2. **Add AI behavior** in `updateEnemiesAndBullets()` (~line 7800+). Find the enemy update section and add a case for your `etype`.

3. **Add rendering** in `drawEnemyFigure()` (~line 6400+). Add a case for your `etype` to draw the enemy.

4. **Add to spawn table** in `spawnEnemies()` (~line 6060). Add your type to the `typeRoll` probability distribution.

5. **Optionally** generate a sprite sheet in `generateEnemySheet()`.

---

### Add a New Stage Theme

1. Add entry to `THEMES` (~line 5080):
   ```javascript
   11: {
     name: 'MY THEME', cols: 200,
     sky: [['#112233', 0], ['#223344', 0.5], ['#001122', 1]],
     mountains: ['#1a2a3a', '#152530', '#101a25'],
     tileTop: '#44aa88', tileFill: '#336655',
     tileInner: '#225544', tileLine: '#114433',
     stars: 50, starBright: 0.6,
   },
   ```

2. Update `MAX_STAGE` (search for it in settings).

3. **Optionally** add terrain generation features in `generateLevel()` using a theme flag (like `desert`, `snow`, etc.).

4. **Optionally** add a weather type in `initWeather()` for the new theme.

5. **Optionally** add story cutscene data in `STORY`.

---

### Add a New Weapon

1. Add to `WEAPONS` (~line 5713):
   ```javascript
   MYGUN: { name: 'MYGUN', fireRate: 15, damage: 3, bullets: 1,
            spread: 0, color: '#00ff00', size: 4, type: 'mytype' },
   ```

2. Add firing behavior in `fireWeapon()` if your type needs special bullet creation.

3. Add bullet behavior in bullet update section for your `type` (movement pattern, collision, effects).

4. Add bullet rendering in `drawBullet()` for your type.

5. Add to powerup spawn table in `spawnPowerups()`.

6. Add admin button in the Weapons admin section.

---

### Add a New Visual Effect

1. **Variables** — Add state variables near the visual effects section (~line 3934).

2. **Update function** — Create `updateMyEffect()` and call it from `gameLoop()` update chain.

3. **Draw function** — Create `drawMyEffect()` and call it from `draw()` at the appropriate layer.

4. **Admin toggle** — Add a checkbox in the Visual Effects admin section.

---

### Add a New Achievement

1. Add to `ACHIEVEMENT_DEFS` (~line 3573):
   ```javascript
   my_achievement: { name: 'My Achievement', desc: 'Do something cool', icon: '⭐' },
   ```

2. Call `unlockAchievement('my_achievement')` wherever the condition is met.

---

### Add a New Sound Effect

In `playSound()` (~line 1613), add a new case:
```javascript
case 'my_sound': {
  const osc = audioCtx.createOscillator();
  const gain = audioCtx.createGain();
  osc.connect(gain);
  gain.connect(audioCtx.destination);
  osc.type = 'square';
  osc.frequency.setValueAtTime(440, audioCtx.currentTime);
  osc.frequency.exponentialRampToValueAtTime(110, audioCtx.currentTime + 0.3);
  gain.gain.setValueAtTime(sfxVolume * 0.3, audioCtx.currentTime);
  gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.3);
  osc.start(audioCtx.currentTime);
  osc.stop(audioCtx.currentTime + 0.3);
  break;
}
```

---

### Modify the Tile Map

Tile values in `level.map[row][col]`:
| Value | Meaning |
|-------|---------|
| 0 | Air |
| 1 | Solid ground/wall |
| 2 | Ladder |
| 3 | One-way platform |
| 4 | Water |
| 5 | Lava |

Change tiles at runtime:
```javascript
level.map[row][col] = 1;  // Place solid tile
level.map[row][col] = 0;  // Remove tile (make air)
```

The level editor in the admin panel does exactly this with mouse clicks.

---

### Add Admin Panel Controls

In the HTML admin panel section (~line 1147), add inside any `<div class="admin-body">`:
```html
<div class="admin-row">
  <label>My Setting</label>
  <input type="range" min="0" max="100" value="50"
         oninput="mySetting=+this.value;this.nextElementSibling.textContent=this.value">
  <span class="val">50</span>
</div>
```

For checkboxes:
```html
<div class="admin-row">
  <label>My Toggle</label>
  <input type="checkbox" onchange="myToggle=this.checked">
</div>
```

---

## Quick Reference: Key Functions

| Function | Line | Purpose |
|----------|------|---------|
| `gameLoop()` | ~11804 | Main loop (60fps) |
| `update()` | ~7798 | All game logic |
| `draw()` | ~9974 | Full frame render |
| `initGame()` | ~7748 | Initialize stage |
| `generateLevel(n)` | ~5202 | Procedural level gen |
| `createPlayer(x, id)` | ~5989 | Create player object |
| `spawnEnemies(lvl, n)` | ~6060 | Populate enemies |
| `fireWeapon(p)` | search | Fire player weapon |
| `registerKill(e)` | search | Enemy killed — score/combo |
| `killPlayer(p)` | ~9877 | Player death logic |
| `startGameSession()` | ~11916 | Start button handler |
| `showOverlay(t, s, a)` | ~11148 | Show menu overlay |
| `playSound(name)` | ~1613 | Play sound effect |
| `startMusic(track)` | ~1856 | Start music track |
| `playCutscene(key, cb)` | ~3303 | Play story cutscene |
| `unlockAchievement(id)` | ~3646 | Unlock achievement |
| `applyDifficulty()` | search | Apply difficulty preset |
| `startTransition(type, cb)` | ~4150 | Screen transition |
| `addLight(x, y, r, c, i, l)` | ~4231 | Dynamic light |
| `spawnParticles(...)` | ~5849 | Spawn particles |
| `spawnGlowParticles(...)` | ~5867 | Spawn glow particles |
| `spawnRing(...)` | ~5885 | Spawn ring effect |
| `spawnDebris(...)` | ~5902 | Spawn debris |
| `triggerShake(i, d)` | ~3864 | Screen shake |
| `triggerFlash(c, a)` | ~3864 | Screen flash |
| `triggerFreeze(n)` | ~3864 | Freeze frames |
| `spawnPopup(x, y, t, c, s)` | search | Floating text |
| `drawTitleScreen()` | ~4300 | Title screen render |
| `startCredits(cb)` | ~4596 | Start credits roll |
| `startTutorial()` | ~4751 | Start tutorial |
| `startDemoMode()` | ~4460 | Start demo auto-play |

---

## Tips for Working with the Codebase

1. **Use your editor's Ctrl+G (Go to Line)** — Line numbers are your primary navigation tool in a 12,900-line file.

2. **Search for section comments** — All major sections have `// ============` or `// ---` headers.

3. **The admin panel is your best friend** — Toggle god mode, spawn enemies, give weapons, skip stages, tweak physics — all without code changes.

4. **Test in the browser console** — All game variables are global. You can type `player.lives = 99` or `stage = 7` directly.

5. **The game is deterministic per stage** — Same stage number always generates the same level (seeded PRNG). This makes debugging reproducible.

6. **Particle effects are visual-only** — They don't affect gameplay. Safe to add/modify without breaking anything.

7. **The overlay is DOM, not canvas** — Title/menu screens use HTML elements. In-game everything is canvas-rendered.

8. **localStorage keys** all start with `contraForce_` — Easy to find and clear in DevTools.

---

*Last updated: February 2026*
