# Dokumen Desain: Cat Clicker

## Overview

Cat Clicker adalah aplikasi web single-page interaktif yang dikemas dalam satu file `index.html`. Pengguna mengklik kucing untuk mendapatkan reaksi acak yang lucu, sambil melacak jumlah klik. Teknologi yang digunakan:

- **HTML5** — struktur halaman semantik
- **Tailwind CSS via CDN** — utility-first styling tanpa build step
- **Vanilla JavaScript** — logika interaksi dan state management
- **Satu file** — `index.html` berisi semua HTML, CSS kustom, dan JavaScript

Aplikasi ini bersifat stateless (tidak ada persistensi ke server atau localStorage) dan berjalan sepenuhnya di browser tanpa dependensi eksternal selain CDN Tailwind.

---

## Architecture

Karena ini adalah aplikasi single-file, arsitekturnya dibagi secara logis menjadi tiga lapisan dalam satu file:

```
index.html
├── <head>
│   ├── Meta tags & title
│   └── Tailwind CSS CDN link
│
├── <body>  ← Struktur HTML
│   ├── Header (judul)
│   ├── Main Content
│   │   ├── Kucing_Display (emoji + reaction bubble)
│   │   └── Klik_Counter
│   ├── Tombol Reset
│   └── Footer
│
└── <script> ← JavaScript (state + logika)
    ├── REACTIONS array
    ├── State variables (clickCount)
    └── Event handlers (handleCatClick, handleReset)
```

### Alur Data

```
Klik Pengguna
    │
    ▼
handleCatClick()
    ├── clickCount += 1
    ├── Reaction_Engine.getRandom()  ──► pilih dari REACTIONS[]
    ├── Update DOM: counter text
    ├── Update DOM: reaction bubble
    └── Trigger animasi CSS (bounce)
```

---

## Components and Interfaces

### 1. Header

- Menampilkan judul aplikasi: **"🐱 Cat Clicker"**
- Teks instruksi: *"Klik kucing untuk melihat reaksinya!"*
- Posisi: atas halaman, terpusat

### 2. Kucing_Display

Representasi kucing menggunakan emoji besar karena tidak ada asset gambar. Emoji dipilih karena:
- Universal di semua browser modern
- Tidak memerlukan file eksternal
- Ekspresif dan mudah dikenali

**Emoji kucing utama**: `🐱` ukuran besar (font-size ~6-8rem via Tailwind)

**Reaction Bubble**: Area teks di atas/bawah emoji yang menampilkan teks reaksi setelah klik. Menggunakan animasi fade-in/out.

**Interaksi**:
- `cursor-pointer` saat hover
- Animasi bounce/scale saat diklik
- Klik trigger `handleCatClick()`

### 3. Klik_Counter

- Teks format: `"Jumlah Klik: [angka]"`
- Diperbarui real-time setiap klik
- Posisi: di bawah Kucing_Display

### 4. Tombol Reset

- Label: `"🔄 Reset"`
- Mereset `clickCount` ke 0
- Menghapus teks reaksi
- Menghapus state animasi

### 5. Footer

- Teks sederhana: *"Dibuat dengan ❤️ & Tailwind CSS"*
- Posisi: bawah halaman

---

## Data Models

### Array Reaksi (`REACTIONS`)

```javascript
const REACTIONS = [
  { teks: "Meow! 😺",       kelas_emoji: "😺" },
  { teks: "Purrrr... 😻",   kelas_emoji: "😻" },
  { teks: "Hiss! 😾",       kelas_emoji: "😾" },
  { teks: "Zzzz... 😴",     kelas_emoji: "😴" },
  { teks: "Yawn! 🙀",       kelas_emoji: "🙀" },
  { teks: "Feed me! 😿",    kelas_emoji: "😿" },
  { teks: "Play time! 😸",  kelas_emoji: "😸" },
  { teks: "So happy! 😹",   kelas_emoji: "😹" },
];
```

Setiap objek reaksi memiliki:
- `teks` — string yang ditampilkan di reaction bubble
- `kelas_emoji` — emoji kucing yang menggantikan emoji default sementara

### State Variables

```javascript
let clickCount = 0;           // Integer, nilai awal 0
let currentReaction = null;   // String | null, reaksi yang sedang ditampilkan
let animationTimer = null;    // Timeout ID untuk menghapus animasi
```

### DOM References (di-cache saat DOMContentLoaded)

```javascript
const catEl        = document.getElementById('cat-display');
const reactionEl   = document.getElementById('reaction-bubble');
const counterEl    = document.getElementById('click-counter');
const resetBtn     = document.getElementById('reset-btn');
```

---

## Skema Warna (Tema Pink/Peach)

