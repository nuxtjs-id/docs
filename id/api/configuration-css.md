---
title: "API: Properti css"
description: Nuxt.js memungkinkan Anda menentukan file/modules/libraries CSS yang
  ingin Anda atur secara global (disertakan pada setiap halaman).
---

> Nuxt.js memungkinkan Anda menentukan file/modules/libraries CSS yang ingin Anda atur secara global (disertakan pada setiap halaman).

Jika Anda ingin menggunakan `sass` pastikan Anda telah menginstal paket `node-sass` dan `sass-loader`. Jika belum, instal saja:

```sh
npm install --save-dev node-sass sass-loader
```

- Type: `Array`
 - Items: `string`

Pada `nuxt.config.js`, tambahkan resource CSS:

```js
export default {
  css: [
    // Masukan node module secara langsung (di sini adalah file SASS)
    'bulma',
    // file CSS di dalam proyek
    '@/assets/css/main.css',
    // file SCSS di dalam proyek
    '@/assets/css/main.scss'
  ]
}
```

Nuxt.js secara otomatis akan membaca jenis file dengan ekstensi itu dan menggunakan pemroses pra-prosesor yang sesuai untuk webpack. Anda masih perlu menginstal loader yang diperlukan jika Anda perlu menggunakannya.
