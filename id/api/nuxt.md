---
title: "API: Menggunakan Nuxt.js Secara Program"
description: Anda dapat menggunakan Nuxt.js secara terprogram untuk menggunakannya sebagai middleware yang memberi Anda kebebasan membuat server sendiri untuk merender aplikasi web Anda.
---

Anda mungkin ingin menggunakan server Anda sendiri dengan middleware dan API Anda. Itu sebabnya Anda dapat menggunakan Nuxt.js secara terprogram.

Anda dapat melakukan require Nuxt.js seperti ini:

```js
const { Nuxt, Builder } = require('nuxt')
```

## Konstruktor Nuxt

Untuk melihat daftar opsi yang akan diberikan kepada Nuxt.js, lihat bagian konfigurasi.

```js
// Require modul `Nuxt` dan `Builder`
const { Nuxt, Builder } = require('nuxt')

// Require konfigurasi Nuxt
const config = require('./nuxt.config.js')

// Membuat instance new Nuxt
const nuxt = new Nuxt(config)

// Aktifkan live build & reload pada mode dev
if (nuxt.options.dev) {
  new Builder(nuxt).build()
}

// Kita dapat menggunakan `nuxt.render(req, res)` atau `nuxt.renderRoute(route, context)`
```

Anda dapat melihat [nuxt-express](https://github.com/nuxt/express) and [adonuxt](https://github.com/nuxt/adonuxt) starter untuk dapat memulainya dengan cepat.

### Debug logs

Jika Anda ingin menampilkan log Nuxt.js, Anda dapat menambahkan kode di bawah ini ke bagian atas file Anda:

```js
process.env.DEBUG = 'nuxt:*'
```