Menggunakan warna Tailwind CSS yang sesuai dengan tema kucing yang cerah dan menyenangkan:

| Elemen               | Warna Tailwind                  | Hex Referensi |
|----------------------|---------------------------------|---------------|
| Background halaman   | `bg-pink-50`                    | #fdf2f8       |
| Header background    | `bg-pink-200`                   | #fbcfe8       |
| Judul teks           | `text-pink-700`                 | #be185d       |
| Kucing container     | `bg-white` + `shadow-pink-200`  | #ffffff       |
| Reaction bubble      | `bg-peach` → `bg-orange-100`    | #ffedd5       |
| Reaction teks        | `text-orange-600`               | #ea580c       |
| Counter teks         | `text-pink-600`                 | #db2777       |
| Tombol Reset         | `bg-pink-400` hover `bg-pink-500` | #f472b6     |
| Footer teks          | `text-pink-400`                 | #f472b6       |

---

## Layout & Struktur Halaman

```
┌─────────────────────────────────────────┐
│            🐱 Cat Clicker               │  ← Header (bg-pink-200)
│   "Klik kucing untuk melihat reaksinya!"│
├─────────────────────────────────────────┤
│                                         │
│         ┌───────────────────┐           │
│         │  💬 Meow! 😺      │           │  ← Reaction Bubble (fade-in)
│         │                   │           │
│         │       🐱          │           │  ← Kucing Display (emoji besar)
│         │   (klik di sini)  │           │
│         └───────────────────┘           │
│                                         │
│          Jumlah Klik: 5                 │  ← Counter
│                                         │
│         [ 🔄 Reset ]                    │  ← Reset Button
│                                         │
├─────────────────────────────────────────┤
│       Dibuat dengan ❤️ & Tailwind CSS   │  ← Footer
└─────────────────────────────────────────┘
```

### Struktur HTML Lengkap

```html
<body class="min-h-screen bg-pink-50 flex flex-col">

  <!-- Header -->
  <header class="bg-pink-200 py-6 text-center shadow-sm">
    <h1 class="text-4xl font-bold text-pink-700">🐱 Cat Clicker</h1>
    <p class="text-pink-500 mt-1">Klik kucing untuk melihat reaksinya!</p>
  </header>

  <!-- Main Content -->
  <main class="flex-1 flex flex-col items-center justify-center py-10 px-4">

    <!-- Cat Card -->
    <div class="bg-white rounded-3xl shadow-lg shadow-pink-100 p-8 flex flex-col items-center gap-4 w-full max-w-sm">

      <!-- Reaction Bubble -->
      <div id="reaction-bubble" class="h-10 text-xl font-semibold text-orange-500 transition-opacity duration-300 opacity-0">
        <!-- diisi oleh JS -->
      </div>

      <!-- Cat Display -->
      <div id="cat-display"
           class="text-8xl cursor-pointer select-none transition-transform duration-150 active:scale-90"
           role="button"
           aria-label="Kucing, klik untuk reaksi"
           tabindex="0">
        🐱
      </div>

      <!-- Click Counter -->
      <p id="click-counter" class="text-lg font-medium text-pink-600">
        Jumlah Klik: 0
      </p>
    </div>

    <!-- Reset Button -->
    <button id="reset-btn"
            class="mt-6 px-6 py-2 bg-pink-400 hover:bg-pink-500 active:bg-pink-600 text-white font-semibold rounded-full shadow transition-colors duration-200">
      🔄 Reset
    </button>

  </main>

  <!-- Footer -->
  <footer class="py-4 text-center text-pink-400 text-sm">
    Dibuat dengan ❤️ &amp; Tailwind CSS
  </footer>

</body>
```

---

## Animasi CSS

### 1. Animasi Klik Kucing (Bounce/Scale)

Menggunakan Tailwind utility `transition-transform` dikombinasikan dengan class kustom:

```css
/* Di dalam <style> tag di head */
@keyframes cat-bounce {
  0%   { transform: scale(1); }
  30%  { transform: scale(1.3) rotate(-5deg); }
  60%  { transform: scale(0.9) rotate(3deg); }
  100% { transform: scale(1); }
}

.cat-bounce {
  animation: cat-bounce 0.4s ease;
}
```

**Cara kerja**: JS menambahkan class `.cat-bounce` ke elemen kucing saat diklik, lalu menghapusnya setelah 400ms (`animationend` event atau `setTimeout`).

### 2. Animasi Reaction Bubble (Fade In/Out)

Menggunakan Tailwind `opacity-0` ↔ `opacity-100` dengan `transition-opacity`:

