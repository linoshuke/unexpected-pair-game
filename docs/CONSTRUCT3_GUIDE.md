# Construct 3 Development Guide

Panduan teknis untuk implementasi game di Construct 3.

---

## 🎯 Project Setup

### Initial Configuration
1. **New Project Settings:**
   - Project type: New Empty Project
   - Window size: 1920x1080 (16:9 landscape) atau 1080x1920 (9:16 portrait untuk vertical)
   - Sampling: Linear
   - Fullscreen mode: Letterbox scale
   - Orientation: Portrait (untuk vertical runner)

2. **Project Properties:**
   ```
   Name: Unexpected Pair
   Version: 0.1.0
   Author: [Nama Tim]
   Description: Vertical endless runner dengan dual-world mechanics
   ```

---

## 📐 Layout Structure

### Layouts
```
- MainMenu          (Menu utama)
- GameplayLevel     (Main game layout)
- GameOver          (Game over screen)
```

### Layers (dalam GameplayLevel)

**Recommended layer order (bottom to top):**
```
1. Background_Far        (Parallax 20%, non-interactive)
2. Background_Mid        (Parallax 50%)
3. Background_Near       (Parallax 80%)
4. Ground                (Platform/ground tiles, solid)
5. Collectibles          (Coins, power-ups)
6. Enemies               (Enemy objects)
7. Player                (Player character)
8. Obstacles             (Spike, barriers - collision)
9. Projectiles           (Bullets, attacks)
10. Effects              (VFX, particles - top layer, non-interactive)
11. UI                   (HUD elements - 0% parallax, always visible)
```

**Layer Properties:**
- **UI layer**: 
  - Parallax: 0%, 0%
  - Z elevation: 100
  - Transparent: Yes
  
- **Background layers**: 
  - Parallax sesuai depth (far = 20%, near = 80%)
  - Interactive: No

---

## 🎮 Object Types & Families

### Player Object
**Object: Player (Sprite)**
- Behaviors:
  - Platform (Mode 1) - disable saat Mode 2
  - Custom Movement (untuk Mode 2 float control)
- Instance Variables:
  ```
  CurrentMode = 1              (Number: 1 atau 2)
  HP = 3                       (Number)
  IsInvincible = 0             (Boolean: 0 atau 1)
  CanAttack = 1                (Boolean)
  CanSwitch = 1                (Boolean)
  AttackCooldown = 0           (Number)
  SwitchCooldown = 0           (Number)
  ```
- Animations:
  - Mode1_Idle, Mode1_Run, Mode1_Jump, Mode1_Attack
  - Mode2_Idle, Mode2_Float, Mode2_Shoot
  - Transition, Hit, Death

---

### Enemy Family
**Family: EnemyFamily**

Include objects:
- Enemy_GroundWalker
- Enemy_Bat
- Enemy_Ghost
- Enemy_Shooter
- Enemy_Charger

**Family Instance Variables:**
```
EnemyHP = 1              (Number)
EnemyDamage = 1          (Number)
EnemySpeed = 100         (Number)
EnemyType = "walker"     (Text)
IsActive = 1             (Boolean)
```

**Individual Enemy Behaviors:**
- GroundWalker: Platform, Sin (untuk sway motion)
- Bat: Bullet (horizontal movement)
- Ghost: Custom Movement
- Shooter: Bullet + Timer (untuk shoot interval)

---

### Obstacle Family
**Family: ObstacleFamily**

Include objects:
- Obstacle_Spike
- Obstacle_Barrier
- Obstacle_RotatingBlade
- Obstacle_MovingPlatform

**Family Instance Variables:**
```
ObstacleDamage = 1       (Number)
IsDestructible = 0       (Boolean)
```

---

### Collectibles

**Coin (Sprite)**
- Behaviors: None (static atau simple rotate)
- Instance Variables:
  ```
  CoinValue = 10         (Number)
  ```

**PowerUp (Sprite)**
- Instance Variables:
  ```
  PowerUpType = "shield"    (Text: shield/speed/magnet/multishot)
  Duration = 5              (Number: seconds)
  ```
