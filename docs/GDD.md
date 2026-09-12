# Unexpected Pair - Game Design Document

**Game Title**: Unexpected Pair  
**Genre**: Vertical Endless Runner  
**Platform**: Web (HTML5)  
**Engine**: Construct 3  
**Target**: Game Jam Internal  
**Date**: 2026-09-12

---

## 🎯 High Concept

Vertical endless runner dengan mekanik dual-world switching antara Alam Manusia dan Alam Hantu. Pemain harus beradaptasi dengan dua gaya gameplay berbeda sambil menghindari obstacle dan mengalahkan musuh.

---

## 🎮 Core Gameplay

### Mode Switching
- **Switch Key**: [Space/Shift/Mouse Click - TBD]
- Transisi visual antara kedua alam (color palette shift, VFX)
- Cooldown switching: [TBD - 1-2 detik?]

### Mode 1: Running (Alam Manusia) 🏃
**Movement**:
- Auto-scroll vertikal ke atas
- Movement horizontal (kiri/kanan) untuk menghindari obstacle

**Controls**:
- **Arrow Keys / A-D**: Movement horizontal
- **Jump**: [Up Arrow / W / Space]
- **Attack**: [X / Left Click]

**Combat**:
- Melee attack dengan range pendek
- Attack cooldown: ~0.5 detik
- Damage: 1 hit untuk enemy kecil

**Characteristics**:
- Kecepatan normal
- Gravity standard
- Jump arc normal

---

### Mode 2: Flying/Floating (Alam Hantu) 👻
**Movement**:
- Auto-scroll vertikal ke atas (same speed)
- Flight control ala Jetpack Joyride/Flappy Bird

**Controls**:
- **Hold [Space/Click]**: Turun
- **Release**: Naik
- **Attack**: Auto-shoot atau [X] untuk manual shoot

**Combat**:
- Ranged projectile attack
- Fire rate: [3-5 shots per detik]
- Projectile speed: Fast
- Damage: 1 hit untuk enemy kecil

**Characteristics**:
- Smooth floating motion
- No gravity (atau reversed gravity)
- Momentum-based movement

---

## 🎭 Game Elements

### Collectibles
**Coins** 💰
- Value: 10 points each
- Spawning: Scattered path, encourage switching modes
- Visual: Berbeda di kedua alam (gold coin vs spirit orb)

**Power-up Bubbles** ✨
- **Shield**: Temporary invincibility (5 detik)
- **Speed Boost**: Faster scroll speed (10 detik)
- **Magnet**: Auto-collect nearby coins (8 detik)
- **Multi-Shot**: Shoot 3 bullets (Mode 2 only, 10 detik)
- Duration indicator: Timer bar atau particle effect

---

### Enemies

**Mode 1 Enemies (Alam Manusia)**
1. **Ground Walker** - Jalan di ground level, harus dilompati atau diserang
2. **Flying Bat** - Terbang horizontal, harus diserang atau dihindari
3. **Charger** - Rush ke arah player ketika dekat

**Mode 2 Enemies (Alam Hantu)**
1. **Ghost** - Melayang slow, easy target
2. **Spirit Shooter** - Shoot projectile ke player
3. **Phase Enemy** - Teleport/dash cepat

**Enemy HP**:
- Small: 1 hit
- Medium: 2-3 hits
- Boss (optional): 5-10 hits

---

### Obstacles

**Mode 1 Obstacles**
- **Spike Wall**: Static, harus dihindari dengan movement horizontal
- **Moving Platform**: Bergerak horizontal
- **Falling Rock**: Jatuh dari atas

**Mode 2 Obstacles**
- **Energy Barrier**: Horizontal barrier, harus naik/turun untuk hindari
- **Rotating Blade**: Spinning obstacle
- **Portal Zone**: Area yang force switch mode

**Universal Obstacles**
- **Gap**: Hole yang harus dilompati (Mode 1) atau terbang melewatinya (Mode 2)

---

## 📊 UI Elements

### HUD (Head-Up Display)
1. **Health Bar** ❤️
   - Position: Top-left
   - Style: Heart containers atau bar
   - Total HP: 3-5 hearts

2. **Distance Tracker** 📏
   - Position: Top-center atau top-right
   - Format: "Distance: XXXm"
   - Updates real-time

3. **Score/Coins** 💰
   - Position: Top-right
   - Format: "Score: XXXX | Coins: XXX"

4. **Mode Indicator**
   - Visual cue: Icon atau background color
   - Shows current mode active

5. **Power-up Timer**
   - Position: Near player atau bottom
   - Shows remaining duration

### Menus
- **Main Menu**: Play, Settings, Credits, Quit
- **Pause Menu**: Resume, Restart, Settings, Main Menu
- **Game Over Screen**: Final Score, Distance, Retry, Main Menu
- **Settings**: Volume sliders, Controls remapping

---

## 🎨 Art Direction

### Visual Style
- **Alam Manusia**: Warm colors (orange, yellow, brown), daylight
- **Alam Hantu**: Cool colors (purple, blue, cyan), ethereal glow
- Contrast antara kedua alam harus jelas untuk gameplay clarity

### Player Character
- **Mode 1**: Human form - solid, grounded design
- **Mode 2**: Ghost/spirit form - translucent, glowing
- Smooth transition animation between forms

### Background
- Parallax scrolling layers
- Distinct visual for each realm
- Transition effect saat switch mode (color grading, particle burst)

---

## 🔊 Audio Design

