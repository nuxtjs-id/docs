---
title: "API: Properti serverMiddleware"
description: Mendefinisikan middleware sisi-server.
---

- Type: `Array`
    - Items: `String` or `Object` or `Function`

Secara internal, Nuxt membuat instance [koneksi](https://github.com/senchalabs/connect) bahwa kita dapat menambahkan middleware kustom kita sendiri. Ini memungkinkan kami untuk mendaftarkan rute tambahan (biasanya rute `/ api`) **tanpa perlu server eksternal**.

Karena koneksinya sendiri itu adalah middleware, middleware yang terdaftar (ter-register) akan bekerja dengan `nuxt start` keduanya
dan juga ketika digunakan sebagai middleware dengan penggunaan terprogram seperti [express-template](https://github.com/nuxt-community/express-template).
[Modul](/guide/modules) Nuxt bisa juga menyediakan `serverMiddleware`
menggunakan [this.addServerMiddleware()](/api/internals-module-container#addservermiddleware-middleware-)

Tambahan untuk itu, kami mengenalkan opsi `prefix` yang secara default adalah `true`. Ini akan menambah basis router (router base) ke middlewares server Anda.

**Contoh:**

* Path Server middleware: `/api`
* Router base: `/admin`
* Dengan `prefix: true` (default): `/admin/api`
* Dengan `prefix: false`: `/api`

## serverMiddleware vs middleware!

Jangan bingung dengan [middleware routes](/guide/routing#middleware) yang dipanggil sebelum masing-masing rute oleh Vue pada Sisi Klien atau SSR.
Middleware yang terdaftar pada properti `serverMiddleware` menjalankan sisi server **sebelum** `vue-server-renderer` dan dapat digunakan untuk tugas khusus server seperti menangani permintaan API atau melakukan serve untuk aset.

## Penggunaan

Jika middleware adalah String, Nuxt.js akan mencoba untuk secara otomatis menyelesaikan (resolve) dan memerlukannya (require).

Contoh (`nuxt.config.js`):

```js
import serveStatic from 'serve-static'

export default {
  serverMiddleware: [
    // Akan mendaftarkan paket redirect-ssl npm
    'redirect-ssl',

    // Akan mendaftarkan file dari direktori api proyek untuk menangani /api/* yang dibutuhkan
    { path: '/api', handler: '~/api/index.js' },

    // Kita juga dapat membuat contoh secara kustom
    { path: '/static2', handler: serveStatic(__dirname + '/static2') }
  ]
}
```

<p class="Alert Alert--danger">
    <b>HEADS UP! </b>
    Jika Anda tidak ingin middleware didaftarkan untuk semua rute, Anda harus menggunakan form Obyek dengan path tertentu,
    jika tidak, default handler tidak akan berfungsi!
</p>

## Middleware Server Kustom

Dimungkinkan juga untuk menulis middleware secara kustom. Untuk informasi lebih lanjut, lihat [Dokumentasi Connect](https://github.com/senchalabs/connect#appusefn).

Middleware (`api/logger.js`):

```js
export default function (req, res, next) {
  // req adalah objek request Node.js http
  console.log(req.url)

  // res adalah objek respons Node.js http

  // next adalah fungsi untuk melakukan panggilan untuk meminta middleware berikutnya
  // Jangan lupa memanggil next di akhir jika middleware Anda bukan endpoint
  next()
}
```

Konfigurasi Nuxt (`nuxt.config.js`):

```js
serverMiddleware: [
  '~/api/logger'
]
```