- Animations: Shield, SpeedBoost, Magnet, MultiShot

---

### Projectiles

**PlayerBullet (Sprite)**
- Behaviors: Bullet
  - Speed: 800
  - Angle: 90 (up)
- Instance Variables:
  ```
  Damage = 1
  ```

**EnemyBullet (Sprite)**
- Behaviors: Bullet
  - Speed: 400
  - Angle: 270 (down toward player)
- Instance Variables:
  ```
  Damage = 1
  ```

---

## 🧩 Event Sheet Structure

### 1. EventSheet_Global
**Global variables, functions, reusable logic**

```
Global Variables:
- GameScore = 0              (Number)
- DistanceTraveled = 0       (Number)
- CoinsCollected = 0         (Number)
- ScrollSpeed = 200          (Number: pixels per second)
- DifficultyLevel = 1        (Number)
- GameState = "menu"         (Text: menu/playing/paused/gameover)
- IsMusicEnabled = 1         (Boolean)
- IsSFXEnabled = 1           (Boolean)

Global Arrays:
- HighScores                 (Size: 5, untuk top 5 scores)
```

---

### 2. EventSheet_MainMenu

**Events:**
```
On start of layout
  → Play music "MainMenu"
  → Set GameState to "menu"

On "ButtonPlay" clicked
  → Go to layout "GameplayLevel"

On "ButtonSettings" clicked
  → Open settings panel

On "ButtonQuit" clicked
  → Close browser
```

---

### 3. EventSheet_Gameplay
**Main game logic**

#### Group: Initialization
```
On start of layout
  → Reset variables (Score, Distance, HP)
  → Set ScrollSpeed to 200
  → Spawn player at starting position
  → Play music "Gameplay"
  → Enable spawner timers
```

#### Group: Player_Movement_Mode1
```
Platform behavior enabled

On "Jump" key pressed
  + Player is on floor
  → Player simulate jump
  → Play sound "Jump"

On "Left" pressed
  → Player set Platform vector X to -300

On "Right" pressed
  → Player set Platform vector X to 300
```

#### Group: Player_Movement_Mode2
```
Platform behavior disabled
Custom movement enabled

While "Hold" key is down
  → Player move at angle 270 (down) speed 300

While "Hold" key NOT down
  → Player move at angle 90 (up) speed 300

Clamp Y position
  → Player Y > Layout.Height - 100 : Set Y to Layout.Height - 100
  → Player Y < 100 : Set Y to 100
```

#### Group: Mode_Switch
```
On "Switch" key pressed
  + Player.CanSwitch = 1
  
  → System: Toggle Player.CurrentMode (1↔2)
  → If CurrentMode = 1:
      - Enable Platform behavior
      - Disable Custom Movement
      - Set animation to "Mode1_Idle"
  → If CurrentMode = 2:
      - Disable Platform behavior
      - Enable Custom Movement
      - Set animation to "Mode2_Idle"
  → Play sound "ModeSwitch"
  → Spawn VFX "TransitionEffect" at Player
  → Set CanSwitch to 0
  → Wait 1 second → Set CanSwitch to 1
```

#### Group: Player_Attack
```
On "Attack" key pressed
  + Player.CanAttack = 1
  + Player.CurrentMode = 1
  
  → Set animation to "Mode1_Attack"
  → Spawn "MeleeHitbox" at Player
  → Play sound "MeleeAttack"
  → Set CanAttack to 0
  → Wait 0.5 seconds → Set CanAttack to 1

On "Attack" key pressed
  + Player.CanAttack = 1
  + Player.CurrentMode = 2
  
  → Spawn "PlayerBullet" at Player.ImagePoint("Gun")
  → PlayerBullet set angle to 90
  → Play sound "Shoot"
  → Set CanAttack to 0
  → Wait 0.3 seconds → Set CanAttack to 1
```

#### Group: Camera_Scroll
```
Every tick
  → Camera move Y by -ScrollSpeed * dt
  → Player move Y by -ScrollSpeed * dt (keep player relative to camera)

(Alternative: Set Player Y to fixed position, scroll world objects down)
```

#### Group: Spawn_System

