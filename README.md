# Unexpected Pair - Vertical Endless Runner

Game vertical endless runner dengan tema **Alam Manusia x Alam Hantu** untuk Game Jam Internal.

## 🎮 Konsep Game

Pemain berlari vertikal ke atas sambil menghindari obstacle dan musuh. Game memiliki dua mode yang bisa di-switch:

### Mode 1: Running (Alam Manusia)
- Lari vertikal normal
- **Jump** untuk melompat
- **Melee Attack** untuk menyerang jarak dekat

### Mode 2: Flying/Floating (Alam Hantu)
- Melayang seperti Jetpack Joyride / Geometry Dash ship mode
- **Hold** = turun, **Release** = naik
- **Ranged Attack** (shoot bullets)

## 🎯 Game Elements

- **Coins**: Collectible untuk score
- **Enemies**: Musuh yang harus dihindari/diserang
- **Obstacles**: Rintangan di kedua mode
- **Health Bar**: HP player
- **Distance Tracker**: Penanda jarak yang sudah ditempuh
- **Power-up Bubbles**: Power-up khusus

## 🛠️ Tech Stack

- **Engine**: Construct 3
- **Version Control**: GitHub

## 📁 Struktur Folder

```
GameJamInternal/
├── assets/
│   ├── sprites/
│   │   ├── player/          # Sprite player untuk kedua mode
│   │   ├── enemies/         # Sprite musuh
│   │   ├── obstacles/       # Sprite obstacle
│   │   ├── collectibles/    # Coin, power-ups
│   │   ├── ui/              # Health bar, distance tracker, buttons
│   │   ├── backgrounds/     # Background alam manusia & hantu
│   │   └── effects/         # VFX, particles
│   ├── sounds/
│   │   ├── music/           # BGM
│   │   └── sfx/             # Sound effects
│   └── fonts/               # Custom fonts
├── docs/                    # Design documents, notes
├── exports/                 # Build exports (HTML5, etc)
└── backups/                 # Manual backups (git-ignored)
```

## 🎨 Referensi

- **Split Fiction** - Dual world mechanics
- **Jetpack Joyride** - Flying/floating controls
- **Geometry Dash** - Ship mode controls

## 👥 Tim

[Tambahkan nama anggota tim di sini]

## 📝 Development Notes

Project ini menggunakan Construct 3. File `.c3p` adalah main project file yang akan di-commit ke repository.

---

**Started**: 2026-09-12
