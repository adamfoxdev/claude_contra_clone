# CONTRA FORCE — HTML5

A massive single-file HTML5 Canvas arcade shooter with full physics, 12 weapons + fusion system, 12 enemy types + mini-bosses, environmental hazards, 6 playable characters, 5 game modes, co-op multiplayer, achievements, leaderboards, procedural music, visual effects, speedrun timer, accessibility options, and classic arcade-style gameplay.

## Overview

CONTRA FORCE is a faithful HTML5 recreation of the classic arcade action game, featuring:
- **Pixel-perfect graphics** with retro arcade aesthetics, parallax backgrounds, and CRT shader
- **Animated sprite sheets** — procedurally generated at init, replacing per-frame drawing with drawImage
- **Physics-based gameplay** with gravity, momentum, and collision detection
- **6 Playable characters** — Commando, Shadow, Tank, Medic, Scout, Ghost — each with unique stats and abilities
- **5 Game modes** — Campaign, Endless/Survival, Boss Rush, Time Attack, New Game+, and Challenge Missions
- **2-Player Local Co-op** — fight side-by-side on the same keyboard
- **Online Multiplayer** — peer-to-peer WebRTC co-op with no server required
- **Gamepad / controller support** — up to 2 gamepads, standard mapping, vibration feedback
- **Procedural chip-tune music** — 5 dynamic tracks (title, action, boss, gameover, victory) with Web Audio synthesis
- **27 sound effects** — weapon-specific SFX, enemy sounds, jingles, and UI feedback
- **25 Achievements** with toast notifications, progress tracking, and a dedicated panel
- **Leaderboard system** with persistent top-20 high scores and shareable score codes
- **5 Difficulty presets** — Easy, Normal, Hard, Nightmare, and Custom
- **10 Stages + 2 Secret Stages** with unique themes, boss fights, and progressive difficulty
- **12 Enemy types** — Soldier, Runner, Turret, Sniper, Heavy, Bomber, Jumper, Shield, Drone, Teleporter, Cloaker, Shield Pair
- **5 Mini-boss types** — Charger, Shielded, Artillery, Splitter, Jump Stomper — with unique AI patterns
- **5 Environmental hazards** — Moving Platforms, Crushers, Spike Traps, Conveyor Belts, Laser Grids
- **12 Weapons** — Rifle, Spread, Laser, Machine Gun, Flame, Rocket, Homing, Wave, Lightning, Boomerang, Black Hole, Mine
- **Weapon Fusion system** — combine 2 weapon pickups for 12 powerful hybrid weapons (e.g., Prism Burst, Smart Missile, Singularity)
- **Vehicles** — Hoverbike and Tank with unique firepower and armor
- **Advanced boss AI** — up to 5 attack phases with stage-scaling difficulty
- **Combo kill system** with visual popups and score multipliers
- **Speedrun timer with splits** — per-stage PB tracking with ahead/behind delta display
- **Visual effects suite** — CRT shader, dynamic weather, screen transitions, dynamic lighting, animated water/lava
- **Accessibility options** — color-blind filters, aim assist, difficulty modifiers, high-contrast HUD
- **Interactive tutorial** — guided stage 0 teaching all mechanics
- **Animated title screen** with parallax and demo auto-play mode
- **Credits roll** after victory with full scrolling credits sequence
- **Save/Load system** — quick save (F5), quick load (F9), file export/import
- **Level editor file I/O** — download and upload custom levels as JSON
- **Destructible terrain** — dig and blast through tiles, 50 underground rows
- **Grapple hook** for traversing gaps and climbing
- **Treasure system** — Gold, Gems, Ancient Relics, Shield, Speed Boost pickups
- **Particle editor** — real-time tuning of all particle effects in the admin panel
- **Admin panel** with 20+ sections for physics tweaking, gameplay tuning, and debug tools
- **Enhanced touch controls** with adjustable opacity, swappable sides, and vibration
- **Story mode with cutscenes** — narrative prologue, per-stage briefings, and epic epilogue
- **Desktop packaging** — Electron wrapper for Steam, Itch.io, and standalone distribution

## How to Play

### Starting the Game
1. Open `contra_opus4_6.html` in your web browser
2. Choose a **difficulty** on the title screen
3. Press **ENTER** or tap **START GAME** to begin

### Controls — Player 1

