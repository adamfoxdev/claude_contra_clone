# CONTRA FORCE — HTML5

A classic side-scrolling shooter game implemented in HTML5 Canvas with full physics, weapons, co-op multiplayer, achievements, leaderboards, custom difficulty, procedural music, save/load, shareable scores, and arcade-style gameplay mechanics.

## Overview

CONTRA FORCE is a faithful HTML5 recreation of the classic arcade action game, featuring:
- **Pixel-perfect graphics** with retro arcade aesthetics and parallax backgrounds
- **Physics-based gameplay** with gravity, momentum, and collision detection
- **2-Player Local Co-op** — fight side-by-side on the same keyboard
- **Procedural chip-tune music** — 5 dynamic tracks (title, action, boss, gameover, victory) with Web Audio synthesis
- **27 sound effects** — weapon-specific SFX, enemy sounds, jingles, and UI feedback
- **25 Achievements** with toast notifications, progress tracking, and a dedicated panel
- **Leaderboard system** with persistent top-20 high scores and shareable score codes
- **5 Difficulty presets** — Easy, Normal, Hard, Nightmare, and Custom
- **10 Stages** with unique themes, boss fights, and progressive difficulty
- **8 Enemy types** — Soldier, Runner, Turret, Sniper, Heavy, Bomber, Jumper, Shield
- **10+ Weapons** including Spread, Laser, Machine Gun, Flamethrower, Rocket, Homing, Wave, and Pierce
- **Advanced boss AI** — up to 5 attack phases with stage-scaling difficulty
- **Combo kill system** with visual popups and score multipliers
- **Save/Load system** — quick save (F5), quick load (F9), file export/import
- **Level editor file I/O** — download and upload custom levels as JSON
- **Destructible terrain** — dig and blast through tiles
- **Grapple hook** for traversing gaps and climbing
- **Treasure system** — Gold, Gems, Ancient Relics, and Shield pickups
- **Admin panel** for physics tweaking, gameplay tuning, and debug tools
- **Enhanced touch controls** with adjustable opacity, swappable sides, and vibration

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

## Level Editor File I/O

Export and import custom level designs:
- **Download Level**: Admin Panel → Level File I/O → saves current stage map, enemies, power-ups, and objects as JSON
- **Upload Level**: Load a previously saved level file to continue editing
- **Copy/Paste JSON**: Quick copy level data to clipboard or paste from clipboard

## Gameplay Elements

- **Lives**: Shown in the HUD (♥ symbols). Varies by difficulty
- **Score**: Points from kills, terrain destruction, treasures — scaled by difficulty multiplier
- **Weapons**: Collected from floating power-ups throughout stages
  - Rifle, Spread, Laser, Machine Gun, Flame, Rocket, Homing, Wave, Pierce
  - Extra lives can also drop as "LIFE" power-ups
- **Stages**: 10 unique themed environments each with a boss:
  1. Jungle — 2. Base — 3. Waterfall — 4. Snow Mountain — 5. Alien Lair — 6. Energy Zone — 7. Final Fortress — 8. Volcano — 9. Cyber Core — 10. Alien Hive
- **Zoom**: Adjust from 40% to 250% with +/- keys
- **Destructible tiles**: Shoot or dig through breakable terrain to uncover paths and treasure
- **Treasures**: Gold Bars (+500), Gems (+1000), Ancient Relics (+2500), Shield (3s invincibility)

## Technical Features

### Architecture
- **Single-file implementation**: All HTML, CSS, and JavaScript in one portable HTML file
- **Canvas-based rendering**: Hardware-accelerated 2D graphics with pixel-perfect rendering
- **Custom physics engine**: Gravity, collision detection, wall climbing, ladder mechanics
- **Seeded random generation**: Reproducible level layouts per stage number

### Physics System
- Gravity simulation with terminal velocity
- Precise AABB collision detection for tiles, entities, and projectiles
- Ladder climbing, wall crawling, and grapple traversal
- Platform pass-through (one-way platforms)

### Game Systems
- **HUD**: Lives, score, weapon, stage/theme, difficulty badge, zoom, grapple cooldown, achievement counter, leaderboard button
- **Enemy AI**: 8 enemy types with patrol, shooting, jumping, and shield behaviors
- **Boss fights**: Unique boss per stage with up to 5 phased attack patterns, stage-scaling difficulty, charge attacks, ground waves, and aerial bombardment
- **Co-op management**: Independent lives, shared score, co-op game-over logic
- **Particle system**: Explosions, blood, sparks, glow particles, debris, screen shake, hit freeze, screen flash, and ring effects
- **State management**: Title, playing, paused, game over, and victory states

## Admin Panel

Access with the **`** (backtick) key to open an in-game debug/tuning panel.

### Sections
- **Physics**: Gravity, jump force, movement speed, climb speed, max fall speed
- **Gameplay**: Enemy spawn rate, enemy HP multiplier, bullet speed, fire rate, zoom, hitbox display
- **AI / Enemies**: Kill all enemies, respawn enemies, clear bullets
- **Stage**: Jump to any stage (1–10), regenerate level
- **Player**: Add lives, toggle invincibility, give weapons, set score
- **Co-op**: Toggle 2-player mode
- **Editor**: Tile editor mode (paint solid/ladder/erase), clear painted tiles
- **Save/Load**: Quick save, load, delete, export/import save files
- **Level File I/O**: Download/upload levels, copy/paste JSON
- **Audio**: Music volume, SFX volume, enable/disable toggles
- **Touch Controls**: Opacity, button size (S/M/L), vibration, swap sides
- **Achievements**: Reset achievements, reset leaderboard
- **Keybinds**: Rebind controls for both players

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

### Adding Achievements
Add entries to the `ACHIEVEMENTS` array with `id`, `name`, `desc`, `icon`, `category`, and a `check(stats)` function that returns `true` when the condition is met.

### Custom Difficulty
Select **CUSTOM** difficulty, then use the Admin Panel sliders to fine-tune every parameter individually.

## Future Enhancements

- [x] ~~Sound effects and music~~ — 27 SFX + 5 procedural chip-tune tracks
- [x] ~~Online leaderboards~~ — Shareable base64 score codes
- [x] ~~Level editor export/import~~ — Full JSON file I/O with copy/paste
- [x] ~~Save/load mid-game state~~ — F5/F9 quick save + file export/import
- [x] ~~Additional stages and boss patterns~~ — 10 stages, 5-phase boss AI
- [x] ~~Enhanced touch controls~~ — Opacity, swap sides, vibration
- [ ] Gamepad / controller support
- [ ] Online multiplayer via WebRTC
- [ ] Animated sprite sheets (replace procedural drawing)
- [ ] Story mode with cutscenes
- [ ] Steam / Itch.io packaging (Electron)

## License

This is a fan recreation of the classic CONTRA arcade game. Original game by Konami.

## Credits

**CONTRA FORCE HTML5** — Modern HTML5 Implementation
- Game engine, physics, AI, and all systems: Custom implementation
- Retro pixel-art styling: CSS + Canvas procedural rendering
- Input: Keyboard, mobile touch, gamepad-ready architecture

---

**Enjoy the classic arcade action! Press ENTER to start.** 🎮