```css
/* State awal */
#reaction-bubble { opacity: 0; transition: opacity 0.3s ease; }

/* JS menambah/hapus class ini */
#reaction-bubble.visible { opacity: 1; }
```

**Cara kerja**:
1. Saat klik: set teks reaksi → tambah class `visible` (fade in)
2. Setelah 2.5 detik: hapus class `visible` (fade out)
3. Setelah fade out selesai (300ms): kosongkan teks

### 3. Hover Effect Kucing

```
hover:scale-110  →  kucing sedikit membesar saat hover
transition-transform duration-200
```

---

## Struktur JavaScript

```javascript
// ─── Constants ───────────────────────────────────────────
const REACTIONS = [
  { teks: "Meow! 😺",      emoji: "😺" },
  { teks: "Purrrr... 😻",  emoji: "😻" },
  { teks: "Hiss! 😾",      emoji: "😾" },
  { teks: "Zzzz... 😴",    emoji: "😴" },
  { teks: "Yawn! 🙀",      emoji: "🙀" },
  { teks: "Feed me! 😿",   emoji: "😿" },
  { teks: "Play time! 😸", emoji: "😸" },
  { teks: "So happy! 😹",  emoji: "😹" },
];

// ─── State ───────────────────────────────────────────────
let clickCount = 0;

// ─── DOM References (di-cache) ───────────────────────────
let catEl, reactionEl, counterEl, resetBtn;

// ─── Reaction Engine ─────────────────────────────────────
function getRandomReaction() {
  const idx = Math.floor(Math.random() * REACTIONS.length);
  return REACTIONS[idx];
}

// ─── UI Updates ──────────────────────────────────────────
function updateCounter() {
  counterEl.textContent = `Jumlah Klik: ${clickCount}`;
}

function showReaction(reaction) {
  reactionEl.textContent = reaction.teks;
  catEl.textContent = reaction.emoji;
  reactionEl.classList.add('visible');

  // Auto-hide setelah 2.5 detik
  setTimeout(() => {
    reactionEl.classList.remove('visible');
    setTimeout(() => {
      reactionEl.textContent = '';
      catEl.textContent = '🐱'; // kembali ke emoji default
    }, 300);
  }, 2500);
}

function triggerBounce() {
  catEl.classList.remove('cat-bounce');
  void catEl.offsetWidth; // reflow trick untuk restart animasi
  catEl.classList.add('cat-bounce');
}

// ─── Event Handlers ──────────────────────────────────────
function handleCatClick() {
  clickCount += 1;
  updateCounter();
  const reaction = getRandomReaction();
  showReaction(reaction);
  triggerBounce();
}

function handleReset() {
  clickCount = 0;
  updateCounter();
  reactionEl.classList.remove('visible');
  reactionEl.textContent = '';
  catEl.textContent = '🐱';
}

// ─── Initialization ──────────────────────────────────────
document.addEventListener('DOMContentLoaded', () => {
  catEl      = document.getElementById('cat-display');
  reactionEl = document.getElementById('reaction-bubble');
  counterEl  = document.getElementById('click-counter');
  resetBtn   = document.getElementById('reset-btn');

  catEl.addEventListener('click', handleCatClick);
  catEl.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' || e.key === ' ') handleCatClick();
  });
  resetBtn.addEventListener('click', handleReset);

  updateCounter(); // pastikan counter terbaca "Jumlah Klik: 0"
});
```

---

## Correctness Properties

*Sebuah properti adalah karakteristik atau perilaku yang harus berlaku pada semua eksekusi sistem yang valid — pada dasarnya, pernyataan formal tentang apa yang seharusnya dilakukan sistem. Properti berfungsi sebagai jembatan antara spesifikasi yang dapat dibaca manusia dan jaminan kebenaran yang dapat diverifikasi oleh mesin.*

### Property 1: Reaksi selalu berasal dari array yang valid

*Untuk semua* pemanggilan `getRandomReaction()`, nilai yang dikembalikan harus selalu menjadi salah satu elemen yang ada di dalam array `REACTIONS`.

**Validates: Requirements 2.1, 2.2**

---

### Property 2: Array reaksi memiliki minimal 5 elemen unik

*Untuk semua* kondisi, array `REACTIONS` harus memiliki panjang ≥ 5 dan semua properti `teks` harus unik (tidak ada duplikat).

**Validates: Requirements 2.3**

---

### Property 3: Distribusi reaksi bersifat acak (tidak deterministik)

*Untuk semua* sekuens N pemanggilan `getRandomReaction()` di mana N ≥ 20, jumlah reaksi unik yang dikembalikan harus lebih dari 1.

**Validates: Requirements 2.4**

---

### Property 4: Counter selalu tepat sama dengan jumlah klik

