---
title: Struktur Direktori
description: Struktur aplikasi Nuxt.js default dimaksudkan untuk memberikan titik awal yang bagus untuk aplikasi besar dan kecil.
---

> Struktur aplikasi Nuxt.js default dimaksudkan untuk memberikan titik awal yang bagus untuk aplikasi kecil dan besar. Tentu saja, Anda bebas mengatur aplikasi sesuka Anda.

<div class="Promo__Video">
  <a href="https://vueschool.io/lessons/guided-nuxtjs-project-tour?friend=nuxt" target="_blank">
    <p class="Promo__Video__Icon">
      Tonton pelajaran gratis tentang <strong>struktur direktori Nuxt.js</strong> di Vue School 
    </p>
  </a>
</div>

## Direktori

### Direktori Aset

Direktori `assets` berisi aset Anda yang belum dikompilasi seperti file, gambar, atau font Stylus atau Sass.

[Dokumentasi lebih lanjut tentang integrasi Aset](/guide/assets)

### Direktori Komponen

Direktori `components` berisikan komponen Vue.js Anda. Anda tidak bisa menggunakan keduanya `asyncData` atau `fetch` dalam komponen ini.

### Direktori Layouts

Direktori `layouts` termasuk tata letak aplikasi Anda. Layouts digunakan untuk mengubah tampilan dan nuansa halaman Anda (misalnya dengan menyertakan sidebar).

[Dokumentasi lebih lanjut tentang integrasi Tata Letak](/guide/views#layouts)

_Direktori ini tidak dapat diganti namanya tanpa konfigurasi tambahan._

### Direktori Middleware

Direktori `middleware` berisi Middleware Aplikasi Anda. Middleware memungkinkan Anda menentukan fungsi khusus yang dapat dijalankan sebelum merender halaman atau grup halaman (layouts/tata letak).

[Dokumentasi lebih lanjut tentang integrasi Middleware](/guide/routing#middleware)

### Direktori Pages

Direktori `pages` berisi Tampilan Aplikasi dan Rute Anda. Kerangka kerja membaca semua file `.vue` di dalam direktori ini dan membuat aplikasi router.

_Direktori ini tidak dapat diganti namanya tanpa konfigurasi tambahan._

[Dokumentasi lebih lanjut tentang integrasi Pages](/guide/views)

### Direktori Plugins

Direktori `plugins` berisi plugin Javascript yang ingin Anda jalankan sebelum instantiating Aplikasi root Vue.js. Ini adalah tempat untuk mendaftarkan komponen secara global dan untuk melakukan inject fungsi atau konstanta.

[Dokumentasi lebih lanjut tentang integrasi Plugin](/guide/plugins)

### Direktori Static

Direktori `static` langsung dipetakan ke root server (`/static/robots.txt` dapat diakses di bawah `http://localhost:3000/robots.txt`) dan berisikan file yang kemungkinan tidak akan diubah (misalnya favicon)

**Contoh:** `/static/robots.txt` dipetakan sebagai `/robots.txt`

_Direktori ini tidak dapat diganti namanya tanpa konfigurasi tambahan._

[Dokumentasi lebih lanjut tentang integrasi statis](/guide/assets#static)

### Direktori Store

Direktori `store` berisi file-file [Vuex Store](http://vuex.vuejs.org/en/) Anda. Vuex Store include dengan Nuxt.js tetapi dinonaktifkan secara default. Jika Anda membuat file `index.js` di dalam direktori ini, maka akan mengaktifkan store.

_Direktori ini tidak dapat diganti namanya tanpa konfigurasi tambahan._

[Dokumentasi lebih lanjut tentang integrasi Store](/guide/vuex-store)

### File nuxt.config.js

File `nuxt.config.js` berisi konfigurasi khusus Nuxt.js Anda.

_File ini tidak dapat diganti namanya tanpa konfigurasi tambahan._

[Dokumentasi lebih lanjut tentang integrasi `nuxt.config.js`](/guide/configuration)

### File package.json

File `package.json` berisi dependensi dan skrip Aplikasi Anda.

_File ini tidak dapat diganti namanya._

## Alias

| Alias | Direktori |
|-----|------|
| `~` atau `@` | [srcDir](/api/configuration-srcdir) |
| `~~` atau `@@` | [rootDir](/api/configuration-rootdir) |

Secara default, `srcDir` sama saja dengan `rootDir`.

<div class="Alert Alert--nuxt-green">

<b>Info:</b> Di dalam template `vue` Anda, jika anda membutuhkan untuk melakukan link direktori `assets` atau `static` Anda, gunakan `~/assets/your_image.png` dan `~/static/your_image.png`.

</div>