| Key | Action |
|-----|--------|
| **Arrow Keys / WASD** | Move & Aim |
| **SPACE / Z** | Shoot |
| **X** | Jump |
| **C** | Fire Grapple Hook |
| **Arrow Up** on ladder | Climb up |
| **Arrow Down** on ladder | Climb down |
| **Arrow Up** against wall | Wall crawl |
| **+/-** | Zoom in/out |
| **ENTER** | Start game |

### Controls — Player 2 (Co-op)

| Key | Action |
|-----|--------|
| **I / J / K / L** | Up / Left / Down / Right |
| **U** | Shoot |
| **O** | Jump |
| **P** | Fire Grapple Hook |
| **Y** | Aim up (alternate) |

> Enable co-op in the **Admin Panel** → toggle "2-Player Co-op".

### System Keys

| Key | Action |
|-----|--------|
| **Tab** | Toggle Achievements panel |
| **L** | Toggle Leaderboard panel |
| **F5** | Quick Save game state |
| **F9** | Quick Load game state |
| **Escape** | Close open panels |
| **`** (backtick) | Toggle Admin Panel |

### Music & Sound Controls

On-screen buttons in the top-right corner:
- **🎵** — Toggle background music on/off
- **🔊** — Toggle sound effects on/off

Volume sliders available in the Admin Panel → Audio section.

### Gamepad / Controller

Connect a standard gamepad (Xbox, PlayStation, etc.) and press any button. Supports up to 2 gamepads for co-op.

| Button | Action |
|--------|--------|
| **D-Pad / Left Stick** | Move & Aim |
| **A (Cross)** | Jump |
| **B / X (Circle/Square)** | Shoot |
| **Y (Triangle) / LB / RB** | Grapple Hook |
| **LT / RT** | Shoot (alt) |
| **Start** | Start Game / Advance Cutscene |

- **Gamepad 1** → Player 1, **Gamepad 2** → Player 2 (co-op)
- Dead zone and vibration settings available in the Admin Panel → Gamepad section
- Rumble feedback on player death

### Mobile Touch Controls

On mobile devices and tablets, virtual touch controls automatically appear:

**D-Pad (Left Side)** — ↑ ↓ ← → for movement & aim

**Action Buttons (Right Side):**
- **JUMP** (Green) — Jump / climb
- **FIRE** (Red) — Shoot
- **HOOK** (Blue) — Grapple hook

**Button Size Adjustment:** Tap S / M / L in the top-right to resize controls. Your preference is saved to localStorage.

**Customization (Admin Panel → Touch Controls):**
- **Opacity** — Adjust button transparency (20%–100%)
- **Swap Sides** — Move D-pad to right, action buttons to left
- **Vibration** — Haptic feedback on button press (where supported)

Touch controls auto-enable on mobile, tablets, and narrow viewports (≤ 768px).

## Difficulty System

Choose your challenge from the title screen or game-over screen. Difficulty affects lives, enemy spawn rate, enemy HP, bullet speed, fire rate, boss HP, power-up frequency, player speed, score multiplier, and respawn invincibility.

| Preset | Lives | Enemy Spawn | Enemy HP | Boss HP | Power-ups | Score Mult |
|--------|-------|-------------|----------|---------|-----------|------------|
| **Easy** | 5 | 0.7× | 0.7× | 0.7× | 1.5× | 0.5× |
| **Normal** | 3 | 1.0× | 1.0× | 1.0× | 1.0× | 1.0× |
| **Hard** | 2 | 1.3× | 1.5× | 1.5× | 0.6× | 1.5× |
| **Nightmare** | 1 | 1.6× | 2.0× | 2.0× | 0.4× | 2.5× |
| **Custom** | — | Uses Admin Panel overrides | | | | |

Your difficulty preference is saved and restored between sessions.

## Achievement System

25 achievements across 8 categories track your gameplay mastery:

| Category | Examples |
|----------|----------|
| **Combat** | First Blood, Hundred Kills, Thousand Kills |
| **Boss** | Boss Slayer, Boss Rush (all 10 bosses) |
| **Progression** | Complete stages, beat the game |
| **Weapons** | Collect all weapon types |
| **Exploration** | Find treasure, discover secrets |
| **Skill** | No-death run, high combos |
| **Score** | Reach score milestones |
| **Speed** | Speedrun stages under time limits |

- Press **Tab** to open the Achievements panel (or tap the 🏆 HUD icon)
- Toast notifications appear when you unlock achievements
- Progress bars show incremental advancement
- All achievements persist in localStorage

## Leaderboard

- Top 20 scores are tracked locally with stage reached, kills, time, difficulty, and date
- Press **L** to view the leaderboard (or tap the 📊 HUD icon)
- Scores are auto-submitted on game over and victory
- Difficulty is recorded per entry — higher difficulties reward more score via the multiplier

## Combo System

Chain enemy kills within a short window to build combos. Visual popups display your current combo multiplier. Your highest combo is tracked per run and feeds into achievements.

## Music & Sound System

Fully procedural chip-tune audio powered by the Web Audio API — no external audio files required.

### Background Music (5 Tracks)
| Track | Tempo | Context |
|-------|-------|---------|
| **Title** | 140 BPM | Title screen / overlay |
| **Action** | 160 BPM | Normal gameplay |
| **Boss** | 180 BPM | Boss encounters |
| **Game Over** | 90 BPM | Death screen |
| **Victory** | 140 BPM | Victory screen |

Music automatically switches based on game state and boss proximity.

### Sound Effects (27 Types)
- **Weapons**: Shoot, Spread, Laser, Rocket, Flame, Homing, Machine, Wave — each weapon has a unique sound
- **Gameplay**: Jump, Hit, Die, Explosion, Power-up, Dig, Coin, Treasure
- **Events**: Boss roar, Stage Clear jingle, Victory fanfare, Game Over dirge, Menu Select
- **Enemies**: Enemy shoot, Wall climb, Grapple fire/latch/pull

Volume and enable/disable controls in the Admin Panel → Audio section. Preferences persist in localStorage.

## Save / Load System

Save and restore your full game state at any time:

| Method | How |
|--------|-----|
| **Quick Save** | Press **F5** during gameplay |
| **Quick Load** | Press **F9** anytime |
| **File Export** | Admin Panel → Save/Load → Export File (downloads `.json`) |
| **File Import** | Admin Panel → Save/Load → Import File (upload `.json`) |

Saves include: score, stage, difficulty, both players (position, weapon, lives), level map, enemies, power-ups, camera position, and run statistics.

## Shareable Leaderboard

Share your high scores with friends using encoded score codes:
- **Share**: Click 📤 SHARE SCORE on the game-over/victory screen — copies a base64 code
- **Import**: Click 📥 IMPORT SCORE — paste a friend's code to add their score to your leaderboard
- Imported scores are marked with a 📥 flag

## Story Mode & Cutscenes

Contra Force features a full narrative story mode with canvas-rendered cutscenes between stages.

### Enabling Story Mode
- Toggle **📖 STORY MODE** on the title screen (below the START button)
- Or toggle in **Admin Panel → Story Mode**
- Preference is saved to localStorage

### Story Flow
| Cutscene | When It Plays |
|----------|---------------|
| **Prologue** | Before Stage 1 — sets up the alien invasion and introduces Lance & Bill |
| **Stage 1–10 Briefings** | Before each stage — mission intel, character dialogue |
| **Epilogue** | After defeating the final boss — escape sequence and victory narrative |

### Cutscene Controls
| Input | Action |
|-------|--------|
| **Click / ENTER / SPACE** | Advance text (first press shows full line, second advances to next) |
| **ESC** | Skip entire cutscene |

### Characters
- **📡 COMMAND** — Mission control, provides intel and objectives
- **🔴 LANCE** — Hot-headed commando, aggressive and fearless
- **🔵 BILL** — Cool-headed strategist, calm under pressure

Cutscenes feature:
- Typewriter text effect with word-wrapping
- Character portraits with colored name labels
- Atmospheric gradient backgrounds matching each stage theme
- Animated star field particles
- Blinking advance indicator
- Scene counter progress display

## Online Multiplayer (WebRTC)

Play co-op with a friend online — no server required! Uses peer-to-peer WebRTC connections with manual signaling.

### How to Connect

1. **Host** clicks 🌐 ONLINE MULTIPLAYER → HOST GAME
2. Host copies the generated invite code and sends it to friend
3. **Guest** clicks 🌐 ONLINE MULTIPLAYER → JOIN GAME, pastes the invite code
4. Guest copies their answer code and sends it back to host
5. Host pastes the answer code → connection established!

### Architecture
- **Host-authoritative**: The host runs the game simulation and sends state snapshots to the guest
- **Guest sends inputs**: The guest's keyboard inputs are forwarded to the host, which processes them for Player 2
- **RTCDataChannel**: Low-latency data channel for input and state sync
- **STUN servers**: Google's public STUN servers for NAT traversal
- **No server needed**: All signaling is done via copy/paste codes (base64-encoded SDP)

### During Gameplay
- The host sees a green 🌐 ONLINE HOST badge; the guest sees a blue 🌐 ONLINE GUEST badge
- Latency (ping) is displayed in the HUD
- The host controls game start/restart — the guest waits for the host
- Co-op mode is automatically enabled when connected

## Level Editor File I/O

Export and import custom level designs:
- **Download Level**: Admin Panel → Level File I/O → saves current stage map, enemies, power-ups, and objects as JSON
- **Upload Level**: Load a previously saved level file to continue editing
- **Copy/Paste JSON**: Quick copy level data to clipboard or paste from clipboard

## Game Modes

| Mode | Description |
|------|-------------|
| **Campaign** | Classic 10-stage progression with boss fights and story cutscenes |
| **Endless / Survival** | Infinite waves of escalating enemies — survive as long as possible |
| **Boss Rush** | Fight all 10 bosses back-to-back with minimal downtime |
| **Time Attack** | Speedrun through stages — per-stage best times tracked |
| **New Game+** | Replay with escalating difficulty multipliers (NG+1, NG+2, ...) |
| **Challenge Missions** | Special objectives: no-hit boss, speed demon, weapon master, pacifist, etc. |

## Characters

6 playable characters with unique stats, abilities, and starting weapons. Select before each game on the character select screen.

| Character | Specialty | Passive Ability |
|-----------|-----------|----------------|
| **Commando** | Balanced all-rounder | Standard stats |
| **Shadow** | Speed & stealth | Faster movement, shorter invincibility |
| **Tank** | High durability | Extra lives, slower speed |
| **Medic** | Support & sustain | Periodic health recovery |
| **Scout** | Agility & exploration | Higher jump, faster climb |
| **Ghost** | Unlockable (beat the game) | Enhanced dodge, unique weapon |

## Weapons & Fusion

### Base Weapons (12)

Collected from floating power-ups throughout stages:

| Weapon | Type | Description |
|--------|------|-------------|
| Rifle | Normal | Default weapon |
| Spread | Normal | Multi-bullet fan |
| Laser | Pierce | Passes through enemies |
| Machine Gun | Normal | Rapid fire with spread |
| Flamethrower | Flame | Short-range fire stream |
| Rocket | Rocket | Slow, high-damage explosive |
| Homing | Homing | Seeks nearest enemy |
| Wave | Wave | Sine-wave projectile |
| Lightning | Lightning | Chain-lightning effect |
| Boomerang | Boomerang | Returns to player |
| Black Hole | Black Hole | Gravity well trap |
| Mine | Mine | Deployable proximity mine |

### Weapon Fusion System

Pick up two different weapons within 5 seconds to fuse them into a powerful hybrid weapon! 12 fusion recipes:

| Combination | Fusion Weapon | Effect |
|-------------|---------------|--------|
| Spread + Laser | **Prism Burst** | Piercing fan of lasers |
| Spread + Machine | **Storm** | Rapid triple spray |
| Laser + Flame | **Plasma Beam** | Continuous plasma pierce |
| Machine + Rocket | **Gatling RPG** | Rapid rockets |
| Flame + Spread | **Inferno Fan** | Flame in all directions |
| Homing + Lightning | **Thunderstorm** | Homing lightning bolts |
| Rocket + Homing | **Smart Missile** | Heavy homing rocket |
| Wave + Boomerang | **Vortex** | Returning wave projectiles |
| Black Hole + Mine | **Singularity** | Massive gravity trap |
| Lightning + Wave | **Tesla Wave** | Shocking sine wave |
| Machine + Flame | **Napalm Spray** | Rapid fire napalm |
| Rocket + Spread | **Cluster Bomb** | Explosive spread rockets |

Fusion indicator bar appears in the HUD showing the countdown window. Fused weapons are marked with ⚡.

## Vehicles

| Vehicle | Stats | Found On |
|---------|-------|----------|
| **Hoverbike** | Speed boost, forward cannon | Stages 3, 7 |
| **Tank** | Heavy armor, powerful turret | Stages 5, 9 |

Vehicles absorb damage before the player is hurt. Destroyed vehicles eject the player safely.

## Gameplay Elements

- **Lives**: Shown in the HUD (♥ symbols). Varies by difficulty
- **Score**: Points from kills, terrain destruction, treasures — scaled by difficulty multiplier
- **Stages**: 10 unique themed environments + 2 secret stages, each with a boss:
  1. Jungle — 2. Ruins — 3. Water Base — 4. Space Station — 5. Desert — 6. Snow Fortress — 7. Final Assault — 8. Volcano — 9. Cyber Core — 10. Alien Hive — 11. Secret Paradise — 12. Secret Stage
- **Secret Stages**: Hidden exits on stages 3 → 11 and 7 → 12
- **Zoom**: Adjust from 40% to 250% with +/- keys
- **Destructible tiles**: Shoot or dig through breakable terrain to uncover paths and treasure
- **Underground**: 50 rows of underground terrain to excavate below each stage
- **Treasures**: Gold Bars (+500), Gems (+1000), Ancient Relics (+2500), Shield, Speed Boost, Life, Weapon pickups

## Mini-Bosses

1–2 mini-bosses spawn per stage (2 from stage 5 onward) at ~30% and ~60% of the level length. Each has a unique AI pattern:

| Mini-Boss | Pattern | Behavior |
|-----------|---------|----------|
| **Charger** | Charge | Pauses, then rushes at the player at high speed |
| **Shielded** | Shield | Front-facing energy shield blocks 80% damage — attack from behind |
| **Artillery** | Lob | Fires arcing projectiles that follow gravity |
| **Splitter** | Split | On death, splits into 2 smaller fast enemies |
| **Jump Stomper** | Stomp | Leaps high and ground-pounds, damaging nearby players |

Mini-bosses display a health bar and "MINI-BOSS" label. They scale with difficulty settings.

## Environmental Hazards

Procedrually placed throughout levels with density scaling per stage:

| Hazard | Behavior |
|--------|----------|
| **Moving Platforms** | Oscillate between two points. Player rides on top and moves with the platform |
| **Crushers** | Wait → crush downward → hold → retract cycle. Instant kill on contact |
| **Spike Traps** | Toggle on/off on a timer. Instant kill when active |
| **Conveyor Belts** | Push the player left or right when standing on them. Animated arrows show direction |
| **Laser Grids** | Vertical beams that cycle on/off. Active beams kill on contact. Visual pulse effect |

## Speedrun Timer

Enable in the Admin Panel → Speedrun Timer section.

- **Live timer** displayed top-center during gameplay
- **Per-stage splits** recorded on each boss kill
- **Personal Best tracking** saved to localStorage
- **Delta display** shows green (ahead) or red (behind) PB per split
- **Split history** visible on the right side (last 5 splits)
- Works with all game modes but pairs especially well with Time Attack

## Accessibility Options

All options available in the Admin Panel → Accessibility section:

| Option | Description |
|--------|-------------|
| **Color-Blind Filters** | Protanopia (red), Deuteranopia (green), Tritanopia (blue) — pixel-level LMS simulation |
| **Aim Assist** | Auto-snaps aim toward the nearest enemy with configurable strength (10–80%). Visual crosshair on target |
| **Infinite Lives** | Never run out of lives |
| **Enemy Speed** | Scale enemy movement speed (20–150%) |
| **Enemy Bullet Speed** | Scale enemy bullet speed (20–150%) — "bullet time" effect |
| **High Contrast HUD** | Darker background behind HUD elements for readability |
| **Large HUD Text** | Increases HUD font size |
| **Reduce Screen Shake** | Cuts shake intensity to 30% |

## Visual Effects

| Effect | Description |
|--------|-------------|
| **CRT Shader** | Scanlines, chromatic aberration, and screen curvature — full retro look |
| **Dynamic Weather** | Rain, snow, sandstorms, volcanic ash — varies per stage theme |
| **Screen Transitions** | Wipe, fade, and circle-iris transitions between stages |
| **Dynamic Lighting** | Muzzle flashes, explosions, and environmental glow create real-time light sources |
| **Animated Water/Lava** | Tiles animate with wave motion and color cycling |

All effects are toggleable in the Admin Panel → Visual Effects section.

## Tutorial

An interactive tutorial (Stage 0) teaches all core mechanics:
- Movement, jumping, shooting, aiming
- Ladder climbing, prone position
- Grapple hook usage
- Weapon pickups
- Underground digging

Automatically offered on first play or accessible from the admin panel.

## Technical Features

### Architecture
- **Single-file implementation**: ~13,800 lines of HTML, CSS, and JavaScript in one portable HTML file
- **Canvas-based rendering**: Hardware-accelerated 2D graphics with pixel-perfect rendering
- **Custom physics engine**: Gravity, collision detection, wall climbing, ladder mechanics
- **Seeded random generation**: Reproducible level layouts per stage number
- **Post-processing pipeline**: Color-blind filters → CRT shader applied per frame

### Physics System
- Gravity simulation with terminal velocity
- Precise AABB collision detection for tiles, entities, and projectiles
- Ladder climbing, wall crawling, and grapple traversal
- Platform pass-through (one-way platforms)
- Moving platform ride-along (player moves with platform)
- Conveyor belt physics (push force on grounded player)

### Rendering
- **Sprite sheet system**: All entities (players, 12 enemy types, mini-bosses, boss, explosions) have procedurally generated sprite sheets created at startup on offscreen canvases
- **Multi-frame animations**: Player idle (4f), run (4f), jump (2f), shoot (6f), prone (4f), climb (4f), wall climb (4f). Enemies: idle (2f), walk (4f), shoot (2f), death (4f). Boss: idle (3f), attack (3f), damaged (2f), death (4f)
- **drawImage rendering**: Runtime uses `ctx.drawImage()` from sprite sheets instead of per-frame `fillRect` calls — faster and enables proper animation
- **Procedural fallback**: Legacy drawing functions preserved; toggle in Admin Panel → Animated Sprites
- **Post-processing**: CRT scanlines, chromatic aberration, color-blind pixel filters

### Game Systems
- **HUD**: Lives, score, weapon (with fusion ⚡ indicator), stage/theme, difficulty badge, zoom, grapple cooldown, speedrun timer, fusion countdown, achievement counter, leaderboard button
- **Enemy AI**: 12 enemy types with patrol, shooting, jumping, flying, teleporting, cloaking, and shield behaviors
- **Mini-boss AI**: 5 patterns — charge, shield, artillery lob, split-on-death, jump-stomp
- **Boss fights**: Unique boss per stage with up to 5 phased attack patterns, stage-scaling difficulty, charge attacks, ground waves, and aerial bombardment
- **Environmental hazards**: Moving platforms, crushers, spike traps, conveyor belts, laser grids — all with physics interactions
- **Weapon fusion**: 12 recipes combining weapon pairs into hybrid weapons with enhanced effects
- **Co-op management**: Independent lives, shared score, co-op game-over logic
- **Particle system**: Explosions, blood, sparks, glow particles, debris, screen shake, hit freeze, screen flash, ring effects, and a real-time particle editor
- **State management**: Title, character select, playing, demo, cutscene, paused, game over, victory, and credits states
- **Speedrun tracking**: Per-stage splits, PB storage, delta display

## Admin Panel

Access with the **`** (backtick) key to open an in-game debug/tuning panel.

