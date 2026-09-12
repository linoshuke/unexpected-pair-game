# Panduan Programming — Bagian World_Human
### Gamejam "Unexpected Pair" | Construct 3 Free | 2D Endless Runner (Pixel Art)

Panduan ini fokus ke bagian yang kamu pegang: **dunia manusia (World_Human)** —
sprite, mekanik player, obstacle, enemy, dan skor yang berjalan di layer Human.
Bagian yang sifatnya "shared" (kamera, world switch, HP) tetap dijelaskan karena
kamu perlu tahu cara kerjanya untuk koordinasi dengan partner yang pegang
World_Ghost.

Urutan di bawah **sengaja mengikuti urutan Fase di task list** — kerjakan dari
atas ke bawah, jangan loncat, karena tiap langkah jadi fondasi langkah
berikutnya.

---

## 0. Setup Project (sekali di awal)

1. Buat project baru → pilih ukuran layout, contoh **480 x 270 px** (rasio 16:9,
   kelipatan gampang buat pixel art) lalu di preview zoom-in otomatis.
2. Di **Project Properties**:
   - `Pixel rounding` → **On**
   - `Sampling` → **Nearest** (jangan Bilinear, biar pixel art tidak blur)
   - `Downscaling quality` → **Low (fastest)**
   - `Fullscreen scaling` → **Integer scale** (opsional, biar rasio pixel selalu rapi)
3. Sepakati dengan partner ukuran grid sprite, contoh: **1 tile = 16px** atau
   **32px**. Semua sprite (platform, player, enemy) kelipatan angka ini biar
   nyambung secara visual antar dunia.
4. Buat folder terpisah di panel Project: `Sprites_Human`, `Sprites_Ghost`,
   `Sprites_Shared`, `EventSheets`, `Functions`.

### ⚠️ Batasan Construct 3 Free (wajib dibaca sebelum lanjut)

Free plan cuma kasih **2 layer per layout** dan **±50 event per project**
(untuk seluruh game, bukan per-orang), dan **tidak ada fitur Families**
(grouping banyak object type jadi satu, itu fitur berbayar). Ini mengubah
beberapa rencana di bawah dibanding versi awal:

- **Tidak ada layer terpisah `World_Human` dan `World_Ghost`.** Kalian
  berdua **berbagi 1 layer gameplay** yang sama (semua objek Human & Ghost
  hidup di layer yang sama), dan 1 layer sisanya dipakai untuk `HUD`.
- **Tidak pakai Families.** Kalau butuh event yang berlaku untuk banyak tipe
  objek sekaligus, tulis event terpisah per object type, atau — lebih hemat —
  taruh logikanya di 1 **Function** yang dipanggil dari masing-masing objek.
- **50 event itu SANGAT sedikit** untuk fitur selengkap task list ini, dan
  itu jatah bersama kamu + partner. Wajib disiplin prioritas (lihat bagian
  "Ringkasan Urutan Kerja" di akhir) dan cek dengan partner: apakah kampus
  punya lisensi edukasi Construct 3 (biasanya event-nya jadi unlimited), atau
  pakai trial 7 hari offline saat deadline dekat.

---

## FASE 1 — Fondasi

### 1.1 Layer (revisi: cuma 2 layer)

Karena jatah Free cuma 2 layer, susunannya:

1. Layer `Game` (bawah) — **semua objek gameplay ada di sini**: platform,
   PlayerHuman, PlayerGhost, obstacle, enemy, coin, background — Human
   *dan* Ghost, dicampur jadi satu layer. Parallax **100/100**.
2. Layer `HUD` (atas) — Text HP/Coin/Distance/WorldState. Parallax **0/0**
   (biar diam, tidak ikut scroll).

Tidak ada lagi layer `World_Human`/`World_Ghost` terpisah — pemisahan dunia
Human vs Ghost dilakukan lewat **posisi Y**, bukan lewat layer (ini justru
persis sesuai instruksi asli di 1.4: "Human di atas, Ghost di bawah").
Contoh pembagian band vertikal kalau layout tinggi 270px:
- Band Human: Y sekitar 20–140 (platform, spike, bomb, enemy manusia di sini)
- Band Ghost: Y sekitar 140–260 (partner yang urus)

