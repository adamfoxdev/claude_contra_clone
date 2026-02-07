# CONTRA FORCE — HTML5

A classic side-scrolling shooter game implemented in HTML5 Canvas with full physics, weapons, co-op multiplayer, achievements, leaderboards, custom difficulty, and arcade-style gameplay mechanics.

## Overview

CONTRA FORCE is a faithful HTML5 recreation of the classic arcade action game, featuring:
- **Pixel-perfect graphics** with retro arcade aesthetics and parallax backgrounds
- **Physics-based gameplay** with gravity, momentum, and collision detection
- **2-Player Local Co-op** — fight side-by-side on the same keyboard
- **25 Achievements** with toast notifications, progress tracking, and a dedicated panel
- **Leaderboard system** with persistent top-20 high scores (localStorage)
- **5 Difficulty presets** — Easy, Normal, Hard, Nightmare, and Custom
- **7 Stages** with unique themes, boss fights, and progressive difficulty
- **8 Enemy types** — Soldier, Runner, Turret, Sniper, Heavy, Bomber, Jumper, Shield
- **10+ Weapons** including Spread, Laser, Machine Gun, Flamethrower, Rocket, Homing, Wave, and Pierce
- **Combo kill system** with visual popups and score multipliers
- **Destructible terrain** — dig and blast through tiles
- **Grapple hook** for traversing gaps and climbing
- **Treasure system** — Gold, Gems, Ancient Relics, and Shield pickups
- **Admin panel** for physics tweaking, gameplay tuning, and debug tools
- **Mobile touch controls** with adjustable button sizes (S/M/L)

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
| **Escape** | Close open panels |
| **`** (backtick) | Toggle Admin Panel |

### Mobile Touch Controls

On mobile devices and tablets, virtual touch controls automatically appear:

**D-Pad (Left Side)** — ↑ ↓ ← → for movement & aim

**Action Buttons (Right Side):**
- **JUMP** (Green) — Jump / climb
- **FIRE** (Red) — Shoot
- **HOOK** (Blue) — Grapple hook

**Button Size Adjustment:** Tap S / M / L in the top-right to resize controls. Your preference is saved to localStorage.

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
| **Boss** | Boss Slayer, Boss Rush (all 7 bosses) |
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

## Gameplay Elements

- **Lives**: Shown in the HUD (♥ symbols). Varies by difficulty
- **Score**: Points from kills, terrain destruction, treasures — scaled by difficulty multiplier
- **Weapons**: Collected from floating power-ups throughout stages
  - Rifle, Spread, Laser, Machine Gun, Flame, Rocket, Homing, Wave, Pierce
  - Extra lives can also drop as "LIFE" power-ups
- **Stages**: 7 unique themed environments (Jungle, Base, Snow, Lava, Ruins, Fortress, Stronghold) each with a boss
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
- **Boss fights**: Unique boss per stage with phased attack patterns and HP scaling
- **Co-op management**: Independent lives, shared score, co-op game-over logic
- **Particle system**: Explosions, blood, sparks, glow particles, debris, screen shake, hit freeze, screen flash, and ring effects
- **State management**: Title, playing, paused, game over, and victory states

## Admin Panel

Access with the **`** (backtick) key to open an in-game debug/tuning panel.

### Sections
- **Physics**: Gravity, jump force, movement speed, climb speed, max fall speed
- **Gameplay**: Enemy spawn rate, enemy HP multiplier, bullet speed, fire rate, zoom, hitbox display
- **AI / Enemies**: Kill all enemies, respawn enemies, clear bullets
- **Stage**: Jump to any stage (1–7), regenerate level
- **Player**: Add lives, toggle invincibility, give weapons, set score
- **Co-op**: Toggle 2-player mode
- **Editor**: Tile editor mode (paint solid/ladder/erase), clear painted tiles
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

- [ ] Sound effects and music (Web Audio synth foundations already present)
- [ ] Online leaderboards
- [ ] Level editor export/import
- [ ] Save/load mid-game state
- [ ] Additional stages and boss patterns
- [ ] Enhanced touch controls with customizable layouts

## License

This is a fan recreation of the classic CONTRA arcade game. Original game by Konami.

## Credits

**CONTRA FORCE HTML5** — Modern HTML5 Implementation
- Game engine, physics, AI, and all systems: Custom implementation
- Retro pixel-art styling: CSS + Canvas procedural rendering
- Input: Keyboard, mobile touch, gamepad-ready architecture

---

**Enjoy the classic arcade action! Press ENTER to start.** 🎮
