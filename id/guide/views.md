---
title: Tampilan (Views)
description: Bagian Tampilan (view) menjelaskan semua yang Anda perlukan untuk mengonfigurasi data dan tampilan untuk rute tertentu di Aplikasi Nuxt.js Anda (Dokumen, Tata Letak, Halaman, dan HTML head).
---

> Bagian Tampilan (view) menjelaskan semua yang Anda perlukan untuk mengonfigurasi data dan tampilan untuk rute tertentu di Aplikasi Nuxt.js Anda (Dokumen, Tata Letak, Halaman, dan HTML head).

![nuxt-views-schema](/nuxt-views-schema.svg)

## Template App

> Anda dapat menyesuaikan templat aplikasi HTML yang digunakan oleh Nuxt.js untuk menyertakan skrip atau class CSS kondisional.

Untuk mengubah templat, buat file `app.html` di folder src proyek Anda. (yang merupakan direktori root proyek secara default).


Template default yang digunakan oleh Nuxt.js adalah:

```html
<!DOCTYPE html>
<html {{ HTML_ATTRS }}>
  <head {{ HEAD_ATTRS }}>
    {{ HEAD }}
  </head>
  <body {{ BODY_ATTRS }}>
    {{ APP }}
  </body>
</html>
```

Satu kasus penggunaan menggunakan templat aplikasi khusus adalah menambahkan class CSS kondisional untuk IE:

```html
<!DOCTYPE html>
<!--[if IE 9]><html lang="en-US" class="lt-ie9 ie9" {{ HTML_ATTRS }}><![endif]-->
<!--[if (gt IE 9)|!(IE)]><!--><html {{ HTML_ATTRS }}><!--<![endif]-->
  <head {{ HEAD_ATTRS }}>
    {{ HEAD }}
  </head>
  <body {{ BODY_ATTRS }}>
    {{ APP }}
  </body>
</html>
```

<!-- TODO: Load polyfills here? -->

## Tata Letak (Layouts)

Tata letak sangat membantu ketika Anda ingin mengubah tampilan dan nuansa aplikasi Nuxt.js Anda.
Apakah Anda ingin menyertakan bilah sisi atau memiliki tata letak yang berbeda untuk seluler dan desktop

### Tata Letak Default (Default Layout)

Anda dapat memperluas tata letak utama (main layout) dengan menambahkan file `layouts/default.vue`.
Ini akan digunakan untuk semua halaman yang tidak memiliki tata letak yang ditentukan.

<div class="Alert Alert--nuxt-green">

<b>Info:</b> Pastikan untuk menambahkan komponen `<nuxt/>` saat membuat tata letak untuk memasukkan komponen halaman secara benar.

</div>

Tata letak default yang keluar dari kotak hanya tiga baris panjang dan cukup merender komponen halaman:

```html
<template>
  <nuxt/>
</template>
```

### Tata Letak Kustom (Custom Layout)

Every file (*top-level*) di dalam direktori `layouts` akan membuat tata letak kustom yang dapat diakses dengan properti `layout` pada komponen halaman.

Katakanlah kita ingin membuat tata letak blog dan menyimpannya ke `layouts/blog.vue`:

```html
<template>
  <div>
    <div>My blog navigation bar here</div>
    <nuxt/>
  </div>
</template>
```

Maka kita harus memberitahu halaman (i.e. `pages/posts.vue`) untuk menggunakan tata letak kustom Anda:

```html
<template>
<!-- Templat Anda -->
</template>
<script>
export default {
  layout: 'blog'
  // definisikan komponen halaman
}
</script>
```

Informasi lebih lanjut tentang properti `layout`: [API Halaman `layout`](/api/pages-layout)