Karena band-nya terpisah secara Y, platform/obstacle Human otomatis tidak
akan overlap dengan PlayerGhost meskipun sama-sama ada di 1 layer yang sama —
jadi tidak perlu hide/show apapun untuk objek dunia, cukup untuk 2 sprite
player-nya saja (lihat 1.4).

Kalau mau background sedikit parallax (kesan kedalaman), bikin 1 sprite
`Background` besar, taruh di layer `Game` juga, lalu **Send to Back** lewat
Z Order Bar (bukan layer baru) — cukup 1 kecepatan scroll, tidak usah beda
parallax, supaya hemat layer.

### 1.2 Sprite Placeholder (pixel art)

**PlayerHuman**
- Sprite baru, ukuran kanvas per-frame konsisten, contoh 32x32 atau 32x48.
- Animasi minimal 4: `Idle`, `Run`, `Jump`, `Attack`.
  - Idle: 2-4 frame, speed rendah (3-5 fps) biar tidak "gelisah".
  - Run: 4-6 frame, speed lebih tinggi (10-12 fps).
  - Jump: cukup 2-3 frame (naik, puncak, turun), sering di-kontrol manual lewat
    event (tidak auto-loop), bukan berdasar speed.
  - Attack: 3-5 frame, jangan set "loop", karena harus berhenti di frame
    terakhir lalu balik ke Idle lewat event.
- Import: gambar per-frame lalu drag ke sprite editor, atau import satu
  spritesheet dan pakai fitur **Sprite → Import frames from sprite strip**.

**Platform_Solid**
- Sprite/Tiled Background sederhana, ukuran kelipatan grid tile kalian.
- Tambahkan behavior **Solid**.

**Platform_JumpThru**
- Sama seperti di atas tapi behavior-nya **Jump-thru** (built-in Construct 3),
  bukan Solid. Ini yang bikin player bisa lompat menembus dari bawah tapi
  berdiri di atasnya.

> Tips hemat sprite (sesuai catatan task list): jangan buat sprite terpisah
> untuk tiap ukuran platform. Cukup 1 sprite Platform_Solid dan 1
> Platform_JumpThru, lalu ubah **Width** instance-nya di layout untuk variasi
> panjang.

### 1.3 Kamera & Auto-scroll

Ini kerja bareng partner, tapi implementasinya kamu perlu tahu karena semua
objek Human ikut aturan yang sama.

- Buat objek kosong (atau pakai PlayerHuman langsung) sebagai target kamera,
  pasang behavior **Scroll To**.
- Variabel global: `ScrollSpeed` (number, nilai awal misal `150`).
- Untuk tiap objek dunia (platform, obstacle, enemy) yang **tidak dikontrol
  behavior lain**, gerakkan tiap tick:

```
Event: System → Every tick
Action: [Object] Set X to [Object].X - ScrollSpeed * dt
```

- Alternatif hemat event (disarankan task list): pasang behavior **Bullet**
  dengan **Angle of motion = 180°** dan **Speed = ScrollSpeed**. Lebih hemat
  karena tidak perlu event manual per objek.

> Catatan: PlayerHuman **tidak** perlu ikut auto-scroll manual — dia diam di
> posisi X tetap (kamera yang gerak menjelasi dunia lewat Scroll To), sedangkan
> platform/obstacle/enemy yang bergerak ke kiri.

### 1.4 World Switch (revisi: toggle objek, bukan toggle layer)

Karena tidak ada layer terpisah dan tidak ada Families, switch dunia
dilakukan dengan toggle **visible + behavior aktif** pada 2 sprite player
saja (PlayerHuman, PlayerGhost) — bukan pada seluruh layer. Cukup 2 objek,
jadi tidak butuh "For each", hemat event.

Variabel global `WorldState` (text, default `"human"`).

