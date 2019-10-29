---
title: "API: Properti head"
description: Nuxt.js memungkinkan Anda menentukan semua meta default untuk aplikasi Anda di dalam nuxt.config.js.
---

> Nuxt.js memungkinkan Anda menentukan semua meta default untuk aplikasi Anda di dalam nuxt.config.js, menggunakan properti `head` yang sama

- Type: `Object`

Sebagai contoh pada `nuxt.config.js`:
```js
export default {
  head: {
    titleTemplate: '%s - Nuxt.js',
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },

      // hid digunakan sebagai pengidentifikasi unik. Jangan menggunakan sebagai `vmid` karena tidak akan berfungsi
      { hid: 'description', name: 'description', content: 'Meta description' }
    ]
  }
}
```

Untuk mengetahui daftar apasaja yang bisa Anda berikan untuk `head`, lihat [dokumentasi vue-meta](https://vue-meta.nuxtjs.org/api/#metainfo-properties).

Anda dapat juga menggunakan `head` didalam komponen dan mengakses data melalui `this` ([selengkapnya](/api/pages-head)).

<div class="Alert Alert--teal">

<b>Informasi:</b> Untuk menghindari duplikasi meta tag ketika digunakan dalam komponen child, atur pengidentifikasi unik dengan key `hid` pada meta Anda ([baca lebih lanjut] (https://vue-meta.nuxtjs.org/api/#tagidkeyname)) .

</div>