Lihat [video demontrasi](https://www.youtube.com/watch?v=YOKnSTp7d38) untuk melihat tata letak kustom dalam aksi.

<!-- TODO: Scoped styles best practice -->

### Halaman eror

Halaman kesalahan adalah *komponen halaman* yang selalu ditampilkan ketika kesalahan terjadi (itu tidak terjadi saat rendering sisi server).

<div class="Alert Alert--orange">

<b>Peringatan:</b> Walaupun file ini ditempatkan di folder <code>layouts</code>, itu harus diperlakukan sebagai <b>halaman</b>.

</div>

Seperti disebutkan di atas, tata letak ini istimewa, semenjak Anda **tidak** memasukan `<nuxt/>` di dalam templatnya.
Anda harus melihat tata letak ini sebagai komponen yang ditampilkan ketika terjadi eror (`404`, `500`, dll.).
Mirip dengan komponen halaman lainnya, Anda dapat mengatur tata letak khusus untuk halaman eror juga dengan cara biasa.

Kode sumber halaman kesalahan default [tersedia di GitHub](https://github.com/nuxt/nuxt.js/blob/dev/packages/vue-app/template/components/nuxt-error.vue).

Anda dapat menyesuaikan halaman kesalahan dengan menambahkan file `layouts/error.vue`:

```html
<template>
  <div class="container">
    <h1 v-if="error.statusCode === 404">Halaman tidak ditemukan</h1>
    <h1 v-else>Terjadi kesalahan</h1>
    <nuxt-link to="/">Halaman utama</nuxt-link>
  </div>
</template>

<script>
export default {
  props: ['error'],
  layout: 'blog' // Anda dapat mengatur tata letak khusus untuk halaman eror
}
</script>
```


## Halaman (Pages)

Setiap komponen Halaman adalah komponen Vue tetapi Nuxt.js menambahkan atribut dan fungsi khusus untuk membuat pengembangan aplikasi universal Anda semudah mungkin.

<div class="Promo__Video">
  <a href="https://vueschool.io/lessons/nuxtjs-page-components?friend=nuxt" target="_blank">
    <p class="Promo__Video__Icon">
      Silahkan lihat pembelajaran gratis tentang <strong>Componen Halaman Nuxt.js</strong> di Vue School 
    </p>
  </a>
</div>

```html
<template>
  <h1 class="red">Hello {{ name }}!</h1>
</template>

<script>
export default {
  asyncData (context) {
    // dipanggil setiap kali sebelum memuat komponen
    // seperti namanya, itu bisa async
    // Juga, objek yang di-return akan digabungkan dengan objek data Anda
    return { name: 'World' }
  },
  fetch () {
    // Metode `fetch` digunakan untuk mengisi store sebelum merender halaman
  },
  head () {
    // Setel Tag Meta untuk Halaman ini
  },
  // dan lakukan dengan lebih banyak fungsi untuk dijelajahi
  ...
}
</script>

<style>
.red {
  color: red;
}
</style>
```

| Atribut | Penjelasan | Dokumentasi |
|-----------|-------------| ------------- |
| `asyncData` | Kunci yang paling penting. Bisa asinkron dan dapat menerima konteks sebagai argumen. | [Panduan: Async data](/guide/async-data) |
| `fetch` | Digunakan untuk mengisi store sebelum merender halaman. Ini seperti metode `asyncData`, kecuali mengatur data komponen. | [Halaman API `fetch`](/api/pages-fetch) |
| `head` | Atur tag `<meta>` spesifik untuk halaman saat ini. | [Halaman API `head`](/api/pages-head) |
| `layout` | Spesifikasikan layout yang terdefinisi di dalam direktori `layouts`. | [Halaman API `layout`](/api/pages-layout) |
| `loading` | Jika di set menjadi `false`, mencegah halaman untuk memanggil `this.$nuxt.$loading.finish()` secara otomatis saat Anda memasukkannya dan `this.$nuxt.$loading.start()` ketika Anda meninggalkannya, memungkinkan Anda untuk **secara manual** mengontrol tingkah laku, seperti yang ditampilkan pada [contoh ini](/examples/custom-page-loading). Hanya diterapkan apabila `loading` juga di atur di dalam `nuxt.config.js`. | [Konfigurasi API `loading`](/api/configuration-loading) |
| `transition` | Menentukan transisi kustom untuk halaman. | [Halaman API `transition`](/api/pages-transition) |
| `scrollToTop` | Boolean (default: `false`). Tentukan apakah Anda ingin halaman tersebut melakukan scroll ke atas sebelum merender halaman. Ini digunakan untuk [route bersarang](/guide/routing#nested-routes). | [Halaman API `scrollToTop`](/api/pages-scrolltotop#the-scrolltotop-property) |
| `validate` | Fungsi validator untuk [route dinamis](/guide/routing#dynamic-routes). | [Halaman API `validate`](/api/pages-validate#the-validate-method) |
| `middleware` | Tentukan middleware untuk halaman ini. Middleware akan dipanggil sebelum merender halaman. | [Panduan: middleware](/guide/routing#middleware) |

Informasi lebih lanjut tentang penggunaan properti halaman: [Halaman API](/api)

## HTML Head

Nuxt.js menggunakan [vue-meta](https://vue-meta.nuxtjs.org/) untuk melakukan update `document head` dan `meta attributes` pada aplikasi anda.

Penggunaan `vue-meta` Nuxt.js dapat anda lihat [di GitHub](https://github.com/nuxt/nuxt.js/blob/dev/packages/vue-app/template/index.js#L42-L48).

<div class="Alert Alert--teal">

<b>Info:</b> Nuxt.js menggunakan <code>hid</code> daripada menggunakan default key <code>vmid</code> untuk mengidentifikasi meta elemen

</div>


### Default Meta Tags

Nuxt.js memungkinkan Anda mendefinisikan semua tag `<meta>` default untuk aplikasi Anda di dalam `nuxt.config.js`. Definisikan menggunakan properti `head` yang sama:

Contoh viewport khusus dengan font Google kustom:

```js
head: {
  meta: [
    { charset: 'utf-8' },
    { name: 'viewport', content: 'width=device-width, initial-scale=1' }
  ],
  link: [
    { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css?family=Roboto' }
  ]
}
```

Untuk mempelajari lebih lanjut tentang opsi yang tersedia untuk `head`, silahkan lihat [dokumentasi vue-meta](https://vue-meta.nuxtjs.org/api/#metainfo-properties).

Informasi lebih lanjut tentang metode `head` tersedia di halaman [Konfigurasi API `head`](/api/configuration-head).

### Meta Tag Kustom untuk Halaman

Informasi lebih lanjut tentang metode head dapat ditemukan di [Halaman API `head`](/api/pages-head).