### Sections
- **Physics**: Gravity, jump force, movement speed, climb speed, max fall speed
- **Combat**: Bullet speed, enemy bullet speed, lives, god mode, hitbox display, pause, co-op toggle
- **Achievements & Leaderboard**: View/reset achievements, view/clear leaderboard
- **Keybinds**: Rebind controls for both players with multi-key support
- **Weapons**: Quick-select any of the 12 weapons
- **Game**: Stage select (1–12), score control, kill all enemies, respawn, clear bullets, regenerate level
- **Spawn**: Click-to-spawn enemies and objects on the game canvas
- **Level Editor**: Tile painting (solid/ladder/water/treasure/erase), brush size, flatten, clear, export/import JSON, minimap
- **Save/Load**: Quick save, load, delete, export/import save files
- **Level File I/O**: Download/upload levels, copy/paste JSON
- **Audio**: Music volume, SFX volume, enable/disable toggles
- **Touch Controls**: Opacity, button size (S/M/L), vibration, swap sides
- **Online Multiplayer**: Host/join WebRTC co-op, connection status
- **Gamepad / Controller**: Dead zone, vibration toggle, detection status
- **Animated Sprites**: Toggle sprite-sheet rendering vs procedural, preview all sheets
- **Story Mode**: Toggle narrative cutscenes, preview prologue/epilogue
- **Characters**: View/unlock all 6 characters, reset unlocks
- **Vehicles & Secrets**: Spawn vehicles, view secret stage info
- **Visual Effects**: CRT shader (scanlines, aberration, curvature), weather, dynamic lighting, animated tiles
- **Particle Editor** 💥: Real-time tuning of explosion, death, glow, ring, debris particle presets
- **Accessibility** ♿: Color-blind filters, aim assist, infinite lives, enemy speed/bullet speed modifiers, HUD options, reduced shake
- **Speedrun Timer** ⏱: Enable/disable timer, clear PB splits, view personal best history
- **Weapon Fusion** ⚔: View all 12 fusion recipes and combinations