*Untuk semua* nilai N ≥ 0, jika `handleCatClick()` dipanggil N kali dari state awal (clickCount = 0), maka `clickCount` harus tepat sama dengan N.

**Validates: Requirements 3.2, 3.4**

---

### Property 5: Format tampilan counter selalu benar

*Untuk semua* nilai integer N ≥ 0, `updateCounter()` harus menghasilkan teks yang cocok dengan format `"Jumlah Klik: N"` di elemen DOM counter.

**Validates: Requirements 3.3**

---

### Property 6: Reset mengembalikan semua state ke kondisi awal

*Untuk semua* state aplikasi (clickCount = N berapa pun, reaksi apa pun yang ditampilkan), memanggil `handleReset()` harus menghasilkan: `clickCount === 0`, teks counter = `"Jumlah Klik: 0"`, dan teks reaksi kosong.

**Validates: Requirements 4.2, 4.3, 4.4**

---

## Error Handling

### JavaScript Tidak Tersedia

```html
<noscript>
  <div style="text-align:center; padding:2rem; color:#be185d;">
    ⚠️ JavaScript diperlukan untuk menjalankan Cat Clicker.
    Silakan aktifkan JavaScript di browser Anda.
  </div>
</noscript>
```

### DOM Element Tidak Ditemukan

Semua DOM references di-cache setelah `DOMContentLoaded`, sehingga elemen dipastikan ada sebelum event listener dipasang. Tidak ada akses DOM sebelum halaman sepenuhnya dimuat.

### Race Condition pada Animasi

Timer reaksi (2.5s) menggunakan referensi `setTimeout` yang diganti setiap klik baru — jika pengguna mengklik lagi sebelum timer selesai, timer lama dibatalkan dan timer baru dimulai:

```javascript
let reactionTimer = null;

function showReaction(reaction) {
  clearTimeout(reactionTimer); // batalkan timer sebelumnya
  // ... set teks & class ...
  reactionTimer = setTimeout(() => { /* fade out */ }, 2500);
}
```

---

## Testing Strategy

### Pendekatan Dual Testing

Aplikasi ini menggunakan **kombinasi unit test berbasis contoh** dan **property-based test** untuk mencapai cakupan komprehensif.

Library property-based testing yang digunakan: **[fast-check](https://github.com/dubzzz/fast-check)** (JavaScript/TypeScript).

Setiap property test dikonfigurasi untuk berjalan minimum **100 iterasi**.

### Unit Tests (Berbasis Contoh)

Memverifikasi perilaku spesifik dan kondisi tepi:

| Test | Deskripsi | Requirement |
|------|-----------|-------------|
| UT-1 | Halaman dimuat dengan judul "Cat Clicker" | 1.1 |
| UT-2 | Elemen Kucing_Display ada di DOM | 1.2 |
| UT-3 | Teks instruksi tampil saat load | 1.3 |
| UT-4 | Counter dimulai dari 0 | 3.1 |
| UT-5 | Tombol Reset ada di DOM | 4.1 |
| UT-6 | Tag `<noscript>` ada di HTML | 6.4 |
| UT-7 | Animasi bounce ditrigger saat klik | 2.5 |
| UT-8 | Cursor pointer tampil saat hover | 5.4 |

### Property-Based Tests

Memverifikasi properti universal yang berlaku untuk semua input:

| Test | Properti | Iterasi | Tag |
|------|----------|---------|-----|
| PBT-1 | Reaksi selalu dari array valid | 200 | `Feature: cat-clicker, Property 1` |
| PBT-2 | Array reaksi ≥ 5 elemen unik | 1 (invariant) | `Feature: cat-clicker, Property 2` |
| PBT-3 | Distribusi acak untuk N ≥ 20 klik | 100 | `Feature: cat-clicker, Property 3` |
| PBT-4 | Counter = N setelah N klik | 100 | `Feature: cat-clicker, Property 4` |
| PBT-5 | Format counter selalu benar | 100 | `Feature: cat-clicker, Property 5` |
| PBT-6 | Reset mengembalikan state awal | 100 | `Feature: cat-clicker, Property 6` |

### Smoke Tests

Verifikasi konfigurasi satu kali:

- Link CDN Tailwind ada di `<head>`
- File `index.html` berisi semua kode (tidak ada file eksternal)
- Semua JS ada di dalam tag `<script>` inline

### Catatan Aksesibilitas

- Elemen kucing menggunakan `role="button"`, `tabindex="0"`, dan `aria-label`
- Keyboard support: `Enter` dan `Space` memicu klik
- Warna memiliki kontras yang cukup antara teks dan background (pink-700 di atas pink-50)
