# Implementation Plan: Cat Clicker

## Overview

Implementasi aplikasi Cat Clicker dalam satu file `index.html` menggunakan HTML5, Tailwind CSS via CDN, dan Vanilla JavaScript. Pendekatan pembangunan dilakukan secara bertahap — mulai dari struktur HTML dasar, lalu tampilan, logika interaksi, animasi, hingga aksesibilitas dan fallback.

File output: `index.html` di root workspace.

---

## Tasks

- [x] 1. Buat struktur HTML dasar dan setup Tailwind CDN
  - Buat file `index.html` di root workspace
  - Tambahkan doctype, `<html lang="id">`, `<head>`, dan `<body>`
  - Tambahkan meta tag charset (`UTF-8`) dan viewport (`width=device-width, initial-scale=1.0`)
  - Tambahkan `<title>Cat Clicker</title>`
  - Pasang link CDN Tailwind CSS di dalam `<head>`: `<script src="https://cdn.tailwindcss.com"></script>`
  - Tambahkan tag `<style>` kosong di `<head>` untuk animasi kustom (akan diisi nanti)
  - Tambahkan tag `<script>` kosong di akhir `<body>` untuk JavaScript (akan diisi nanti)
  - _Requirements: 1.1, 6.1, 6.2, 6.3_

- [x] 2. Implementasi struktur layout dan tampilan halaman
  - [x] 2.1 Buat kerangka layout utama di dalam `<body>`
    - Tambahkan `<header>`, `<main>`, dan `<footer>` dengan class Tailwind dasar
    - `<body>` gunakan `class="min-h-screen bg-pink-50 flex flex-col"`
    - Header: `class="bg-pink-200 py-6 text-center shadow-sm"`
    - Main: `class="flex-1 flex flex-col items-center justify-center py-10 px-4"`
    - Footer: `class="py-4 text-center text-pink-400 text-sm"`
    - _Requirements: 1.1, 5.1, 5.2_

  - [x] 2.2 Tambahkan judul dan teks instruksi di header
    - `<h1>` dengan teks `"🐱 Cat Clicker"` dan class `text-4xl font-bold text-pink-700`
    - `<p>` dengan teks `"Klik kucing untuk melihat reaksinya!"` dan class `text-pink-500 mt-1`
    - Teks footer: `"Dibuat dengan ❤️ & Tailwind CSS"`
    - _Requirements: 1.1, 1.3_

  - [x] 2.3 Buat Cat Card — kontainer utama kucing
    - Di dalam `<main>`, buat `<div>` card dengan class `bg-white rounded-3xl shadow-lg shadow-pink-100 p-8 flex flex-col items-center gap-4 w-full max-w-sm`
    - Di dalam card, tambahkan elemen berikut (masih kosong, akan diisi di task selanjutnya):
      - `<div id="reaction-bubble">` untuk teks reaksi
      - `<div id="cat-display">` untuk emoji kucing (`🐱`)
      - `<p id="click-counter">` untuk counter
    - _Requirements: 1.2, 5.2, 5.3_

  - [x] 2.4 Buat Reaction Bubble
    - Isi `#reaction-bubble` dengan class `h-10 text-xl font-semibold text-orange-500 opacity-0` dan `style="transition: opacity 0.3s ease;"`
    - Kosongkan isi teks (akan diisi oleh JavaScript)
    - _Requirements: 2.2_

  - [x] 2.5 Buat elemen Kucing Display
    - Isi `#cat-display` dengan emoji `🐱` dan class `text-8xl cursor-pointer select-none`
    - Tambahkan class Tailwind untuk hover: `hover:scale-110 transition-transform duration-200`
    - _Requirements: 1.2, 2.5, 5.2, 5.4_

  - [x] 2.6 Buat Click Counter
    - Isi `#click-counter` dengan teks `"Jumlah Klik: 0"` dan class `text-lg font-medium text-pink-600`
    - _Requirements: 3.1, 3.3_

  - [x] 2.7 Buat Tombol Reset
    - Tambahkan `<button id="reset-btn">` di bawah card (di dalam `<main>`)
    - Teks tombol: `"🔄 Reset"`
    - Class: `mt-6 px-6 py-2 bg-pink-400 hover:bg-pink-500 active:bg-pink-600 text-white font-semibold rounded-full shadow transition-colors duration-200`
    - _Requirements: 4.1_

- [x] 3. Checkpoint — Verifikasi tampilan statis
  - Pastikan semua elemen tampil dengan benar di browser (buka `index.html`)
  - Periksa judul, emoji kucing, counter "Jumlah Klik: 0", dan tombol Reset terlihat
  - Pastikan layout responsif di mobile (lebar < 768px)
  - Tanyakan kepada pengguna jika ada pertanyaan sebelum melanjutkan.