## Browser Compatibility

| Browser | Minimum Version |
|---------|----------------|
| Chrome | 90+ |
| Firefox | 88+ |
| Safari | 14+ |
| Edge | 90+ |

Requires HTML5 Canvas, Web Audio API, ES6+, and `localStorage`.

## Performance Notes

- `image-rendering: pixelated` for authentic retro look
- Efficient per-frame rendering with camera culling
- Consistent 60 FPS target on modern hardware
- Slow-motion and hit-freeze effects for combat impact

## Development

### Modifying Physics
Adjust through the Admin Panel at runtime, or edit the JavaScript configuration section near the top of the file (`gravity`, `playerSpeed`, `jumpForce`, etc.).

### Adding Enemies
Define new types in the `ENEMY_TYPES` object with `w`, `h`, `hp`, `speed`, `score`, `color`, `fireRate`, `shootRange`, and `etype`. They'll be spawned automatically.

### Adding Mini-Bosses
Add entries to the `MINI_BOSS_TYPES` object with a `pattern` field matching one of: `charge`, `shielded`, `artillery`, `splitter`, `jumpstomp`. Implement the AI logic in `updateMiniBossAI()`.

### Adding Weapon Fusions
Add entries to the `WEAPON_FUSIONS` object keyed as `'WEAPON1+WEAPON2'` with standard weapon properties plus a `desc` field.

