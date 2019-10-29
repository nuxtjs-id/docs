---
title: "API: Komponen <nuxt-link>"
description: Menghubungkan antar halaman menggunakan nuxt-link.
---

> Komponen ini digunakan untuk menyediakan navigasi antara komponen halaman dan meningkatkan kinerja dengan prefetching yang cerdas.

Komponen `<nuxt-link>` sangat penting di Nuxt. Karena **digunakan sebagai navigasi** pada aplikasi Anda, sama seperti komponen `<router-link>` pada Aplikasi Vue. Yang memang pada kenyataannya, `<nuxt-link>` meng-extend [`<router-link>`](https://router.vuejs.org/api/#router-link). Itu berarti dibutuhkan properti yang sama dan dapat digunakan dengan cara yang sama pula. Baca [Dokumentasi Vue Router](https://router.vuejs.org/api/#router-link) untuk info lebih lanjut.

Contoh (`pages/index.vue`):

```html
<template>
  <div>
    <h1>Halaman Utama</h1>
    <nuxt-link to="/about">Tentang Kami (tautan internal yang dimiliki oleh Nuxt App)</nuxt-link>
    <a href="https://nuxtjs.org">Eksternal link ke halaman lain</a>
  </div>
</template>
```

**Aliases:** `<n-link>`, `<NuxtLink>`, dan `<NLink>`

> Diperkenalkan di Nuxt v2.4.0

Untuk meningkatkan daya tanggap aplikasi Nuxt.js Anda, ketika tautan akan ditampilkan di dalam viewport, Nuxt.js akan secara otomatis mengambil halaman *code splitted*. Fitur ini terinspirasi oleh [quicklink.js](https://github.com/GoogleChromeLabs/quicklink) oleh Google Chrome Labs.

Untuk me-nonaktifkan prefetching pada halaman yang terhubung, Anda dapat menggunakan prop `no-prefetch`. Sejak Nuxt.js v2.10.0, Anda juga dapat menggunakan prop `prefetch` yang di set `false`:

```html
<n-link to="/about" no-prefetch>Halaman tentang kami (not pre-fetched)</n-link>
<n-link to="/about" :prefetch="false">Halaman tentang kami (not pre-fetched)</n-link>
```

Anda dapat mengonfigurasi perilaku ini secara global dengan [router.prefetchLinks](/api/configuration-router#prefetchlinks).

Sejak Nuxt.js v2.10.0, jika Anda mengatur [router.prefetchLinks](/api/configuration-router#prefetchlinks) menjadi `false` secara global tetapi Anda ingin mem-prefetch spesifik link, Anda dapat menggunakan prop `prefetch`:

```html
<n-link to="/about" prefetch>Halaman Tentang kami (pre-fetched)</n-link>
```

Prop `prefetched-class` juga tersedia untuk mengkustomisasi kelas (clas) yang ditambahkan ketika pemecahan kode (code splitted) halaman sudah ter-prefetch (prefetched). Pastikan untuk mengatur fungsi ini secara global dengan [router.linkPrefetchedClass](/api/configuration-router#linkprefetchedclass).