```
Function: Fn_WorldSwitch
  Condition: WorldState = "human"
    Action: PlayerHuman → Set invisible
    Action: PlayerHuman → Platform behavior → Disable
    Action: PlayerGhost → Set visible
    Action: PlayerGhost → [behavior terbang, lihat 2.3] → Enable
    Action: Set WorldState to "ghost"
  Else
    Action: PlayerGhost → Set invisible
    Action: PlayerGhost → [behavior terbang] → Disable
    Action: PlayerHuman → Set visible
    Action: PlayerHuman → Platform behavior → Enable
    Action: Set WorldState to "human"

Event: Keyboard → On F pressed
  Action: Call Fn_WorldSwitch
```

Panggil `Fn_WorldSwitch` yang sama dari tombol F maupun dari obstacle Gedung
(4.3) — jangan tulis ulang logikanya, ini yang bikin hemat event sesuai
catatan di task list.

PlayerHuman dan PlayerGhost harus selalu **X sama**, hanya **Y berbeda**
(Human di band atas, Ghost di band bawah — lihat pembagian band di 1.1).
Cara paling gampang: setiap tick, set `PlayerGhost.X = PlayerHuman.X` (pilih
satu sebagai "master" X, biasanya PlayerHuman, supaya tidak saling tarik).

> Kenapa objek dunia (platform/obstacle Human) tidak perlu ikut di-hide:
> karena posisinya sudah di band Y yang berbeda dari band Ghost, dan
> PlayerHuman yang di-invisible + behavior-nya di-disable otomatis tidak
> akan collide dengan apapun. Efeknya sama seperti "dunia disembunyikan",
> tanpa butuh layer atau Family tambahan.

---

## FASE 2 — Mekanik PlayerHuman

### 2.1 Lompat

1. Pasang behavior **Platform** ke PlayerHuman.
2. Di Properties behavior Platform, titik awal yang bisa dicoba:
   - Max Speed: `200`
   - Acceleration: `1500`
   - Deceleration: `1500`
   - Jump strength: `500`
   - Gravity: `1500`
   - Max Fall Speed: `600`
3. Space untuk lompat sudah otomatis ter-handle oleh behavior Platform (default
   Jump = Space), tidak perlu bikin event tambahan kecuali mau custom kontrol.
4. Event animasi sederhana:

```
Event: PlayerHuman → Is on floor AND Vector X = 0
  Action: Set animation "Idle"
Event: PlayerHuman → Is on floor AND Vector X ≠ 0
  Action: Set animation "Run"
Event: PlayerHuman → Is jumping OR Is falling
  Action: Set animation "Jump"
```

### 2.2 Attack

1. Buat sprite `HitboxAttack`, invisible (opacity 0 atau centang Invisible),
   tanpa behavior gerak — posisinya di-pin ke PlayerHuman.
2. Instance variable di PlayerHuman: `IsAttacking` (boolean).

```
Event: Keyboard → On Z pressed
  Condition: PlayerHuman.IsAttacking = false
  Action: Set IsAttacking = true
  Action: PlayerHuman → Set animation to "Attack"
  Action: Spawn HitboxAttack di depan PlayerHuman
          (X = PlayerHuman.X + (40 if Mirrored=false else -40))

Event: HitboxAttack → On collision with EnemyHuman
  Action: EnemyHuman → Subtract 1 from HP
  Action: HitboxAttack → Destroy

Event: PlayerHuman → On "Attack" animation finished
  Action: Set IsAttacking = false
  Action: PlayerHuman → Set animation to "Idle"
  Action: HitboxAttack → Destroy (jika masih ada)
```

---

## FASE 3 — Sistem Game (bagian Human)

### 3.1 Health Bar

- Variabel global `PlayerHP` (number, default `5`), `InvincibleTimer` (number,
  default `0`).
- Objek HUD: gunakan plugin bawaan **Bar** atau sprite bar manual di layer HUD.

