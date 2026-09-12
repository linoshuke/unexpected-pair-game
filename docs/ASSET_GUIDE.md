# Asset Organization Guide

Panduan untuk mengorganisir asset dalam folder structure project.

---

## 📁 Sprites (`assets/sprites/`)

### `player/`
Sprite untuk karakter player di kedua mode.

**File naming convention:**
```
player_mode1_idle.png
player_mode1_run_01.png ... player_mode1_run_08.png
player_mode1_jump.png
player_mode1_attack_01.png ... player_mode1_attack_03.png

player_mode2_idle.png
player_mode2_float_01.png ... player_mode2_float_04.png
player_mode2_shoot.png

player_transition_01.png ... player_transition_05.png
```

**Recommended specs:**
- Resolution: 64x64 atau 128x128 px
- Format: PNG dengan transparency
- Sprite sheet atau individual frames (sesuai workflow tim)

---

### `enemies/`
Sprite untuk semua enemy types.

**Mode 1 Enemies:**
```
enemy_groundwalker_idle.png
enemy_groundwalker_walk_01.png ... _04.png
enemy_bat_fly_01.png ... _04.png
enemy_charger_idle.png
enemy_charger_charge_01.png ... _03.png
```

**Mode 2 Enemies:**
```
enemy_ghost_float_01.png ... _04.png
enemy_shooter_idle.png
enemy_shooter_attack.png
enemy_phase_01.png ... _03.png
```

**Death animations:**
```
[enemy_name]_death_01.png ... _05.png
```

**Specs:**
- Resolution: 32x32 hingga 64x64 px
- Consistent art style
- Clear silhouette untuk readability

---

### `obstacles/`
Static dan animated obstacles.

**Mode 1:**
```
obstacle_spike_wall.png
obstacle_moving_platform.png
obstacle_falling_rock_01.png ... _03.png
```

**Mode 2:**
```
obstacle_energy_barrier.png
obstacle_rotating_blade_01.png ... _04.png
obstacle_portal_zone_01.png ... _06.png
```

**Universal:**
```
obstacle_gap_edge.png
```

---

### `collectibles/`
Coins, power-ups, dan pickup items.

```
coin_mode1.png (or coin_mode1_01.png ... _06.png for animation)
coin_mode2.png

powerup_shield.png
powerup_speedboost.png
powerup_magnet.png
powerup_multishot.png
powerup_bubble.png (wrapper/container untuk power-ups)
```

**Specs:**
- Resolution: 32x32 px
- Bright colors untuk visibility
- Subtle animation loop

---

### `ui/`
UI elements dan HUD components.

```
ui_heart_full.png
ui_heart_half.png
ui_heart_empty.png
ui_healthbar_frame.png
ui_healthbar_fill.png

ui_coin_icon.png
ui_score_panel.png
ui_distance_panel.png

ui_button_play.png
ui_button_pause.png
ui_button_resume.png
ui_button_restart.png
ui_button_settings.png
ui_button_quit.png

ui_mode_indicator_mode1.png
ui_mode_indicator_mode2.png

ui_powerup_timer_frame.png
ui_powerup_timer_fill.png
```

**Specs:**
- Clean, readable design
- High contrast untuk visibility
- Support light & dark backgrounds

---

### `backgrounds/`
Background layers untuk parallax scrolling.

**Mode 1 (Alam Manusia):**
```
bg_mode1_layer1_far.png      (paling jauh, slowest scroll)
bg_mode1_layer2_mid.png       (mid-ground)
bg_mode1_layer3_near.png      (foreground, fastest scroll)
bg_mode1_ground.png           (ground/floor texture)
```

**Mode 2 (Alam Hantu):**
```
bg_mode2_layer1_far.png
bg_mode2_layer2_mid.png
bg_mode2_layer3_near.png
bg_mode2_ambient.png          (ethereal effects)
```

**Transition:**
```
bg_transition_overlay.png     (VFX overlay saat switch)
```

**Specs:**
- Tileable horizontal (seamless loop)
- Resolution: Minimal 1920x1080 untuk layer
- Optimize file size (compress untuk web)

---

### `effects/`
VFX, particles, dan visual effects.

```
vfx_mode_switch_01.png ... _08.png
vfx_melee_slash_01.png ... _03.png
vfx_bullet_projectile.png
vfx_bullet_impact.png

vfx_coin_collect_01.png ... _05.png
vfx_powerup_activate_01.png ... _06.png
vfx_damage_flash.png
vfx_death_explosion_01.png ... _08.png

particle_dust.png
particle_sparkle.png
particle_smoke.png
particle_spirit.png (untuk Mode 2)
```

**Specs:**
- Small resolution (16x16 to 32x32)
- Additive blending recommended
- Transparent backgrounds