**Spawn Enemies:**
```
Every 2-4 seconds (random)
  + DistanceTraveled > 100
  
  → Choose random spawn point X (left/center/right)
  → Create random enemy from EnemyFamily at (X, Camera.Y - 600)
  → Set enemy behaviors
```

**Spawn Obstacles:**
```
Every 3-5 seconds (random)
  
  → Create random obstacle at (RandomX, Camera.Y - 800)
```

**Spawn Collectibles:**
```
Every 1-2 seconds
  
  → Create Coin at random safe position
  → 10% chance: Create PowerUp instead
```

#### Group: Collision_Detection

**Player vs Enemy:**
```
Player on collision with EnemyFamily
  + Player.IsInvincible = 0
  
  → Subtract 1 from Player.HP
  → Play sound "PlayerHit"
  → Set IsInvincible to 1
  → Flash player sprite (white tint)
  → Wait 1.5 seconds → Set IsInvincible to 0
  
  → If HP ≤ 0:
      - Go to "GameOver" layout
```

**Player Attack vs Enemy:**
```
MeleeHitbox on collision with EnemyFamily
  
  → Subtract Player.Damage from Enemy.EnemyHP
  → Play sound "EnemyHit"
  → If Enemy.EnemyHP ≤ 0:
      - Add 50 to Score
      - Spawn death VFX
      - Destroy Enemy
      - Play sound "EnemyDeath"

PlayerBullet on collision with EnemyFamily
  
  → Subtract 1 from Enemy.EnemyHP
  → Destroy PlayerBullet
  → (rest same as above)
```

**Player vs Collectible:**
```
Player on collision with Coin
  
  → Add Coin.CoinValue to Score
  → Add 1 to CoinsCollected
  → Play sound "CoinCollect"
  → Spawn collect VFX
  → Destroy Coin

Player on collision with PowerUp
  
  → Activate power-up based on PowerUp.PowerUpType
  → Play sound "PowerUpCollect"
  → Set power-up timer UI
  → Destroy PowerUp
```

#### Group: PowerUp_System
```
Function "ActivatePowerUp" (Parameter: type, duration)
  
  → If type = "shield":
      - Set Player.IsInvincible to 1
      - Add shield sprite overlay on player
      - Wait [duration] seconds → Set IsInvincible to 0, remove overlay
  
  → If type = "speed":
      - Set ScrollSpeed to ScrollSpeed * 1.5
      - Wait [duration] → Reset ScrollSpeed
  
  → If type = "magnet":
      - Set magnet radius (Collectibles in range move toward player)
      - Wait [duration] → Disable magnet
  
  → If type = "multishot":
      - Set bullet count to 3 (spread angle)
      - Wait [duration] → Reset bullet count to 1
```

#### Group: UI_Updates
```
Every tick
  
  → Update DistanceTraveled based on ScrollSpeed
  → Set Text "TextDistance" to "Distance: " & floor(DistanceTraveled) & "m"
  → Set Text "TextScore" to "Score: " & GameScore
  → Set Text "TextCoins" to "Coins: " & CoinsCollected
  → Update Health bar sprites based on Player.HP
```

#### Group: Difficulty_Scaling
```
Every time DistanceTraveled crosses 500m threshold
  
  → Add 1 to DifficultyLevel
  → Increase ScrollSpeed by 5% (cap at 300)
  → Reduce spawn timer intervals
  → Play transition sound/VFX
```

#### Group: Pause_System
```
On "Pause" key pressed
  + GameState = "playing"
  
  → Set time scale to 0
  → Set GameState to "paused"
  → Show pause menu overlay
  → Pause music

On "Resume" clicked
  + GameState = "paused"
  
  → Set time scale to 1
  → Set GameState to "playing"
  → Hide pause menu
  → Resume music
```

---

### 4. EventSheet_GameOver

```
On start of layout
  
  → Play music "GameOver"
  → Display final score, distance, coins
  → Check if new high score
  → If new high score:
      - Update HighScores array
      - Show "NEW HIGH SCORE!" text

On "Retry" button clicked
  → Go to layout "GameplayLevel"

On "MainMenu" button clicked
  → Go to layout "MainMenu"
```