### Adding Achievements
Add entries to the `ACHIEVEMENTS` array with `id`, `name`, `desc`, `icon`, `category`, and a `check(stats)` function that returns `true` when the condition is met.

### Custom Difficulty
Select **CUSTOM** difficulty, then use the Admin Panel sliders to fine-tune every parameter individually.

## Future Enhancements

- [x] ~~Sound effects and music~~ — 27 SFX + 5 procedural chip-tune tracks
- [x] ~~Online leaderboards~~ — Shareable base64 score codes
- [x] ~~Level editor export/import~~ — Full JSON file I/O with copy/paste
- [x] ~~Save/load mid-game state~~ — F5/F9 quick save + file export/import
- [x] ~~Additional stages and boss patterns~~ — 10+2 stages, 5-phase boss AI
- [x] ~~Enhanced touch controls~~ — Opacity, swap sides, vibration
- [x] ~~Gamepad / controller support~~ — Standard mapping, 2-pad co-op, vibration, dead zone config
- [x] ~~Online multiplayer via WebRTC~~ — P2P co-op with manual SDP signaling
- [x] ~~Animated sprite sheets~~ — Procedurally generated sheets with multi-frame animations for all entities
- [x] ~~Story mode with cutscenes~~ — Prologue, 10 stage briefings, epilogue with canvas-rendered cutscene engine
- [x] ~~Steam / Itch.io packaging~~ — Electron wrapper with builder configs for all platforms
- [x] ~~Game modes~~ — Endless, Boss Rush, Time Attack, New Game+, Challenge Missions
- [x] ~~Visual effects~~ — CRT shader, weather, screen transitions, dynamic lighting, animated water/lava
- [x] ~~Playable characters~~ — 6 characters with unique stats and abilities
- [x] ~~Mini-bosses~~ — 5 types with unique AI (Charger, Shielded, Artillery, Splitter, Jump Stomper)
- [x] ~~Environmental hazards~~ — Moving platforms, crushers, spike traps, conveyor belts, laser grids
- [x] ~~Weapon fusion~~ — 12 hybrid weapon recipes combining pickup pairs
- [x] ~~Speedrun timer with splits~~ — Per-stage PB tracking with delta display
- [x] ~~Accessibility options~~ — Color-blind filters, aim assist, difficulty modifiers, HUD customization
- [x] ~~Tutorial stage~~ — Interactive stage 0 teaching all mechanics
- [x] ~~Vehicles~~ — Hoverbike and Tank with unique stats
- [x] ~~Secret stages~~ — 2 hidden stages with secret exits
- [x] ~~Particle editor~~ — Real-time admin panel for tuning all particle effects
- [x] ~~Animated title screen~~ — Parallax background with demo auto-play mode
- [x] ~~Credits roll~~ — Scrolling credits sequence after victory

