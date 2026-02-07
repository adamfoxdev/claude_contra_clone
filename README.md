# CONTRA FORCE — HTML5

A classic side-scrolling shooter game implemented in HTML5 Canvas with full physics, weapons, and arcade-style gameplay mechanics.

## Overview

CONTRA FORCE is a faithful HTML5 recreation of the classic arcade action game, featuring:
- **Pixel-perfect graphics** with retro arcade aesthetics
- **Physics-based gameplay** with gravity, momentum, and collision detection
- **Multiple weapons** including rifles, spread shots, and special equipment
- **Progressive level system** with increasing difficulty
- **Power-ups and collectibles** scattered throughout levels
- **Lives and scoring system** for classic arcade progression
- **Grapple mechanics** for traversing dangerous terrain
- **Admin panel** for physics tweaking and gameplay tuning

## How to Play

### Starting the Game
1. Open `contra_opus4_6.html` in your web browser
2. Press **ENTER** to start the game

### Controls

| Key | Action |
|-----|--------|
| **Arrow Up** | Jump / Climb |
| **Arrow Down** | Crouch / Drop through platforms |
| **Arrow Left** | Move left |
| **Arrow Right** | Move right |
| **Z** | Fire weapon |
| **X** | Secondary action / Grapple |
| **C** | Equipment action |
| **SPACE** | Alternate fire |
| **ENTER** | Start game |
| **Esc** | Pause / Menu |

### Gameplay Elements

- **Lives**: Displayed in the HUD (♥ symbols). Lose all lives and it's game over
- **Score**: Earn points by defeating enemies and completing objectives
- **Weapons**: Switch between different weapon types as you progress:
  - Rifle: Standard weapon
  - Spread Shot: Wide-area damage
  - Special weapons unlocked later
- **Stages**: Progress through multiple challenging stages with different environments
- **Zoom**: Use the zoom feature to see more of the level (default 100%)

## Technical Features

### Architecture
- **Single-file implementation**: All HTML, CSS, and JavaScript contained in one file for easy distribution
- **Canvas-based rendering**: Hardware-accelerated graphics with pixel-perfect rendering
- **Physics engine**: Custom implemented gravity, collision detection, and object interactions
- **Input handling**: Keyboard controls with proper debouncing and state management

### Physics System
- **Gravity simulation**: Realistic falling mechanics
- **Collision detection**: Precise platformer collision handling
- **Velocity and momentum**: Smooth character movement physics
- **Platform interactions**: Dynamic platform collision and standing/landing logic

### Game Systems
- **HUD Display**: Real-time stats including lives, score, weapon type, stage, and zoom level
- **Enemy AI**: Pattern-based enemy behavior with different enemy types
- **Collision response**: Different behaviors for walls, platforms, enemies, and hazards
- **State management**: Game states for menu, playing, paused, and game over

## Admin Panel

The game includes a debug/admin panel for physics and gameplay tuning:

### Physics Settings
- **Gravity**: Adjust gravity strength (0-2x)
- **Jump force**: Control character jump height
- **Movement speed**: Adjust character movement speed
- **Air control**: Change mid-air control responsiveness

### Gameplay Settings
- **Enemy spawn rate**: Adjust enemy frequency
- **Difficulty modifier**: Scale enemy health and damage
- **Physics debugging**: Toggle collision boxes and physics visualization
- **Level tweaking**: Adjust level parameters and test values

**To access admin panel**: Open browser developer console and look for admin section toggle

## Browser Compatibility

- **Chrome 90+**: Full support
- **Firefox 88+**: Full support
- **Safari 14+**: Full support
- **Edge 90+**: Full support

Requires HTML5 Canvas, ES6 JavaScript support, and hardware acceleration for optimal performance.

## Performance Notes

- **Image rendering**: Uses `pixelated` image rendering for authentic retro look
- **Canvas optimization**: Efficient dirty rectangle updates
- **Frame rate**: Consistent 60 FPS target on modern hardware

## Development

### Modifying Physics
Physics parameters can be adjusted through the Admin Panel or by editing the JavaScript configuration section.

### Adding New Levels
Modify the level data structures in the game code to create new stage layouts with different enemy patterns and obstacles.

### Extending Weapons
Add new weapon types by:
1. Defining weapon properties (damage, speed, spread, etc.)
2. Adding firing logic to the weapon system
3. Creating visual assets for the weapon

## Future Enhancements

- [ ] Mobile touch controls
- [ ] Sound effects and music
- [ ] Multiplayer co-op mode
- [ ] Level editor
- [ ] Custom difficulty settings
- [ ] Achievement/leaderboard system
- [ ] Save/load game state

## License

This is a fan recreation of the classic CONTRA arcade game. Original game by Konami.

## Credits

**CONTRA FORCE HTML5** - Modern HTML5 Implementation
- Game engine and physics: Custom implementation
- Retro styling: CSS-based arcade aesthetics
- Controls: Keyboard input handling

---

**Enjoy the classic arcade action!** 🎮