---

## 🎨 Visual Effects & Polish

### Particle Effects
**Use Particles object for:**
- Coin collect sparkles
- Mode switch swirl
- Enemy death explosion
- Power-up activation glow

**Settings example (Sparkle):**
```
Rate: 50
Spray cone: 360°
Speed: 100-200
Size: 10-20
Fade out: Yes
Lifetime: 0.5 seconds
```

### Tween Behaviors
**Use Tween for:**
- Button hover effects (scale 1.0 → 1.1)
- UI element slide in/out
- Health bar smooth decrease
- Score number pop effect

**Example:**
```
On mouse over Button
  → Button Tween "Scale" to 1.1 in 0.2 seconds (Ease: Out-Elastic)

On mouse out
  → Button Tween "Scale" to 1.0 in 0.2 seconds
```

### Camera Shake
```
Function "CameraShake" (Parameter: intensity, duration)
  
  → Repeat [duration * 60] times: (assuming 60fps)
      - Set Scroll X to OriginalX + random(-intensity, intensity)
      - Set Scroll Y to OriginalY + random(-intensity, intensity)
  → Reset to original position
```

Call on:
- Player taking damage (intensity: 10, duration: 0.3)
- Enemy death (intensity: 5, duration: 0.2)
- Mode switch (intensity: 15, duration: 0.4)

---

## 🔧 Optimization Tips

### Performance
1. **Destroy off-screen objects:**
   ```
   Every 1 second
     → For each EnemyFamily:
         - If Enemy.Y > Camera.Y + 200: Destroy Enemy
   ```

2. **Use object pooling** untuk frequently spawned objects (bullets, VFX)

3. **Limit particle count:**
   - Max particles per emitter: 50-100

4. **Disable unused behaviors:**
   - Disable Platform behavior saat Mode 2
   - Disable collisions untuk destroyed objects

### Memory Management
- Use sprite sheets (not individual files)
- Compress audio (OGG format, 128kbps)
- Downscale backgrounds untuk web export (max 2048px)

### Loading
- Enable "Preload sounds" untuk critical SFX
- Use "Loader layout" untuk loading screen jika perlu

---

## 🐛 Common Pitfalls & Solutions

### Issue: Player falls through platform setelah mode switch
**Solution:**
```
On mode switch to Mode 1:
  → Wait 0.1 seconds
  → Enable Platform behavior
  → Set collision enabled
```

### Issue: Spawning impossible patterns (obstacle + enemy bersamaan)
**Solution:**
- Implement spawn grid system
- Check collision before spawning
- Add minimum distance between spawns

### Issue: Bullets tidak destroy setelah keluar layar
**Solution:**
```
Every tick
  → PlayerBullet.Y < Camera.Y - 100: Destroy PlayerBullet
  → EnemyBullet.Y > Camera.Y + 100: Destroy EnemyBullet
```

### Issue: Power-up overlap (multiple active)
**Solution:**
- Use boolean flags untuk each power-up type
- Disable pickup while already active
- Extend timer jika pickup same type

---

## 📦 Export & Build

### HTML5 Export Settings
```
Minify script: ✅ Yes
Recompress images: ✅ Yes (PNG recompression)
Convert to WebP: ✅ Yes (better compression)
Deduplicate images: ✅ Yes
```

### Testing Checklist
- [ ] Test kedua mode switch fluidly
- [ ] No collision bugs
- [ ] Spawning tidak overlap/impossible
- [ ] Power-ups work correctly
- [ ] UI updates accurately
- [ ] Audio plays properly
- [ ] Game over triggers correctly
- [ ] Pause/Resume works
- [ ] Performance 60fps stable

---

## 📚 Useful Construct 3 Resources

- **Manual:** https://www.construct.net/en/make-games/manuals/construct-3
- **Tutorials:** https://www.construct.net/en/tutorials
- **Forum:** https://www.construct.net/en/forum
- **Discord:** Official Construct Community

---

**Last Updated**: 2026-09-12  
**Construct 3 Version**: r408+ (latest stable)