### Music
- **Main Menu**: Calm, mysterious
- **Gameplay**: Upbeat, adaptive (intensity increases with distance)
- **Game Over**: Sad/melancholic

### SFX
- **Mode Switch**: "Whoosh" dengan ethereal echo
- **Jump**: Light "hop" sound
- **Melee Attack**: "Swish" slash sound
- **Ranged Attack**: "Pew" laser/magic sound
- **Hit/Damage**: Impact sound + player grunt
- **Coin Collect**: "Ding" atau chime
- **Power-up**: Magical jingle
- **Enemy Death**: Explosion atau fade out sound
- **Obstacle Hit**: Crash/collision sound

---

## 📈 Progression & Difficulty

### Difficulty Curve
- **0-500m**: Tutorial phase, slow speed, few enemies
- **500-1500m**: Normal phase, standard speed, mixed enemies
- **1500m+**: Hard phase, faster speed, dense obstacles

### Spawn Rate
- Enemies spawn rate increases over distance
- Obstacle density increases
- Power-up spawn rate: Constant atau slight decrease untuk challenge

### Speed Scaling
- Base scroll speed: [TBD - e.g., 200 pixels/sec]
- Speed increase: +5% every 500m
- Max speed cap: 150% base speed

---

## 🎯 Win/Lose Conditions

### Lose Conditions
- HP reaches 0
- Fall off screen (jika ada gap tanpa bottom boundary)

### Win Condition
- Endless runner - no win, goal adalah high score
- Leaderboard: Track highest distance & score

### Scoring System
- Distance traveled: 1 point per meter
- Coin collected: 10 points each
- Enemy killed: 50 points
- Power-up collected: 25 points
- Combo multiplier: Consecutive actions tanpa hit (x1.5, x2, x2.5)

---

## 🛠️ Technical Implementation (Construct 3)

### Event Sheets Structure
```
- MainMenu
- GameplayCore
  - PlayerController
  - ModeSwitch
  - EnemySpawner
  - ObstacleSpawner
  - CollectibleSpawner
  - CollisionDetection
  - ScoreManager
- UIManager
- AudioManager
- GameOver
```

### Key Variables
```
Global Variables:
- PlayerHP
- CurrentScore
- DistanceTraveled
- CurrentMode (1 or 2)
- PowerUpActive (boolean)
- PowerUpType
- ScrollSpeed
- DifficultyLevel

Player Variables:
- IsInvincible
- CanAttack
- CanSwitch
```

### Object Hierarchy
```
Player
├── PlayerSprite
├── HitboxMode1
├── HitboxMode2
└── AttackHitbox

Enemy (Family)
├── GroundWalker
├── FlyingBat
└── Ghost

Obstacle (Family)
├── SpikeWall
├── EnergyBarrier
└── RotatingBlade
```

---

## ✅ Development Roadmap

### Phase 1: Core Mechanics (Week 1)
- [ ] Player movement Mode 1 (run, jump)
- [ ] Player movement Mode 2 (float control)
- [ ] Mode switching mechanism
- [ ] Basic camera follow/auto-scroll

### Phase 2: Combat & Interaction (Week 1-2)
- [ ] Melee attack (Mode 1)
- [ ] Ranged attack (Mode 2)
- [ ] Enemy basic AI (3 types)
- [ ] Collision detection (player-enemy-obstacle)
- [ ] Health system

### Phase 3: Game Elements (Week 2)
- [ ] Coin collection
- [ ] Power-up system (3-4 types)
- [ ] Obstacle spawning
- [ ] Enemy spawning system

### Phase 4: UI & Polish (Week 2-3)
- [ ] HUD (health, score, distance)
- [ ] Main menu
- [ ] Pause menu
- [ ] Game over screen
- [ ] Settings menu

### Phase 5: Content & Balance (Week 3)
- [ ] Art assets (sprites, backgrounds)
- [ ] Sound effects
- [ ] Background music
- [ ] Difficulty balancing
- [ ] Playtesting

### Phase 6: Final Polish (Week 3-4)
- [ ] VFX & particles
- [ ] Transition animations
- [ ] Bug fixes
- [ ] Optimization
- [ ] Final build & export

---

## 🎲 Future Ideas (Post-Jam)
- Boss fights setiap 1000m
- Multiple characters dengan abilities berbeda
- Unlock system (skins, power-ups)
- Daily challenges
- Leaderboard online
- Mobile port
- Story mode dengan ending

---

## 📝 Notes & Considerations

### Design Decisions TBD
- [ ] Input method: Keyboard only atau keyboard + mouse?
- [ ] Attack di Mode 2: Auto-shoot atau manual?
- [ ] Mode switch cooldown: Ya atau tidak?
- [ ] Combo system: Implement atau skip untuk simplicity?
- [ ] Checkpoints: Ada atau pure endless?

### Technical Challenges
- Smooth mode transition tanpa jarring gameplay
- Balancing difficulty curve untuk tetap fun
- Spawning algorithm yang fair (tidak spawning impossible patterns)
- Performance optimization untuk spawn pool

### Scope Management
Untuk game jam, prioritas:
1. ✅ Core mechanics works perfectly
2. ✅ Mode switching feels good
3. ✅ Basic enemies & obstacles
4. ⚠️ Polish (nice to have, jangan sacrifice gameplay)
5. ⚠️ Extra content (jika ada waktu)

---

**Document Version**: 1.0  
**Last Updated**: 2026-09-12  
**Contributors**: [Tambahkan nama tim]