## Desktop App (Electron)

The game can be packaged as a standalone desktop application using Electron.

### Prerequisites

```bash
npm install
```

### Run Locally

```bash
npm start          # production mode
npm run dev        # dev mode (with DevTools)
```

### Build Distributables

| Command | Output | Platform |
|---------|--------|----------|
| `npm run build:win` | NSIS installer + ZIP | Windows x64 |
| `npm run build:mac` | DMG | macOS x64/arm64 |
| `npm run build:linux` | AppImage + ZIP | Linux x64 |
| `npm run build:all` | All of the above | Cross-platform |
| `npm run build:steam` | Unpacked directory for SteamPipe | Windows (Steam) |
| `npm run build:itch` | ZIP archives for butler upload | All platforms (Itch.io) |

Built artifacts appear in `dist/`.

### Steam Distribution

1. Build: `npm run build:steam`
2. Output: `dist/steam/win-unpacked/`
3. Create `steam_appid.txt` with your App ID in the output directory
4. Upload via SteamPipe (`steamcmd`)
5. Optional: Install `steamworks.js` for Steam achievements integration

### Itch.io Distribution

1. Build: `npm run build:itch`
2. Install [butler](https://itch.io/docs/butler/): `butler login`
3. Push builds:
   ```bash
   butler push dist/itch/ContraForce-win.zip    your-username/contra-force:win
   butler push dist/itch/ContraForce-linux.zip   your-username/contra-force:linux
   butler push dist/itch/ContraForce-mac-*.zip   your-username/contra-force:mac
   ```
4. Or upload just the HTML file for web builds:
   ```bash
   butler push contra_opus4_6.html your-username/contra-force:html5
   ```

### Desktop Features

- **F11** — Toggle fullscreen
- Automatic window sizing to fit display
- No menu bar in production mode
- Secure `contextIsolation` + `sandbox` enabled
- Optional Steamworks.js integration for Steam overlay and achievements

### Project Structure

```
contra_opus4_6.html          ← The complete game (runs standalone in browser)
package.json                 ← Electron + electron-builder configuration
electron/
  main.js                    ← Electron main process
  preload.js                 ← Secure bridge between renderer and main
  generate-icons.js          ← Placeholder icon generator (needs `canvas` npm)
  icons/                     ← App icons (PNG, ICO, ICNS)
electron-builder-steam.yml   ← Steam-specific build config
electron-builder-itch.yml    ← Itch.io-specific build config
```

### Icons

Replace the placeholder icons in `electron/icons/` with your final artwork:
- `icon.png` (256×256+) — used as source for all platforms
- `icon.ico` — Windows
- `icon.icns` — macOS
- `16x16.png` through `512x512.png` — Linux

Or use: `npx electron-icon-builder --input=electron/icons/icon.png --output=electron/icons`

## License

This is a fan recreation of the classic CONTRA arcade game. Original game by Konami.

## Credits

**CONTRA FORCE HTML5** — Modern HTML5 Implementation
- Game engine, physics, AI, and all systems: Custom implementation
- Retro pixel-art styling: CSS + Canvas sprite sheet rendering (procedural fallback)
- Input: Keyboard, mobile touch, gamepad (Gamepad API with standard mapping)
- Desktop packaging: Electron + electron-builder (Windows, macOS, Linux)

---

**Enjoy the classic arcade action! Press ENTER to start.** 🎮
