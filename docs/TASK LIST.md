================================================================
 GAMEJAM – UNEXPECTED PAIR | PROGRAMMER TASK LIST
 Engine: Construct 3 Free | Genre: 2D Endless Runner
================================================================

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FASE 1 — FONDASI (Kerjakan ini dulu sebelum apapun)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[ ] 1.1  Buat 2 Layout terpisah dalam 1 scene
         - Layer "World_Human" (atas, visible by default)
         - Layer "World_Ghost" (bawah, hidden by default)
         - Gunakan parallax background berbeda untuk masing-masing

[ ] 1.2  Buat sprite placeholder untuk semua objek utama
         - PlayerHuman (sprite + animasi: idle, run, jump, attack)
         - PlayerGhost (sprite + animasi: idle, fly)
         - Platform_Solid (tidak bisa ditembus)
         - Platform_JumpThru (bisa dilewati dari bawah)
           → pakai behavior "Jump-thru" bawaan Construct 3
         - Catatan: gunakan 1 sprite per objek, beda animasi frame

[ ] 1.3  Setup kamera dan auto-scroll
         - Pasang behavior "Scroll To" pada kamera mengikuti Player
         - Buat variabel global ScrollSpeed (mulai dari nilai tetap)
         - Setiap objek dunia (platform, enemy, dll) bergerak ke kiri
           otomatis dengan: X -= ScrollSpeed * dt
         - Alternatif hemat event: pasang behavior "Bullet" ke kiri
           pada tiap objek, kecepatan = ScrollSpeed

[ ] 1.4  Mekanisme perpindahan dunia (World Switch)
         - Gunakan variable global: WorldState (nilai: "human"/"ghost")
         - Saat tombol Switch ditekan (saran: tombol F atau Q):
           → Jika WorldState = "human" → sembunyikan layer Human,
             tampilkan layer Ghost, set WorldState = "ghost"
           → Sebaliknya lakukan kebalikannya
         - PlayerHuman dan PlayerGhost selalu di posisi X yang sama,
           Y bisa berbeda (Human di atas, Ghost di bawah)
         - Hemat event: gunakan 1 kondisi + 1 else, jangan buat
           2 kondisi terpisah

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FASE 2 — MEKANIK PLAYER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[ ] 2.1  Player Manusia — Lompat
         - Pasang behavior "Platform" pada PlayerHuman
         - Space = lompat (sudah built-in di behavior Platform)
         - Set: gravity, jump strength, max speed sesuai feel game
         - Platform_JumpThru: pastikan behavior "Jump-thru" aktif

[ ] 2.2  Player Manusia — Attack
         - Saran tombol: Z atau tombol X
         - Saat tombol ditekan → mainkan animasi "attack"
         - Spawn objek HitboxAttack (invisible sprite) di depan player
           selama durasi animasi attack
         - HitboxAttack overlap enemy → kurangi HP enemy / destroy
         - Destroy HitboxAttack setelah animasi selesai

[ ] 2.3  Player Hantu — Hold Space untuk Terbang
         - JANGAN pakai behavior Platform untuk PlayerGhost
         - Pasang behavior "Custom Movement" atau gunakan physics manual
         - Logika:
           → Setiap tick: PlayerGhost.Y += GravityGhost * dt
             (ghost terus turun jika tidak ditekan)
           → Selama Space di-hold: PlayerGhost.Y -= FlyForce * dt
             (ghost naik)
           → Clamp posisi Y agar tidak keluar layar (atas/bawah)
         - Ini mirip persis mekanik Geometry Dash

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FASE 3 — SISTEM GAME
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[ ] 3.1  Health Bar
         - Buat variabel global: PlayerHP (misal max = 5)
         - Buat sprite HealthBar atau gunakan object "Bar" (Construct 3
           punya plugin bar bawaan)
         - Saat player kena damage: PlayerHP -= 1
         - Saat PlayerHP <= 0: picu Game Over
         - Tambahkan cooldown invincibility setelah kena hit
           (variable InvincibleTimer, hitung mundur dengan dt)

[ ] 3.2  Sistem Skor — Koin
         - Buat sprite Coin dengan animasi berputar
         - Tambahkan variable global: CoinCount
         - Saat PlayerHuman overlap Coin → CoinCount += 1, destroy Coin
         - Catatan: Coin hanya muncul di dunia manusia (layer Human)

[ ] 3.3  Sistem Skor — Jarak
         - Tambahkan variable global: DistanceScore
         - Setiap tick: DistanceScore += ScrollSpeed * dt
         - Tampilkan di HUD sebagai angka (bisa dibagi konstanta
           agar satuannya terasa "meter" bukan piksel)

