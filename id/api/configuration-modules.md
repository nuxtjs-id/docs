---
title: "API: Properti modules"
description: Modul adalah ekstensi Nuxt.js yang dapat memperluas inti dari fungsionalitas nya dan menambahkan integrasi tanpa akhir.
---

- Type: `Array`

> Modul-modul adalah ekstensi Nuxt.js yang dapat memperluas inti dari fungsionalitas nya dan menambahkan integrasi tanpa akhir.  [Pelajari lebih lanjut](/guide/modules)

Contoh (`nuxt.config.js`):

```js
export default {
  modules: [
    // Menggunakan nama paket (package)
    '@nuxtjs/axios',

    // Relatif untuk srcDir proyek Anda
    '~/modules/awesome.js',

    // Menyediakan opsi
    ['@nuxtjs/google-analytics', { ua: 'X1234567' }],

    // Definisi sebaris
    function () { }
  ]
}
```
Pengembang modul biasanya memberikan langkah-langkah tambahan yang diperlukan untuk penggunaan.

Nuxt.js mencoba menyelesaikan setiap item dalam array modul menggunakan node yang memerlukan path (di dalam `node_modules`) kemudian akan diselesaikan dari proyek `srcDir` jika alias `~` digunakan. Modul dijalankan secara berurutan, jadi urutannya akan bersifat penting.

Modul harus mengekspor fungsi (export function) untuk meningkatkan nuxt build/runtime dan secara opsional mengembalikan promise sampai pekerjaannya selesai.
Perhatikan bahwa modul-modul tersebut diperlukan pada saat runtime jadi harus sudah ditransmisikan jika bergantung kepada fitur ES6 modern.


Silahkan lihat [Panduan Modul](/guide/modules) untuk informasi lebih detail dan bagaimana mereka bekerja atau jika tertarik mengembangkan modul Anda sendiri.
Kami juga telah menyediakan section official [Modules](https://github.com/nuxt-community/awesome-nuxt#modules) daftar lusinan modul siap produksi yang dibuat oleh Nuxt Community.

## `buildModules`

<div class="Alert Alert--info">

Fitur ini tersedia sejak Nuxt v2.9

</div>

Beberapa modul hanya diperlukan selama pengembangan dan build time. Menggunakan `buildModules` akan membantu membuat produksi startup lebih cepat dan juga menurunkan ukuran `node_modules` secara signifikan untuk penyebaran produksi. Silakan merujuk ke setiap dokumen modul untuk melihat apakah disarankan untuk menggunakan `modules` atau `buildModules`.

Perbedaan penggunaannya adalah:

- Daripada menambahkan ke `modules` di dalam `nuxt.config.js`, lebih baik gunakan `buildModules`
- Daripada menambahkan ke `dependencies` di dalam `package.json`, gunakan `devDependencies` (`yarn add --dev` or `npm install --save-dev`)

