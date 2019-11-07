---
title: Aset
description: Secara default, Nuxt menggunakan webpack loader vue-loader, file-loader, dan url-loader untuk penyajian aset yang kokoh. Anda juga dapat menggunakan direktori Static untuk kebutuhan aset statis.
---

> Secara default, Nuxt menggunakan webpack loader vue-loader, file-loader, dan url-loader untuk penyajian aset yang kokoh. Anda juga dapat menggunakan direktori Static untuk kebutuhan aset `statis`.

## Webpack

[vue-loader](http://vue-loader.vuejs.org/) secara otomatis memproses file style dan templat Anda dengan `css-loader` dan kompiler template Vue secara out of the box.
Dalam proses kompilasi ini, semua URL aset seperti `<img src="...">`, `background: url(...)` dan CSS `@import` diselesaikan sebagai dependensi modul.

Sebagai contoh, kita memiliki urutan file sebagai berikut:

```bash
-| assets/
----| image.png
-| pages/
----| index.vue
```

jika Anda menggunakan `url('~assets/image.png')` di dalam CSS Anda, akan *diterjemahkan* ke `require('~/assets/image.png')`.

<div class="Alert Alert--orange">

**Peringatan:** Mulai dari Nuxt 2.0 alias `~/` tidak akan diselesaikan dengan benar di file CSS Anda.
Anda harus menggunakan `~assets` (tanpa slash) atau alias `@` di dalam referensi CSS `url`, sebagai contoh: `background: url("~assets/banner.svg")`

</div>


Atau jika Anda merujuk gambar di dalam `pages/index.vue` Anda:

```html
<template>
  <img src="~/assets/image.png">
</template>
```

Ini akan di kompilasi menjadi:

```js
createElement('img', { attrs: { src: require('~/assets/image.png') } })
```

Karena `.png` bukan merupakan file JavaScript, Nuxt.js mengkonfigurasikan webpack untuk menggunakan [file-loader](https://github.com/webpack/file-loader) dan [url-loader](https://github.com/webpack/url-loader).

Manfaat dari loader ini adalah:

- `file-loader` memungkinkan Anda menentukan tempat untuk menyalin dan menempatkan file aset, dan bagaimana menamainya menggunakan versi hash untuk caching yang lebih baik. Dalam produksi, Anda akan mendapat manfaat dari caching jangka panjang secara default!
- `url-loader` memungkinkan Anda untuk menyatukan file sebagai URL data base-64 secara kondisional jika lebih kecil dari ambang yang diberikan. Ini dapat mengurangi sejumlah request HTTP untuk file yang ringan. Jika file lebih besar, otomatis akan dikembalikan ke file-loader.

Untuk kedua loader itu, konfigurasi defaultnya adalah:

```js
// https://github.com/nuxt/nuxt.js/blob/dev/packages/webpack/src/config/base.js#L297-L316
[
  {
    test: /\.(png|jpe?g|gif|svg|webp)$/,
    loader: 'url-loader',
    query: {
      limit: 1000, // 1kB
      name: 'img/[name].[hash:7].[ext]'
    }
  },
  {
    test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
    loader: 'url-loader',
    query: {
      limit: 1000, // 1kB
      name: 'fonts/[name].[hash:7].[ext]'
    }
  }
]
```

Yang berarti bahwa setiap file di bawah 1 KB akan diuraikan sebagai URL data base-64.
Jika tidak, gambar / font akan disalin dalam folder yang sesuai (di bawah direktori `.nuxt`)
dengan nama yang mengandung versi hash untuk caching yang lebih baik.

Ketika meluncurkan aplikasi kita dengan `nuxt`, templat kita di `pages/index.vue` sebagai berikut:

```html
<template>
  <img src="~/assets/image.png">
</template>
```

Akan diterjemahkan menjadi:

```html
<img src="/_nuxt/img/image.0c61159.png">
```

Jika Anda ingin mengubah konfigurasi loader, silahkan gunakan [build.extend](/api/configuration-build#extend).


## Static

Jika Anda tidak ingin menggunakan aset Webpack dari direktori `aset`, Anda dapat membuat dan menggunakan direktori `static` (di dalam folder root proyek Anda).

Semua file yang disertakan akan secara otomatis dilayani oleh Nuxt dan dapat diakses melalui URL root proyek Anda. (`static/favicon.ico` akan tersedia di `localhost:3000/favicon.ico`)

Opsi ini bermanfaat untuk file seperti `robots.txt`, `sitemap.xml` atau `CNAME` (yang penting untuk penyebaran (deploy) Halaman GitHub).

Dalam kode Anda, Anda dapat merujuk file-file ini relatif ke root (`/`):

```html
<!-- Gambar statis dari direktori statis -->
<img src="/my-image.png"/>

<!-- gambar webpacked dari direktori assets -->
<img src="~/assets/my-image-2.png"/>
```