---

## 🔊 Sounds (`assets/sounds/`)

### `music/`
Background music tracks.

```
music_main_menu.ogg
music_gameplay.ogg
music_gameplay_intense.ogg (untuk high speed/difficulty)
music_game_over.ogg
```

**Specs:**
- Format: OGG Vorbis (best for web)
- Bitrate: 128-192 kbps
- Loopable (seamless loop point)
- Length: 1-3 minutes

---

### `sfx/`
Sound effects untuk actions.

**Player Actions:**
```
sfx_mode_switch.wav
sfx_jump.wav
sfx_melee_attack.wav
sfx_shoot.wav
sfx_player_hit.wav
sfx_player_death.wav
```

**Enemies:**
```
sfx_enemy_hit.wav
sfx_enemy_death.wav
sfx_enemy_shoot.wav
```

**Collectibles:**
```
sfx_coin_collect.wav
sfx_powerup_collect.wav
sfx_powerup_activate.wav
sfx_powerup_end.wav
```

**Obstacles:**
```
sfx_obstacle_hit.wav
sfx_spike_damage.wav
```

**UI:**
```
sfx_button_click.wav
sfx_button_hover.wav
sfx_pause.wav
sfx_unpause.wav
```

**Specs:**
- Format: WAV atau OGG
- Sample rate: 44.1kHz
- Mono untuk SFX (stereo untuk music)
- Short duration (0.1-1 second untuk SFX)

---

## 🔤 Fonts (`assets/fonts/`)

```
game_font_main.ttf/.woff      (untuk UI text, score, distance)
game_font_title.ttf/.woff     (untuk title screens)
```

**Recommendations:**
- Readable font untuk HUD (sans-serif)
- Stylized font untuk title (sesuai tema)
- Web-safe formats (WOFF/WOFF2 untuk HTML5 export)

---

## 📝 General Guidelines

### Naming Conventions
- **Lowercase** dengan **underscore** separator
- Format: `[category]_[name]_[state/frame].extension`
- Contoh: `enemy_bat_fly_03.png`

### File Organization
- Group by function, bukan by visual style
- Sprite sheets OK jika memudahkan workflow
- Keep source files (.psd, .ai) di folder terpisah (tidak di commit)

### Optimization
- Compress PNG tanpa quality loss (gunakan TinyPNG, PNGCrush)
- Trim transparent pixels
- Use power-of-2 dimensions untuk texture performance (32, 64, 128, 256)
- Batch export dengan consistent settings

### Version Control
- Commit final assets only
- Gunakan Git LFS untuk large files (optional)
- Update `.gitignore` jika ada temp/cache files dari art tools

---

## 🎨 Art Style Reference

### Color Palette

**Mode 1 (Alam Manusia):**
- Warm tones: `#FF6B35`, `#F7931E`, `#FDC830`
- Earthy: `#8B4513`, `#228B22`, `#87CEEB`

**Mode 2 (Alam Hantu):**
- Cool tones: `#9B59B6`, `#3498DB`, `#1ABC9C`
- Ethereal: `#E6E6FA`, `#00CED1`, `#8A2BE2`

### Contrast Requirements
- High contrast untuk gameplay clarity
- Enemies harus clearly visible on backgrounds
- UI harus readable di semua conditions

---

## ✅ Asset Checklist

Gunakan checklist ini untuk tracking progress asset:

### Player
- [ ] Mode 1: Idle, Run, Jump, Attack animations
- [ ] Mode 2: Idle, Float, Shoot animations
- [ ] Transition animation

### Enemies (minimal 3 types)
- [ ] Enemy 1: Idle, Movement, Attack, Death
- [ ] Enemy 2: Idle, Movement, Attack, Death
- [ ] Enemy 3: Idle, Movement, Attack, Death

### Obstacles (minimal 4 types)
- [ ] Obstacle 1 (static)
- [ ] Obstacle 2 (animated)
- [ ] Obstacle 3 (mode-specific)
- [ ] Obstacle 4 (universal)

### Collectibles
- [ ] Coin (both modes)
- [ ] Power-ups (3-4 types)

### UI
- [ ] Health bar
- [ ] Score/Distance panels
- [ ] Buttons (Play, Pause, Restart, etc)
- [ ] Mode indicator

### Backgrounds
- [ ] Mode 1 parallax layers (3 layers min)
- [ ] Mode 2 parallax layers (3 layers min)

### VFX
- [ ] Mode switch effect
- [ ] Attack effects
- [ ] Damage effects
- [ ] Collectible effects

### Audio
- [ ] Music (Main Menu, Gameplay, Game Over)
- [ ] SFX (minimal 10 essential sounds)

---

**Last Updated**: 2026-09-12