```
Event: System → Every tick
  Condition: InvincibleTimer > 0
    Action: Subtract dt from InvincibleTimer

Event: PlayerHuman → On collision with [Obstacle/Enemy]
  Condition: InvincibleTimer <= 0
    Action: Subtract 1 from PlayerHP
    Action: Set InvincibleTimer to 1.5
    Action: PlayerHuman → Set opacity 50 (efek kedip, optional)

Event: System → PlayerHP <= 0
  Action: [trigger Game Over — pindah layout / show panel Game Over]
```

Karena PlayerHuman dan PlayerGhost adalah "1 nyawa yang sama", pakai satu
variabel global `PlayerHP` untuk keduanya (bukan variabel terpisah per dunia).

### 3.2 Sistem Skor — Koin

- Sprite `Coin`, animasi berputar (loop, 4-6 frame).
- **Hanya taruh instance Coin di layer World_Human** (sesuai catatan task
  list — koin memang cuma ada di dunia manusia).
- Variabel global `CoinCount` (default 0).

```
Event: PlayerHuman → On collision with Coin
  Action: Add 1 to CoinCount
  Action: Coin → Destroy
```

### 3.4 HUD (bagian yang kamu isi)

Di layer `HUD` (parallax 0,0, tidak ikut scroll), buat Text object untuk:
- `Text_HP` → `Set text to "HP: " & PlayerHP`
- `Text_Coin` → `Set text to "Coin: " & CoinCount`
- `Text_Distance` → `Set text to "Jarak: " & round(DistanceScore/50) & "m"`
  (dibagi konstanta biar terasa satuan meter, bukan pixel mentah)
- Indicator kecil `WorldState` (bisa icon sprite yang ganti frame sesuai
  `WorldState = "human"` / `"ghost"`)

Update semua teks ini di 1 event `Every tick` biar sederhana, jangan bikin
event terpisah per Text object kalau memungkinkan.

---

## FASE 4 — Enemy & Obstacle Dunia Manusia

Kerjakan urut, test tiap satu sebelum lanjut ke berikutnya.

### 4.1 Spike

- Sprite statis, tanpa behavior gerak sendiri (auto ikut scroll global via
  Bullet/manual seperti di 1.3).

```
Event: PlayerHuman → On collision with Spike
  Condition: InvincibleTimer <= 0
    Action: [pakai logika damage yang sama dari 3.1]
```

### 4.2 Bomb

- Sprite `Bomb`, bisa dilompati.
- Opsi jarak (lebih visual daripada overlap polos):

```
Event: System → Every tick
  Condition: distance(PlayerHuman.X, PlayerHuman.Y, Bomb.X, Bomb.Y) < 30
    Action: [damage, sama seperti Spike]
```

- Optional: tambahkan sprite radius transparan merah sebagai indikator
  "danger zone" di bawah Bomb.

### 4.3 Gedung / Trigger Pindah Dunia

- Sprite gedung tinggi, **Solid** (menghalangi jalur, memaksa player harus
  ganti dunia untuk lewat).

```
Event: PlayerHuman → On collision with Gedung
  Condition: WorldState = "human"
    Action: Call Function Fn_WorldSwitch
```

Karena logikanya sudah dibungkus jadi Function di langkah 1.4, di sini kamu
tinggal panggil, tidak perlu tulis ulang.

### 4.5 Enemy Manusia Tipe 1 (serang langsung)

- Pasang behavior **Platform** ke `EnemyHuman1` supaya berpijak di platform
  seperti player.
- Instance variable: `HP` (default 2).

```
Event: System → Every tick
  Condition: distance(EnemyHuman1.X, EnemyHuman1.Y, PlayerHuman.X, PlayerHuman.Y) < 150
    Action: Spawn HitboxEnemy ke arah PlayerHuman
Event: EnemyHuman1.HP <= 0
    Action: EnemyHuman1 → Destroy (+ spawn efek/pickup jika mau)
```

### 4.6 Enemy Manusia Tipe 2 (ancang-ancang)

- Instance variable: `State` (text: `"idle"` / `"windup"` / `"attack"`),
  `WindupTimer` (number).

