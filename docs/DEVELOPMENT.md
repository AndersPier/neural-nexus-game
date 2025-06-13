# Neural Nexus Development Guide

## Quick Start

```bash
# Clone and run locally
git clone https://github.com/AndersPier/neural-nexus-game.git
cd neural-nexus-game
npm start  # or python -m http.server 8000
```

Open http://localhost:8000 in your browser.

## Architecture Overview

Neural Nexus is built as a single HTML file containing all game logic, styled with modern CSS, and powered by vanilla JavaScript for maximum performance.

### Core Components

```javascript
// Main game systems
├── GameState          // Central state management
├── Node               // Game node entities  
├── Connection         // Node connection logic
├── ParticleSystem     // Visual effects
└── InputManager       // Mouse/touch handling
```

### File Structure

```
neural-nexus-game/
├── index.html         # Complete game (19KB)
├── package.json       # Development tools
├── README.md          # Project overview
└── docs/             # Technical documentation
    ├── DEVELOPMENT.md # This file
    ├── GAME_DESIGN.md # Mechanics and progression
    └── PERFORMANCE.md # Optimization guide
```

## Development Workflow

### Local Development
```bash
npm run dev        # Start development server
npm run lighthouse # Run performance audit
```

### Code Organization
The game uses modern JavaScript patterns:

```javascript
// ES6 Classes for game entities
class Node {
  constructor(x, y, id, type = 'normal') {
    this.x = x;
    this.y = y;
    this.id = id;
    this.type = type;
  }
  
  draw() {
    // Canvas rendering
  }
}

// Event-driven input system
canvas.addEventListener('mousedown', handlePointerStart);
canvas.addEventListener('touchstart', handlePointerStart, {passive: false});
```

### Performance Targets
- **Desktop**: 60fps consistent
- **Mobile**: 30fps minimum  
- **Memory**: <50MB during gameplay
- **Load Time**: <3 seconds on 3G

### Testing Protocol
```markdown
**Manual Testing Checklist:**
- [ ] Chrome/Firefox/Safari desktop
- [ ] iOS Safari mobile
- [ ] Android Chrome mobile
- [ ] Touch interactions responsive
- [ ] Performance targets met
```

## Game Mechanics

### Core Gameplay Loop
1. **Level Generation**: Procedural pattern creation
2. **Player Input**: Click/drag to connect nodes  
3. **Validation**: Check against target pattern
4. **Progression**: Advance to next level with score

### Difficulty Scaling
```javascript
const progression = {
  nodeCount: level => Math.min(3 + Math.floor(level / 2), 12),
  timeLimit: level => Math.max(30, 60 - Math.floor(level / 3) * 2),
  complexity: level => Math.min(level * 0.8 + 2, 10)
};
```

## Technical Implementation

### Canvas Rendering
```javascript
// Optimized game loop
function gameLoop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  
  if (!gameState.isPlaying) return;
  
  drawTargetPattern();
  gameState.connections.forEach(conn => conn.draw());
  gameState.nodes.forEach(node => node.draw());
  
  requestAnimationFrame(gameLoop);
}
```

### Input Handling
```javascript
// Unified mouse/touch events
function getPointerPosition(e) {
  const rect = canvas.getBoundingClientRect();
  const clientX = e.clientX || (e.touches && e.touches[0].clientX);
  const clientY = e.clientY || (e.touches && e.touches[0].clientY);
  
  return {
    x: clientX - rect.left,
    y: clientY - rect.top
  };
}
```

### Memory Management
```javascript
// Object pooling for particles
class ParticlePool {
  constructor(size = 100) {
    this.pool = [];
    this.active = [];
  }
  
  acquire() {
    return this.pool.pop() || new Particle();
  }
  
  release(particle) {
    particle.reset();
    this.pool.push(particle);
  }
}
```

## Deployment

### GitHub Pages
Automatic deployment is configured:
- **Production**: https://andersPier.github.io/neural-nexus-game/
- **Staging**: Any branch can be deployed for testing

### Manual Deployment
```bash
# Copy single file to any static host
cp index.html /path/to/web/server/
```

### Performance Monitoring
```bash
npm run lighthouse  # Generate performance report
```

## Future Enhancements

### Planned Features
- **Audio System**: Web Audio API integration
- **Save System**: LocalStorage progress persistence  
- **PWA**: Offline gameplay capability
- **Level Editor**: Community content creation

### Technical Debt
- **Modularization**: Split single file into modules
- **TypeScript**: Add type safety for larger codebase
- **Testing**: Implement automated testing framework
- **Build System**: Add bundling for production optimization

## Contributing

### Code Style
- ES6+ JavaScript features
- Meaningful variable names
- Comments for complex game logic
- Performance-first approach

### Pull Request Process
1. Fork repository
2. Create feature branch
3. Test thoroughly on multiple devices
4. Submit PR with clear description

### Performance Requirements
- No frame rate regressions
- Memory usage must remain stable
- Mobile compatibility maintained
- Load time not increased

## Troubleshooting

### Common Issues

**Game Not Loading:**
- Check browser console for errors
- Verify JavaScript is enabled
- Try hard refresh (Ctrl+F5)

**Poor Performance:**
- Check frame rate in DevTools
- Close other browser tabs
- Try in incognito mode

**Touch Not Working:**
- Ensure using actual mobile device
- Check preventDefault() calls
- Verify touch event handling

### Debug Tools
```javascript
// Development helpers (localhost only)
if (location.hostname === 'localhost') {
  window.gameDebug = {
    fps: () => console.log('FPS monitoring enabled'),
    skipLevel: () => showLevelComplete(),
    godMode: () => gameState.timeLeft = 999
  };
}
```

## Related Documentation

- **Claude Project**: [Development methodology and workflow](https://github.com/AndersPier/neural-nexus-claude-project)
- **Game Design**: See GAME_DESIGN.md for mechanics details
- **Performance**: See PERFORMANCE.md for optimization guide

---

**Last Updated**: June 2025
**Current Version**: 1.0.0