- [x] 4. Implementasi Animasi CSS Kustom
  - [x] 4.1 Tambahkan animasi bounce kucing (`cat-bounce`)
    - Di dalam tag `<style>` di `<head>`, tambahkan keyframe animasi:
      ```css
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
    - _Requirements: 2.5_

  - [x] 4.2 Tambahkan class animasi fade untuk Reaction Bubble
    - Di dalam tag `<style>`, tambahkan:
      ```css
      #reaction-bubble.visible {
        opacity: 1;
      }
      ```
    - _Requirements: 2.2_

- [x] 5. Implementasi Reaction Engine (JavaScript)
  - [x] 5.1 Definisikan konstanta `REACTIONS` array
    - Di dalam tag `<script>`, tambahkan array berisi minimal 8 objek reaksi
    - Setiap objek memiliki properti `teks` (string) dan `emoji` (string)
    - Contoh: `{ teks: "Meow! 😺", emoji: "😺" }`, `{ teks: "Purrrr... 😻", emoji: "😻" }`, dst.
    - Sertakan minimal: Meow, Purrrr, Hiss, Zzzz, Yawn, Feed me, Play time, So happy
    - _Requirements: 2.2, 2.3_

  - [x] 5.2 Buat fungsi `getRandomReaction()`
    - Fungsi menggunakan `Math.floor(Math.random() * REACTIONS.length)` untuk memilih indeks acak
    - Fungsi mengembalikan objek reaksi dari `REACTIONS[idx]`
    - _Requirements: 2.1, 2.4_

  - [ ]* 5.3 Tulis property test untuk Reaction Engine
    - **Property 1: Reaksi selalu berasal dari array yang valid**
    - **Validates: Requirements 2.1, 2.2**
    - **Property 2: Array reaksi memiliki minimal 5 elemen unik**
    - **Validates: Requirements 2.3**
    - **Property 3: Distribusi reaksi bersifat acak (tidak deterministik)**
    - **Validates: Requirements 2.4**
    - Gunakan library fast-check untuk property-based testing (minimum 100 iterasi)
    - Tag: `Feature: cat-clicker, Property 1`, `Property 2`, `Property 3`

- [x] 6. Implementasi Click Counter (JavaScript)
  - [x] 6.1 Definisikan variabel state dan cache referensi DOM
    - Tambahkan variabel state: `let clickCount = 0;`, `let reactionTimer = null;`
    - Cache DOM references di dalam `DOMContentLoaded`:
      ```javascript
      const catEl      = document.getElementById('cat-display');
      const reactionEl = document.getElementById('reaction-bubble');
      const counterEl  = document.getElementById('click-counter');
      const resetBtn   = document.getElementById('reset-btn');
      ```
    - _Requirements: 3.1_

  - [x] 6.2 Buat fungsi `updateCounter()`
    - Fungsi mengisi `counterEl.textContent` dengan `"Jumlah Klik: " + clickCount`
    - _Requirements: 3.3_

  - [x] 6.3 Buat fungsi `showReaction(reaction)`
    - Set `reactionEl.textContent = reaction.teks`
    - Set `catEl.textContent = reaction.emoji` (ubah emoji kucing sementara)
    - Tambahkan class `visible` ke `reactionEl` (fade in)
    - Batalkan timer sebelumnya dengan `clearTimeout(reactionTimer)`
    - Set timer baru 2500ms untuk fade out: hapus class `visible`, lalu setelah 300ms kosongkan teks dan kembalikan emoji kucing ke `🐱`
    - _Requirements: 2.2_

  - [x] 6.4 Buat fungsi `triggerBounce()`
    - Hapus class `cat-bounce` dari `catEl`
    - Paksa reflow dengan `void catEl.offsetWidth`
    - Tambahkan kembali class `cat-bounce`
    - _Requirements: 2.5_

  - [x] 6.5 Buat fungsi `handleCatClick()`
    - Tambah `clickCount += 1`
    - Panggil `updateCounter()`
    - Panggil `getRandomReaction()` dan simpan hasilnya
    - Panggil `showReaction(reaction)`
    - Panggil `triggerBounce()`
    - _Requirements: 2.1, 3.2_

  - [ ]* 6.6 Tulis property test untuk Click Counter
    - **Property 4: Counter selalu tepat sama dengan jumlah klik**
    - **Validates: Requirements 3.2, 3.4**
    - **Property 5: Format tampilan counter selalu benar**
    - **Validates: Requirements 3.3**
    - Gunakan fast-check, generate integer N (0–1000), simulasikan N klik, verifikasi nilai
    - Tag: `Feature: cat-clicker, Property 4`, `Property 5`

- [x] 7. Implementasi Tombol Reset (JavaScript)
  - [x] 7.1 Buat fungsi `handleReset()`
    - Set `clickCount = 0`
    - Panggil `updateCounter()`
    - Hapus class `visible` dari `reactionEl`
    - Kosongkan `reactionEl.textContent`
    - Kembalikan `catEl.textContent` ke `🐱`
    - Batalkan timer aktif dengan `clearTimeout(reactionTimer)` dan set `reactionTimer = null`
    - _Requirements: 4.2, 4.3, 4.4_

  - [ ]* 7.2 Tulis property test untuk Reset
    - **Property 6: Reset mengembalikan semua state ke kondisi awal**
    - **Validates: Requirements 4.2, 4.3, 4.4**
    - Generate state acak (clickCount random, reaksi random), panggil reset, verifikasi semua kembali ke awal
    - Tag: `Feature: cat-clicker, Property 6`

- [x] 8. Pasang Event Listeners dan Inisialisasi
  - Di dalam `document.addEventListener('DOMContentLoaded', ...)`:
    - Pasang `catEl.addEventListener('click', handleCatClick)`
    - Panggil `updateCounter()` saat init untuk memastikan counter terbaca "Jumlah Klik: 0"
    - _Requirements: 2.1, 3.1_

- [x] 9. Implementasi Aksesibilitas
  - [x] 9.1 Tambahkan atribut ARIA dan role pada Kucing Display
    - Tambahkan `role="button"` ke `#cat-display`
    - Tambahkan `tabindex="0"` agar bisa difokus dengan keyboard
    - Tambahkan `aria-label="Kucing, klik untuk reaksi"` untuk screen reader
    - _Requirements: (aksesibilitas dari design.md)_

  - [x] 9.2 Tambahkan keyboard support
    - Di dalam `DOMContentLoaded`, pasang event listener:
      ```javascript
      catEl.addEventListener('keydown', (e) => {
        if (e.key === 'Enter' || e.key === ' ') handleCatClick();
      });
      ```
    - _Requirements: (aksesibilitas dari design.md)_

