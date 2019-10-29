---
title: "API: Properti modern"
description: Build dan server bundel modern
---

> Fitur ini terinpirasi oleh [mode modern dari vue-cli](https://cli.vuejs.org/guide/browser-compatibility.html#modern-mode) 

- Type: `String` or `Boolean`
  - Default: false
  - Possible values:
    - `'client'`: Melakukan serve keduanya, script modern bundel `<script type="module">` dan bundel versi lawas `<script nomodule>`, juga menyediakan `<link rel="modulepreload">` untuk bundel modern. Setiap browser yang mengerti tipe `module` akan memuat bundel modern sementara browser yang lebih lama akan kembali ke versi lama (transpiled).
    - `'server'` atau `true`: Server Node.js akan melakukan cek terhadap versi browser berdasarkan agen pengguna (user agent) dan melakukan serve terhadap bundel modern atau bundel versi lawas.
    - `false`: Me-nonaktifkan build modern

Dua versi bundel adalah sebagai berikut:

1. Bundel modern: menargetkan browser modern yang mendukung modul ES
2. Bundel lawas: menargetkan browser yang lebih lama berdasarkan konfigurasi babel (IE9 kompatibel secara default).

**Info:**

- Gunakan opsi perintah `[--modern | -m]=[mode]` untuk melakukan build/start bundel modern, sebagai contoh: di dalam `package.json`:

```json
{
  "scripts": {
    "build:modern": "nuxt build --modern=server",
    "start:modern": "nuxt start --modern=server"
  }
}
```
**Catatan tentang *nuxt generate*:** Properti `modern` juga dapat bekerja dengan perintah `nuxt generate`, tetapi dalam hal ini hanya opsi `client` yang diutamakan dan akan dipilih secara otomatis saat meluncurkan perintah `nuxt generate --modern` tanpa memberikan nilai apa pun.

- Nuxt akan secara otomatis mendeteksi build `modern` di dalam `nuxt start` ketika `modern` is not specified, mode auto-detected adalah:

| Mode          | Mode Modern |
| ------------- |:-------------:|
| universal     | server        |
| spa           | client        |

- Mode modern untuk `nuxt generate` hanya bisa `client`
- Gunakan [`build.crossorigin`](/api/configuration-build#crossorigin) untuk mengatur atribut `crossorigin` di dalam `<link>` dan `<script>`

> Silahkan lihat [Postingan keren dari Phillip Walton's](https://philipwalton.com/articles/deploying-es2015-code-in-production-today/) untuk memperbanyak pengetahuan tentang build modern.