[ ] 3.4  HUD (tampilkan semua info player)
         - Buat layer "HUD" paling atas, tidak ikut scroll
         - Tampilkan: HP, CoinCount, DistanceScore, WorldState indicator

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FASE 4 — ENEMY & OBSTACLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 Kerjakan satu per satu, test tiap selesai sebelum lanjut

[ ] 4.1  Obstacle — Spike (dunia manusia)
         - Sprite statis, tidak bergerak sendiri (ikut scroll otomatis)
         - Overlap PlayerHuman → damage

[ ] 4.2  Obstacle — Bomb (dunia manusia)
         - Sprite Bomb, bisa dilompati player
         - Jika PlayerHuman overlap atau terlalu dekat (jarak < X px)
           → damage
         - Bisa dibuat lebih visual: tambah radius danger zone

[ ] 4.3  Obstacle — Gedung / Trigger Pindah Dunia (dunia manusia)
         - Sprite gedung tinggi yang menghalangi jalur
         - Overlap PlayerHuman → paksa world switch ke ghost
           (panggil logika yang sama dengan 1.4)
         - Setelah melewati gedung → player bisa kembali manual

[ ] 4.4  Obstacle — Ghost dari arah berlawanan (dunia hantu)
         - Spawn ghost di ujung kanan layar
         - Pasang behavior "Bullet" dengan arah ke kiri
         - Overlap PlayerGhost → damage

[ ] 4.5  Enemy — Manusia Tipe 1 (serang langsung)
         - Pasang behavior "Platform" agar berdiri di platform
         - Saat PlayerHuman dalam jangkauan X (misal < 150px):
           → langsung spawn HitboxEnemy ke arah player
         - Kena serangan player → destroy / kurangi HP enemy

[ ] 4.6  Enemy — Manusia Tipe 2 (ancang-ancang dulu)
         - Sama seperti Tipe 1, tapi tambahkan state machine sederhana
           menggunakan instance variable "State" (nilai: "idle",
           "windup", "attack")
         - Idle → deteksi player → State = "windup" → tunggu X detik
           (pakai timer atau countdown variable) → State = "attack"
           → spawn hitbox
         - Ini hemat event jika dibuat sebagai 1 enemy sprite dengan
           animasi berbeda per state

[ ] 4.7  Enemy — Hantu Pemanah Lurus (dunia hantu)
         - Saat PlayerGhost dalam jarak tertentu → spawn proyektil
         - Proyektil: behavior "Bullet" lurus ke kiri
         - Overlap PlayerGhost → damage

[ ] 4.8  [OPSIONAL / LOW PRIORITY] Enemy — Hantu Panah Homing
         - Sama seperti 4.7, tapi setiap tick arahkan proyektil
           ke posisi PlayerGhost menggunakan:
           Bullet.SetAngleOfMotion(angle(Bullet.X, Bullet.Y,
           PlayerGhost.X, PlayerGhost.Y))
         - Tambahkan kondisi: hanya homing jika jarak > threshold
         - Kerjakan ini terakhir atau skip jika waktu tidak cukup

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FASE 5 — PROCEDURAL GENERATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[ ] 5.1  Spawn trigger berbasis jarak / posisi kamera
         - Buat objek invisible "SpawnPoint" di ujung kanan layar
         - Setiap X pixel kamera bergerak → spawn chunk baru

[ ] 5.2  Buat beberapa "chunk" template (platform + obstacle combo)
         - Minimal 3–5 kombinasi berbeda agar tidak repetitif
         - Setiap chunk: pilih secara random saat di-spawn
         - Gunakan: pick random instance atau random(1,5) untuk
           memilih tipe chunk

[ ] 5.3  Destroy objek yang sudah melewati batas kiri layar
         - Condition: Object.X < Camera.X - LayoutWidth/2 - margin
         - Destroy → hemat memory, penting untuk endless runner

[ ] 5.4  Tingkatkan kesulitan seiring waktu
         - Setiap N detik atau N jarak: ScrollSpeed += increment
         - Bisa juga tingkatkan spawn rate enemy

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CATATAN PENTING — CONSTRUCT 3 FREE (event terbatas)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

- Gunakan instance variable pada sprite untuk state (HP, state AI,
  timer) daripada banyak variabel global terpisah → hemat event
- Gunakan 1 sprite dengan banyak animasi daripada banyak sprite
  berbeda (PlayerHuman 1 sprite: idle/run/jump/attack)
- Hindari nested condition terlalu dalam — pecah jadi function
  (gunakan fitur "Function" di event sheet)
- Enemy AI: buat sesederhana mungkin, cukup dengan 2–3 event per
  tipe enemy
- Prioritas jika waktu mepet: selesaikan Fase 1–3 dulu, Fase 4
  minimal spike + 1 enemy manusia + ghost penghalang, Fase 5 bisa
  pakai manual level dulu

================================================================