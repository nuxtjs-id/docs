---
title: Pengenalan
description: "Nuxt adalah kerangka kerja progresif berdasarkan Vue.js untuk membuat aplikasi web modern. Ini didasarkan pada librari resmi Vue.js (vue, vue-router dan vuex) dan alat pengembangan yang solid (webpack, Babel dan PostCSS)."
---

> Nuxt adalah kerangka kerja progresif berdasarkan Vue.js untuk membuat aplikasi web modern. Ini didasarkan pada librari resmi Vue.js (vue, vue-router dan vuex) dan alat pengembangan yang solid (webpack, Babel dan PostCSS). Tujuan Nuxt adalah membuat pengembangan web yang solid dan berkinerja dengan pengalaman pengembang yang hebat.

## Apa itu NuxtJS?

Nuxt adalah kerangka kerja yang dirancang untuk memberi Anda arsitektur yang solid mengikuti panduan Vue resmi. Dapat diadopsi secara bertahap, dapat digunakan untuk membuat dari landing page statis ke aplikasi web yang siap digunakan untuk perusahaan.

Serbaguna secara alami, mendukung target yang berbeda (server, serverless atau statis) dan rendering sisi server dapat dialihkan.

Dapat diperluas dengan ekosistem modul yang kuat, membuatnya mudah untuk menghubungkan titik akhir REST atau GraphQL Anda, CMS favorit, kerangka kerja CSS dan banyak lagi. Dukungan PWA dan AMP hanya berjarak satu modul dari proyek Nuxt Anda.

NuxtJS adalah tulang punggung proyek Vue.js Anda, memberikan struktur untuk membangun proyek Anda dengan percaya diri sambil bersikap fleksibel.

## Fitur