```
Event: EnemyHuman2.State = "idle"
  Condition: distance(...) < 150
    Action: Set State to "windup"
    Action: Set WindupTimer to 1.0
    Action: Set animation to "Windup"

Event: EnemyHuman2.State = "windup"
  Action: Subtract dt from WindupTimer
Event: EnemyHuman2.State = "windup" AND WindupTimer <= 0
    Action: Set State to "attack"
    Action: Spawn HitboxEnemy
    Action: Set animation to "Attack"

Event: EnemyHuman2 → On "Attack" animation finished
    Action: Set State to "idle"
    Action: Set animation to "Idle"
```

Satu sprite `EnemyHuman2` dengan animasi berbeda per state — jangan bikin
sprite terpisah untuk tiap fase serangan (hemat event & aset).

---

## FASE 5 — Procedural Generation (Chunk Dunia Manusia)

1. Objek invisible `SpawnPoint`, posisinya selalu di ujung kanan layar
   relatif kamera (`Camera.X + LayoutWidth/2 + margin`).
2. Variabel global `DistanceSinceSpawn` — setiap kali melewati threshold jarak
   tertentu (misal tiap 400px), spawn chunk baru lalu reset counter.
3. Buat minimal 3-5 layout chunk manual sebagai referensi (kombinasi
   platform + spike/bomb + kadang enemy), lalu di event pakai:

```
Event: System → DistanceSinceSpawn >= 400
  Action: Set DistanceSinceSpawn to 0
  Action: Set ChunkType to int(random(1, 6))  // hasil 1-5
Event: ChunkType = 1
  Action: Create objects sesuai chunk 1 (posisi relatif SpawnPoint.X)
Event: ChunkType = 2
  Action: Create objects sesuai chunk 2
  ... dst
```

4. **Destroy objek di luar layar** (wajib untuk endless runner, hemat memory):

```
Event: System → For each [Platform/Obstacle/Enemy/Coin]
  Condition: [Object].X < Camera.X - LayoutWidth/2 - 100
    Action: [Object] → Destroy
```

5. Tingkatkan kesulitan seiring waktu:

```
Event: System → Every 10 seconds
  Action: Add 10 to ScrollSpeed
```

---

## Ringkasan Urutan Kerja (checklist kamu)

- [ ] 0. Setup project (resolusi, pixel setting, grid) + sepakati pembagian
      band Y Human/Ghost dan budget event (±50 total, bagi dua)
- [ ] 1.1 Layer `Game` (gabungan Human+Ghost) + layer `HUD` — cuma 2 layer
- [ ] 1.2 Sprite PlayerHuman, Platform_Solid, Platform_JumpThru
- [ ] 1.3 Koordinasi ScrollSpeed & auto-scroll dengan partner
- [ ] 1.4 Koordinasi Fn_WorldSwitch (toggle objek player) dengan partner
- [ ] 2.1 Lompat
- [ ] 2.2 Attack
- [ ] 3.1 Health bar (shared variable PlayerHP)
- [ ] 3.2 Koin
- [ ] 3.4 HUD (Text HP/Coin/Distance/WorldState)
- [ ] 4.1 Spike → 4.2 Bomb → 4.3 Gedung → 4.5 Enemy Tipe 1 → 4.6 Enemy Tipe 2
      (test satu-satu, jangan sekaligus)
- [ ] 5. Chunk dunia manusia + destroy off-screen + difficulty scaling

**Variabel global yang harus disepakati bareng partner World_Ghost** (jangan
sampai kalian berdua bikin dobel): `WorldState`, `ScrollSpeed`, `PlayerHP`,
`InvincibleTimer`, `CoinCount`, `DistanceScore`.

Kalau waktu mepet, prioritas sesuai catatan task list: selesaikan Fase 1-3
dulu sampai jalan, baru Fase 4 minimal Spike + Enemy Tipe 1, Fase 5 boleh
pakai level manual dulu (tanpa procedural generation).