- [x] 10. Implementasi Noscript Fallback
  - Tambahkan tag `<noscript>` di dalam `<body>` (tepat di atas `<header>`)
  - Isi dengan pesan fallback: `"⚠️ JavaScript diperlukan untuk menjalankan Cat Clicker. Silakan aktifkan JavaScript di browser Anda."`
  - Beri styling inline agar tetap terbaca meski Tailwind tidak dimuat: `style="text-align:center; padding:2rem; color:#be185d;"`
  - _Requirements: 6.4_

- [x] 11. Checkpoint Akhir — Verifikasi keseluruhan
  - Buka `index.html` di browser dan uji semua interaksi:
    - Klik kucing → reaksi acak muncul, counter bertambah, animasi bounce berjalan
    - Reaction bubble fade in dan fade out setelah 2.5 detik
    - Klik Reset → counter kembali 0, reaksi hilang, emoji kembali ke 🐱
    - Navigasi keyboard: Tab ke kucing → tekan Enter/Space → reaksi muncul
    - Nonaktifkan JavaScript → pesan noscript tampil
    - Uji di lebar layar < 768px untuk responsivitas
  - Pastikan semua tests pass (jika task bertanda `*` dikerjakan)
  - Tanyakan kepada pengguna jika ada pertanyaan.

---

## Task Dependency Graph

```json
{
  "waves": [
    { "wave": 1, "tasks": ["1"] },
    { "wave": 2, "tasks": ["2.1", "2.2", "2.3", "2.4", "2.5", "2.6", "2.7"] },
    { "wave": 3, "tasks": ["3"] },
    { "wave": 4, "tasks": ["4.1", "4.2"] },
    { "wave": 5, "tasks": ["5.1", "5.2"] },
    { "wave": 6, "tasks": ["5.3", "6.1", "6.2", "6.3", "6.4", "6.5"] },
    { "wave": 7, "tasks": ["6.6", "7.1"] },
    { "wave": 8, "tasks": ["7.2", "8"] },
    { "wave": 9, "tasks": ["9.1", "9.2"] },
    { "wave": 10, "tasks": ["10"] },
    { "wave": 11, "tasks": ["11"] }
  ]
}
```

---

## Notes

- Task bertanda `*` adalah **opsional** (test-related) dan dapat dilewati untuk implementasi MVP yang lebih cepat
- Setiap task mereferensikan requirement spesifik untuk keterlacakan
- Urutan task dirancang agar setiap langkah langsung terintegrasi ke langkah sebelumnya — tidak ada kode yang tergantung di tengah jalan
- Property test menggunakan library **fast-check** dan dikonfigurasi minimal 100 iterasi per properti
- Semua kode (HTML, CSS, JS) berada dalam satu file `index.html` — tidak ada file eksternal selain CDN Tailwind