- Menulis file Vue (`*.vue`)
- Code Splitting Otomatis
- Server-Side Rendering
- Sistem Routing Yang Powerful dengan Asynchronous Data
- Penyajian (serving) File Statis
- Transpilasi [ES2015+](https://babeljs.io/docs/en/learn/)
- Bundling dan minifying JS & CSS Anda
- Mengelola elemen `<head>` (`<title>`, `<meta>`, dll.)
- Penggantian hot-modul dalam Pengembangan
- Pre-processor: Sass, Less, Stylus, etc.
- HTTP/2 push header
- Memperluas dengan arsitektur Modular

## Bagaimana itu bekerja ?

Nuxt.js meliputi yang berikut ini untuk membuat pengembangan aplikasi web yang kaya:

- [Vue 2](https://vuejs.org/)
- [Vue Router](https://router.vuejs.org/en/)
- [Vuex](https://vuex.vuejs.org/en/) (hanya disertakan saat menggunakan [opsi store](/guide/vuex-store))
- [Vue Server Renderer](https://ssr.vuejs.org/en/) (dikecualikan saat menggunakan [`mode: 'spa'`](/api/configuration-mode))
- [Vue Meta](https://github.com/nuxt/vue-meta)

Totalnya hanya **57kB min+gzip** (60kB dengan Vuex).

<div class="Alert">

Di bawahnya kami menggunakan [webpack](https://github.com/webpack/webpack) dengan [vue-loader](https://github.com/vuejs/vue-loader) dan [babel-loader](https://github.com/babel/babel-loader) untuk membundel, memecah kode dan memperkecil kode Anda.

</div>

## Skema

Skema ini menunjukkan apa yang disebut oleh Nuxt.js ketika server dipanggil atau ketika pengguna menavigasi aplikasi melalui `<nuxt-link>`:

![nuxt-schema](/nuxt-schema.svg)

## Server Rendered (Universal SSR)

Anda dapat menggunakan Nuxt.js sebagai kerangka kerja untuk menangani semua rendering UI proyek Anda.

Ketika menjalankan `nuxt`, itu akan memulai server pengembangan dengan hot-reload dan [Vue Server Renderer](https://ssr.vuejs.org/en/) dikonfigurasikan untuk secara otomatis merender-server aplikasi Anda.

## Single Page Applications (SPA)

Jika, karena alasan apa pun, Anda memilih untuk tidak menggunakan rendering sisi server atau memerlukan hosting statis untuk aplikasi Anda, Anda cukup menggunakan mode SPA menggunakan `nuxt --spa`. Dikombinasikan dengan fitur *generate*, itu memberi Anda mekanisme penyebaran SPA yang kuat tanpa perlu menggunakan runtime Node.js atau penanganan server khusus.

Lihat [perintah (command)](/guide/commands) untuk mempelajari lebih lanjut tentang penggunaannya.

Jika Anda sudah memiliki server, Anda dapat memasang Nuxt.js dengan menggunakannya sebagai middleware. Tidak ada batasan sama sekali ketika menggunakan Nuxt.js untuk mengembangkan Aplikasi Web Universal Anda. Lihat panduan [Menggunakan Nuxt.js Secara programatis](/api/nuxt).

## Static Generated (Pre Rendering)

Inovasi besar Nuxt.js hadir dengan perintah `nuxt generate`.

Saat membangun aplikasi Anda, itu akan menghasilkan HTML untuk setiap rute Anda dan menyimpannya dalam file.

<div>
  <a href="https://vueschool.io/courses/static-site-generation-with-nuxtjs?friend=nuxt" target="_blank" class="Promote">
    <img src="/static-site-generation-with-nuxtjs.png" alt="Static Site Generation with Nuxt.js by vueschool"/>
    <div class="Promote__Content">
      <h4 class="Promote__Content__Title">Static Site Generation with Nuxt.js</h4>
      <p class="Promote__Content__Description">Learn how to generate static websites (pre rendering) to improve both performance and SEO while eliminating hosting costs.</p>
      <p class="Promote__Content__Signature">Video courses made by VueSchool to support Nuxt.js development.</p>
    </div>
  </a>
</div>

Sebagai contoh, struktur file berikut:

```bash
-| pages/
----| about.vue
----| index.vue
```

Akan digenerate menjadi:

```
-| dist/
----| about/
------| index.html
----| index.html
```

Dengan ini, Anda dapat meng-host aplikasi web yang dihasilkan di hosting statis apa pun!

Contoh terbaik adalah situs web ini yang digenerate dan di-host di [Netlify](https://www.netlify.com), cek [source code](https://github.com/nuxt/nuxtjs.org) kami atau [Bagaimana melakukan deploy Nuxt.js di Netlify](https://vueschool.io/lessons/how-to-deploy-nuxtjs-to-netlify?friend=nuxt) dari Vue School.

Kami tidak ingin membuat aplikasi secara manual setiap kali kami memperbarui [repositori dokumentasi](https://github.com/nuxt/docs), itu memicu hook ke Netlify yang:

1. Melakukan kloning [repositori nuxtjs.org](https://github.com/nuxt/nuxtjs.org)
2. Install dependensi melalui `npm install`
3. Jalankan `npm run generate`
4. Melakukan serve direktori `dist`

Sekarang kita memiliki **Aplikasi Web yang di Generate** secara otomatis :)

Kita dapat melangkah lebih jauh dengan memikirkan aplikasi web e-commerce yang dibuat dengan `nuxt generate` dan di-host pada sebuah CDN. Setiap kali produk kehabisan stok atau melakukan restock, kami membuat ulang aplikasi web. Tetapi jika pengguna menavigasi melalui aplikasi web pada saat itu, itu akan diperbarui berkat panggilan API yang dibuat ke API e-commerce. Tidak perlu lagi memiliki banyak instance server + cache!

<div class="Alert">

Lihat [Bagaimana melakukan deploy pada Netlify?](/faq/netlify-deployment) untuk detail lebih lanjut tentang cara melakukan deploy ke Netlify.

</div>
