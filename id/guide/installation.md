---
title: Instalasi
description: Sangat mudah untuk memulai Nuxt.js. Proyek sederhana hanya membutuhkan dependensi `nuxt`.
---

> Nuxt.js sangat mudah untuk memulai. Proyek sederhana hanya membutuhkan dependensi `nuxt`.

<div>
  <a href="https://vueschool.io/courses/nuxtjs-fundamentals/?friend=nuxt" target="_blank" class="Promote">
    <img src="/nuxt-fundamentals.png" srcset="/nuxt-fundamentals-2x.png 2x" alt="Nuxt Fundamentals by vueschool"/>
    <div class="Promote__Content">
      <h4 class="Promote__Content__Title">Nuxt.js Fundamentals</h4>
      <p class="Promote__Content__Description">Learn how to get started quickly with Nuxt.js in videos.</p>
      <p class="Promote__Content__Signature">Video courses made by VueSchool to support Nuxt.js development.</p>
    </div>
  </a>
</div>

## Using `create-nuxt-app`

Untuk bisa memulai dengan cepat, tim Nuxt.js telah menciptakan alat scaffolding [create-nuxt-app](https://github.com/nuxt/create-nuxt-app).

Pastikan anda sudah menginstal [npx](https://www.npmjs.com/package/npx) (`npx` ter-include secara default sejak NPM `5.2.0`)

```bash
$ npx create-nuxt-app <project-name>
```

Atau menggunakan [yarn](https://yarnpkg.com/en/):

```bash
$ yarn create nuxt-app <project-name>
```

Ini akan menanyakan beberapa pertanyaan kepada Anda:

1. Pilih antara kerangka kerja untuk integrasi dari sisi server:
  - None (Nuxt server default)
  - [Express](https://github.com/expressjs/express)
  - [Koa](https://github.com/koajs/koa)
  - [Hapi](https://github.com/hapijs/hapi)
  - [Feathers](https://github.com/feathersjs/feathers)
  - [Micro](https://github.com/zeit/micro)
  - [Fastify](https://github.com/fastify/fastify)
  - [Adonis](https://github.com/adonisjs/adonis-framework) (WIP)
2. Pilih kerangka UI favorit Anda:
  - None (anda bisa menambahkannya nanti)
  - [Bootstrap](https://github.com/bootstrap-vue/bootstrap-vue)
  - [Vuetify](https://github.com/vuetifyjs/vuetify)
  - [Bulma](https://github.com/jgthms/bulma)
  - [Tailwind](https://github.com/tailwindcss/tailwindcss)
  - [Element UI](https://github.com/ElemeFE/element)
  - [Ant Design Vue](https://github.com/vueComponent/ant-design-vue)
  - [Buefy](https://buefy.github.io)
  - [iView](https://www.iviewui.com/)
  - [Tachyons](https://tachyons.io)
3. Pilih kerangka pengujian favorit Anda:
  - None (anda bisa menambahkannya nanti)
  - [Jest](https://github.com/facebook/jest)
  - [AVA](https://github.com/avajs/ava)
4. [Mode Nuxt yang anda inginkan (`Universal` atau `SPA`)](https://nuxtjs.org/guide#single-page-applications-spa-)
5. Tambahkan [modul axios](https://github.com/nuxt-community/axios-module) untuk membuat HTTP request secara mudah ke dalam aplikasi Anda.
6. Tambahkan [EsLint](https://eslint.org/) to Lint your code on save.
7. Add [Prettier](https://prettier.io/) to prettify your code on save.

Ketika dijawab, itu akan menginstal semua dependensi sehingga langkah selanjutnya adalah menavigasi ke folder proyek dan meluncurkannya dengan:

```bash
$ cd <project-name>
$ npm run dev
```

Aplikasi akan berjalan di http://localhost:3000.

<div class="Alert">

Nuxt.js akan melakukan listen setiap perubahan file yang terjadi di dalam direktori <code>pages</code>, jadi tidak perlu me-restart aplikasi saat menambahkan halaman baru.

</div>

Untuk mengetahui lebih lanjut tentang struktur direktori proyek: [Dokumentasi Struktur Direktori](/guide/directory-structure).

## Mulai dari awal

Membuat proyek Nuxt.js dari awal sangatlah mudah, hanya memerlukan*1 file dan 1 direktori*. Buat direktori kosong untuk memulai:

```bash
$ mkdir <project-name>
$ cd <project-name>
```

<div class="Alert Alert--nuxt-green">

<b>Info:</b> ganti <code>&lt;project-name&gt;</nom-du-projet></code> dengan sebuah nama untuk proyek tersebut.

</div>

### package.json

Setiap proyek membutuhkan file `package.json` untuk bisa memulai `nuxt`. Salin json ini ke package.json Anda dan simpan sebelum menjalankan npm install (di bawah):

```json
{
  "name": "my-app",
  "scripts": {
    "dev": "nuxt"
  }
}
```

`scripts` akan meluncurkan Nuxt.js melalui `npm run dev`.

### Menginstal `nuxt`

Dengan dibuatnya `package.json`, tambahkan `nuxt` ke dalam proyek melalui:

```bash
$ npm install --save nuxt
```

### Direktori `pages`

Nuxt.js transforms every file `*.vue` di dalam direktori `pages` sebagai rute untuk aplikasi.

Buat direktori `pages`:

```bash
$ mkdir pages
```

lalu buat halaman pertama Anda di `pages/index.vue`:

```html
<template>
  <h1>Hello world!</h1>
</template>
```

dan jalankan proyek dengan:

```bash
$ npm run dev
```

Aplikasi sedang berjalan di http://localhost:3000.

<div class="Alert">

Nuxt.js akan melakukan listen setiap perubahan pada file di dalam direktori <code>pages</code>, jadi tidak perlu memulai ulang aplikasi saat menambahkan halaman baru.

</div>

Untuk mengetahui lebih lanjut tentang struktur direktori proyek: [Dokumentasi Struktur Direktori](/guide/directory-structure).
