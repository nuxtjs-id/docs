---
title: Modul
description: Modul adalah ekstensi Nuxt.js yang dapat memperluas fungsionalitas inti dari Nuxt.js dan menambahkan integrasi tanpa akhir.
---

> Modul adalah ekstensi Nuxt.js yang dapat memperluas fungsionalitas inti dari Nuxt.js dan menambahkan integrasi tanpa akhir.

## Pengenalan

Saat mengembangkan aplikasi pada tingkat produksi dengan Nuxt, Anda akan segera menemukan bahwa core framework
secara fungsional saja tidak cukup. Nuxt dapat diperluas dengan opsi konfigurasi dan plugin,
tetapi jika mempertahankan penyesuaian ini di berbagai proyek itu akan membosankan, berulang-ulang dan memakan waktu.
Di samping itu, mendukung setiap kebutuhan proyek secara langsung akan membuat Nuxt sangat kompleks dan sulit digunakan.

Ini adalah salah satu alasan mengapa Nuxt menyediakan sistem modul tingkat tinggi yang membuatnya mudah untuk memperluas core fungsi nya.
Modul adalah **functions** sederhana yang dipanggil secara berurutan ketika melakukan boot Nuxt.
Framework akan menunggu setiap modul selesai sebelum dapat melanjutkan.
Melalui ini, modul dapat menyesuaikan hampir semua aspek Nuxt.
Terimakasih untuk desain modular Nuxt (berbasis pada webpack's [Tapable](https://github.com/webpack/tapable)),
modul dapat dengan mudah mendaftarkan (register) hooks untuk titik masuk tertentu seperti inisialisasi builder.
Modules can also override templates, configure webpack loaders, add CSS libraries, and perform many other useful tasks.
Modul juga dapat melakukan overide template, mengkonfigurasi pemuat webpack, menambahkan librari CSS, dan melakukan banyak tugas bermanfaat lainnya.

Yang terbaik dari itu semua, Modul nuxt dapat dimasukkan ke dalam paket npm.
Ini membuatnya mudah untuk digunakan kembali di seluruh proyek dan untuk berbagi dengan komunitas Nuxt,
membantu menciptakan ekosistem add-ons Nuxt berkualitas tinggi.

Modul bagus jika Anda:

- Adalah anggota **agile team** yang perlu mem-bootstrap proyek baru secara cepat.
- Bosan **membuat secara berulang-ulang** tugas-tugas yang umum seperti mengintegrasikan Google Analytics.
- Adalah pecinta **Open Source** yang ingin dengan mudah **membagikan** pekerjaan anda dengan komunitas.
- Adalah anggota perusahaan **enterprise** yang mengedepankan nilai-nilai **kualitas** dan **dapat digunakan kembali**.
- Sering menghadapi tenggat waktu yang singkat dan tidak punya waktu untuk menggali rincian setiap librari atau integrasi baru.
- Bosan berurusan dengan perubahan antarmuka tingkat rendah, dan membutuhkan hal-hal yang hanya **sekedar berfungsi**.

## Daftar modul Nuxt.js

Tim Nuxt.js mempersembahkan modul **official**:
- [@nuxt/http](https://http.nuxtjs.org): Cara ringan dan universal untuk membuat request HTTP, berbasis pada [ky-universal](https://github.com/sindresorhus/ky-universal)
- [@nuxtjs/axios](https://axios.nuxtjs.org): Integrasi Axios yang Aman dan Mudah dengan Nuxt.js untuk membuat request HTTP
- [@nuxtjs/pwa](https://pwa.nuxtjs.org): Supercharge Nuxt dengan solusi PWA yang sangat teruji, diperbarui dan stabil
- [@nuxtjs/auth](https://auth.nuxtjs.org): Modul otentikasi untuk Nuxt.js, menawarkan skema dan strategi yang berbeda

Daftar modul Nuxt.js di atas dibuat oleh komunitas yang ada di https://github.com/topics/nuxt-module

## Menulis modul dasar

Seperti yang telah disebutkan modul hanyalah fungsi sederhana. Mereka dapat dikemas sebagai modul npm atau langsung dimasukkan dalam kode sumber proyek.

**modules/simple.js**

```js
export default function SimpleModule (moduleOptions) {
  // Tulis kode anda di sini
}

// Dibawah ini akan dibutuhkan (REQUIRED) jika menerbitkan modul sebagai paket npm
// module.exports.meta = require('./package.json')
```

**`moduleOptions`**

Ini adalah objek berbentuk array `modules`, kita dapat menggunakannya untuk menyesuaikan perilaku.

**`this.options`**

Anda dapat langsung mengakses opsi Nuxt menggunakan referensi ini. This is the content of the user's `nuxt.config.js` dengan semua opsi default yang ditetapkan. Dapat digunakan untuk opsi sharing antar modul.

**`this.nuxt`**

Ini adalah referensi ke instance Nuxt saat ini. Mengacu pada [Class Nuxt untuk metode yang tersedia](/api/internals-nuxt).

**`this`**

Konteks modul. Silakan lihat dokumentasi Class [ModuleContainer](/api/internals-module-container) untuk metode yang tersedia.

**`module.exports.meta`**

Baris ini **dibutuhkan** jika Anda menerbitkan modul sebagai paket npm. Nuxt secara internal menggunakan meta untuk bekerja lebih baik dengan paket Anda.

**nuxt.config.js**

```js
export default {
  modules: [
    // Penggunaan dasar
    '~/modules/simple'

    // Passing opsi secara langsung
      ['~/modules/simple', { token: '123' }]
  ]
}
```

Kemudian kita beritahu Nuxt untuk memuat beberapa modul spesifik untuk proyek dengan parameter opsional sebagai opsi.
Silakan merujuk ke dokumentasi [konfigurasi modul](/api/configuration-modules) untuk info detail!

## Modul Async

Tidak semua modul akan melakukan semuanya secara sinkron (synchronous). Misalnya Anda mungkin ingin mengembangkan modul yang perlu mengambil beberapa API atau melakukan IO async. Untuk ini, Nuxt mendukung modul async yang dapat mengembalikan Promise atau callback.

## Modul Build-only

Biasanya, modul hanya diperlukan selama pengembangan dan waktu build. Menggunakan bantuak `buildModules` untuk membuat production startup lebih cepat dan jugasecara signifikan mengurangi size `node_modules` untuk deploy production. Jika Anda seorang penulis modul, Sangat disarankan untuk mengarahkan pengguna menginstal paket Anda sebagai `devDependency` dan gunakan `buildModules` dari pada `modules` untuk `nuxt.config.js`.

Modul Anda adalah `buildModule` kecuali:
- Menyediakan serverMiddleware
- Harus mendaftarkan hooks runtime Node.js (Seperti sentry)
- Memengaruhi perilaku vue-renderer atau menggunakan hook dari `server:` atau namespace `vue-renderer:`
- Atau apapun diluar webpack scope (Hint: plugins and templates are compiled and are in webpack scope)

<div class="Alert Alert--orange">

<b>Catatan:</b> Jika Anda akan menggunakan <code>buildModules</code> harus di ingat bahwa fitur ini hanya tersedia sejak Nuxt <b>v2.9</b>. Untuk versi lama harus melakukan upgrade Nuxt atau menggunakan section <code>modules</code>.

</div>

### Gunakan async/await

```js
import fse from 'fs-extra'

export default async function asyncModule () {
  // Anda dapat melakukan pekerjaan async di sini menggunakan `async`/`await`
  const pages = await fse.readJson('./pages.json')
}
```

### Mengembalikan Promise

```js
import axios from 'axios'

export default function asyncModule () {
  return axios.get('https://jsonplaceholder.typicode.com/users')
    .then(res => res.data.map(user => '/users/' + user.username))
    .then((routes) => {
      // Lakukan sesuatu dengan melakukan extend Nuxt Route
    })
}
```

## Snippets Umum

### Opsi level Atas

Terkadang lebih nyaman jika kita dapat menggunakan opsi tingkat atas saat me-register modul `nuxt.config.js`.
Ini memungkinkan kita untuk menggabungkan beberapa sumber opsi.

**nuxt.config.js**

```js
export default {
  modules: [
    ['@nuxtjs/axios', { anotherOption: true }]
  ],

  // modul axios mengetahui hal ini dengan menggunakan `this.options.axios`
  axios: {
    option1,
    option2
  }
}
```

**module.js**

```js
export default function (moduleOptions) {
  // `options` akan berisi option1, option2 dan anotherOption
  const options = Object.assign({}, this.options.axios, moduleOptions)

  // ...
}
```

### Menyediakan plugin

Adalah umum bahwa modul menyediakan satu atau lebih plugin ketika ditambahkan.
Sebagai contoh modul [bootstrap-vue](https://bootstrap-vue.js.org), perlu mendaftar sendiri ke Vue.
Dalam situasi seperti itu kita dapat menggunakan bantuan `this.addPlugin`.

**plugin.js**

```js
import Vue from 'vue'
import BootstrapVue from 'bootstrap-vue/dist/bootstrap-vue.esm'

Vue.use(BootstrapVue)
```

**module.js**

```js
import path from 'path'

export default function nuxtBootstrapVue (moduleOptions) {
  // Register `plugin.js` template
  this.addPlugin(path.resolve(__dirname, 'plugin.js'))
}
```

### Plugin Template

Templat dan plugin yang sudah terdaftar akan meningkatkan [lodash templates](https://lodash.com/docs/4.17.4#template) untuk secara kondisional mengubah output plugin terdaftar.

**plugin.js**

```js
// Atur Google Analytics UA
ga('create', '<%= options.ua %>', 'auto')

<% if (options.debug) { %>
// Development code only
<% } %>
```

**module.js**

```js
import path from 'path'

export default function nuxtBootstrapVue (moduleOptions) {
  // Register `plugin.js` template
  this.addPlugin({
    src: path.resolve(__dirname, 'plugin.js'),
    options: {
      // Nuxt akan mengganti `options.ua` dengan `123` saat menyalin plugin ke proyek
      ua: 123,

      // sebagian kondisi dengan dev akan di strip dari kode plugin pada saat production build
      debug: this.options.dev
    }
  })
}
```

### Tambahkan Librari CSS

Jika modul Anda akan menyediakan librari CSS, pastikan untuk melakukan pemeriksaan jika pengguna sudah memasukkan librari untuk menghindari duplikasi, dan tambahkan **opsi untuk men-disable** librari CSS di dalam modul.

**module.js**

```js
export default function (moduleOptions) {
  if (moduleOptions.fontAwesome !== false) {
    // Tambahkan Font Awesome
    this.options.css.push('font-awesome/css/font-awesome.css')
  }
}
```

### Aset Emit

<!-- todo: up2date? -->

Kita dapat mendaftarkan plugin webpack untuk memancarkan aset selama build.

**module.js**

```js
export default function (moduleOptions) {
  const info = 'Built by awesome module - 1.3 alpha on ' + Date.now()

  this.options.build.plugins.push({
    apply (compiler) {
      compiler.plugin('emit', (compilation, cb) => {
        // Ini akan meng-generate `.nuxt/dist/info.txt' dengan isi variabel info.
        // Sumber juga bisa menjadi penyangga
        compilation.assets['info.txt'] = { source: () => info, size: () => info.length }

        cb()
      })
    }
  })
}
```

### Daftarkan loader webpack custom

Kita dapat melakukan hal yang sama seperti `build.extend` di dalam `nuxt.config.js` menggunakan `this.extendBuild`.

**module.js**

```js
export default function (moduleOptions) {
    this.extendBuild((config, { isClient, isServer }) => {
      // `.foo` Loader
      config.module.rules.push({
        test: /\.foo$/,
        use: [...]
      })

      // Customize existing loaders
      // Refer to source code for Nuxt internals:
      // https://github.com/nuxt/nuxt.js/tree/dev/packages/builder/src/webpack/base.js
      const barLoader = config.module.rules.find(rule => rule.loader === 'bar-loader')
  })
}
```

## Menjalankan Tugas pada spesifik hooks

Modul Anda mungkin perlu melakukan hal-hal hanya pada kondisi tertentu dan tidak hanya selama inisialisasi Nuxt.
Kita dapat menggunaka sistem Nuxt.js powerful [Hookable](https://github.com/nuxt/nuxt.js/blob/dev/packages/core/src/hookable.js) Nuxt.js untuk melakukan tugas pada acara tertentu.
Nuxt akan menunggu fungsi Anda jika mengembalikan Promise atau didefinisikan sebagai `async`.

Berikut ini beberapa contoh dasar:

```js
export default function myModule () {
  this.nuxt.hook('modules:done', (moduleContainer) => {
    // Ini akan dipanggil ketika semua modul selesai memuat
  })

  this.nuxt.hook('render:before', (renderer) => {
    // Dipanggil setelah penyaji (renderer) dibuat
  })

  this.nuxt.hook('build:compile', async ({ name, compiler }) => {
    // Dipanggil sebelum kompiler (default: webpack) dimulai
  })

  this.nuxt.hook('generate:before', async (generator) => {
    // Ini akan dipanggil sebelum Nuxt meng-generate halaman Anda
  })
}
```

## Perintah modul package

**Uji coba**

Mulai `v2.4.0`, Anda dapat menambahkan perintah nuxt khusus melalui paket modul Nuxt. Untuk melakukannya, Anda harus mengikuti API `NuxtCommand` saat mendefinisikan perintah Anda. Contoh sederhana secara hipotetis yang ditempatkan di `my-module/bin/command.js` terlihat seperti ini:

```js
#!/usr/bin/env node

const consola = require('consola')
const { NuxtCommand } = require('@nuxt/cli')

NuxtCommand.run({
  name: 'command',
  description: 'Perintah Modul Saya',
  usage: 'command <foobar>',
  options: {
    foobar: {
      alias: 'fb',
      type: 'string',
      description: 'Pengujian sederhana'
    }
  },
  run (cmd) {
    consola.info(cmd.argv)
  }
})
```

Beberapa hal yang perlu diperhatikan di sini. Pertama, perhatikan panggilan ke `/usr/bin/env` untuk mengambil Node yang dapat dieksekusi. Juga perhatikan bahwa sintaks modul ES tidak dapat digunakan untuk perintah kecuali jika Anda memasukkan [`esm`](https://github.com/standard-things/esm) secara manual ke dalam kode Anda.

Selanjutnya, Anda perhatikan bagaimana `NuxtCommand.run()` digunakan untuk menentukan pengaturan dan perilaku perintah. Opsi didefinisikan pada `options`, yang bisa diurai melalui [`minimist`](https://github.com/substack/minimist).
Setelah argumen diuraikan, `run()` secara otomatis dipanggil dengan instance `NuxtCommand` sebagai parameter pertama.

Pada contoh di atas, `cmd.argv` digunakan untuk mengambil argumen baris perintah parsed. Ada lebih banyak metode dan properti pada `NuxtCommand` -- dokumentasi tentang mereka akan diberikan nanti karena fitur ini masih diuji lebih lanjut dan diperbaiki.

Untuk membuat perintah Anda dikenali oleh Nuxt CLI, daftarkan di bawah section `bin` di package.json Anda, menggunakan konvensi `nuxt-module`, dimana `module` berkaitan dengan nama package Anda. Dengan binary sentral ini, anda dapat menggunakan `argv` untuk mem-parsing lebih lanjut `subcommands` untuk perintah Anda jika Anda menginginkannya.

```js
{
  "bin": {
    "nuxt-foobar": "./bin/command.js"
  }
}
```

Setelah paket Anda diinstal (melalui NPM atau Yarn), Anda dapat mengeksekusi `nuxt foobar ...` pada command-line.

<div class="Alert">

Ada banyak cara hooks dan kemungkinan untuk modul. Silahkan baca [Internal Nuxt](/api/internals) untuk mengetahui lebih lanjut tentang API nuxt-internal.

</div>
