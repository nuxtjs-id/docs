---
title: Perintah dan Penerapan (Deployment)
description: Nuxt.js hadir dengan seperangkat perintah yang berguna, baik untuk tujuan pengembangan maupun produksi.
---

> Nuxt.js hadir dengan serangkaian perintah yang berguna, baik untuk tujuan pengembangan maupun produksi.

## Daftar Perintah (Command)

| Perintah         | Penjelasan                                                                              |
|-----------------|------------------------------------------------------------------------------------------|
| nuxt            | Luncurkan server pengembangan di localhost:3000 dengan hot-reload.                        |
| nuxt build      | Bangun aplikasi Anda dengan webpack dan minify JS & CSS (untuk produksi).            |
| nuxt start      | Mulai server dalam mode produksi (setelah menjalankan `nuxt build`).                        |
| nuxt generate   | Bangun aplikasi dan hasilkan setiap rute sebagai file HTML (digunakan untuk hosting statis). |

#### Argumen

Anda dapat menggunakan `--help` dengan perintah apa pun untuk melihat cara penggunaan terperinci. Argumen umum adalah:

- **`--config-file` or `-c`:** menentukan path ke file `nuxt.config.js`.
- **`--spa` or `-s`:** Menjalankan perintah dalam mode SPA dan menonaktifkan rendering sisi server.
- **`--unix-socket` or `-n`:** menentukan jalur ke soket UNIX.

#### Menggunakannya di dalam package.json

Anda harus meletakkan perintah ini di `package.json`:

```json
"scripts": {
  "dev": "nuxt",
  "build": "nuxt build",
  "start": "nuxt start",
  "generate": "nuxt generate"
}
```

Kemudian, Anda dapat menjalankannya melalui `npm run <command>` (contoh: `npm run dev`).

<div class="Alert Alert--nuxt-green">

<b>Tips pro:</b> untuk meneruskan argumen ke perintah npm, Anda membutuhkan tambahan nama script <code>--</code> (contoh: <code>npm run dev -- --spa</code>).

</div>

## Lingkungan Pengembangan (Development Environment)

Untuk meluncurkan Nuxt dalam mode pengembangan dengan hot-reload:

```bash
nuxt
// ATAU
npm run dev
```

## Penerapan Produksi (Deployment)

Nuxt.js memungkinkan Anda memilih di antara tiga mode untuk menyebarkan aplikasi Anda: SSR, Static Generated, atau SPA.

### Penerapan Rendered Sisi-Server (Universal SSR)

Untuk melakukan deploy, alih-alih menjalankan `nuxt`, Anda mungkin ingin membangun sebelumnya. Oleh karena itu, building dan starting adalah perintah yang terpisah:

```bash
nuxt build
nuxt start
```

Anda juga dapat mengatur `server.https` di` nuxt.config.js` Anda dengan serangkaian opsi yang sama yang diteruskan ke [`https.createServer`](https://nodejs.org/api/https.html), hendaknya Anda pilih untuk melayani Nuxt.js dalam mode HTTPS.
Soket Unix juga tersedia jika Anda mengatur opsi `server.socket` di dalam `nuxt.config.js` (atau `-n` di dalam [CLI](https://nuxtjs.org/guide/commands#list-of-commands)).
Ketika menggunakan [Unix sockets](https://en.wikipedia.org/wiki/Berkeley_sockets), pastikan untuk tidak mengatur parameter `host` dan` port`. Jika tidak, parameter `socket` akan diabaikan.

Berikut adalah `package.json` yang direkomendasikan:

```json
{
  "name": "my-app",
  "dependencies": {
    "nuxt": "latest"
  },
  "scripts": {
    "dev": "nuxt",
    "build": "nuxt build",
    "start": "nuxt start"
  }
}
```

Catatan: kami merekomendasikan untuk meletakan `.nuxt` di dalam `.npmignore` atau `.gitignore`.

### Penyebaran Generasi Static (Pra-render)

Nuxt.js memberi Anda kemampuan untuk menyimpan (host) aplikasi web di hosting statis apa pun.

Untuk membuat aplikasi web kita menjadi file statis adalah sebagai berikut:

```bash
npm run generate
```

Ini akan membuat folder `dist` dengan segala isinya yang sudah siap untuk digunakan di situs hosting statis.

Untuk mengembalikan kode status non-zero ketika eror halaman ditemukan dan membiarkan CI/CD menggagalkan penyebaran (deployment) atau build, Anda dapat menggunakan argumen `--fail-on-page-error`.

```bash
npm run generate --fail-on-page-error

// ATAU

yarn generate --fail-on-page-error
```

Jika Anda memiliki proyek dengan [route dinamis](/guide/routing#dynamic-routes), silahkan lihat [konfigurasi generate](/api/configuration-generate) untuk memberi tahu Nuxt.js cara membuat rute dinamis ini.

<div class="Alert">

Ketika membuat aplikasi web Anda dengan `nuxt generate`, [konteks](/api/context) akan diberikan kepada [asyncData](/guide/async-data) dan [fetch](/guide/vuex-store#the-fetch-method) tidak akan mempunyai `req` dan `res`.

</div>

### Penerapan Aplikasi Satu Halaman (SPA)

`nuxt generate` masih membutuhkan mesin SSR selama build/generate dan memiliki keuntungan memiliki semua halaman yang sudah dirender, dan memiliki skor SEO dan page-load yang tinggi. Karena konten dihasilkan pada *waktu pembuatan*. Misalnya, kami tidak dapat menggunakannya untuk aplikasi yang isinya bergantung pada autentikasi pengguna atau API realtime (setidaknya untuk pemuatan pertama).

Ide SPA itu sangat sederhana! Ketika mode SPA diaktifkan menggunakan flag `mode: 'spa'` atau `--spa`, kemudian menjalankan build, maka generate akan secara otomatis dimulai setelah build. Generasi ini berisi meta umum dan link resource, tetapi bukan konten halaman.

Jadi, untuk melakukan deploy SPA, Anda harus melakukan hal berikut:

 - Ubah `mode` di dalam `nuxt.config.js` menjadi `spa`.
 - Jalankan `npm run build`.
 - Lakukan deploy untuk folder `dist/` yang sudah dibuat  ke static hosting milik anda seperti misalnya Surge, GitHub Pages atau nginx.

Metode deploy lain yang mungkin dilakukan adalah dengan menggunakan Nuxt sebagai middleware dalam frameworks selama dalam mode `spa`. Ini membantu mengurangi beban server dan menggunakan Nuxt dalam proyek-proyek di mana SSR tidak dimungkinkan.

<div class="Alert">

Baca [FAQ](/faq) kami dan temukan contoh bagus untuk penerapan ke host populer.

</div>
