---
title: Alat Pengembangan
description: Nuxt.js membantu Anda membuat pengembangan web Anda menyenangkan.
---

> Menguji aplikasi Anda adalah bagian dari pengembangan web. Nuxt.js membantu Anda membuatnya semudah mungkin.

## Pengujian End-to-End

[AVA](https://github.com/avajs/ava) adalah kerangka kerja pengujian JavaScript powerful, digabungkan dengan [jsdom](https://github.com/tmpvar/jsdom), kita dapat menggunakannya untuk melakukan pengujian end-to-end dengan mudah.

Pertama, kita perlu menambahkan AVA dan jsdom sebagai dependensi pengembangan:

```bash
npm install --save-dev ava jsdom
```

Kemudian tambahkan skrip pengujian ke `package.json` dan konfigurasikan AVA untuk mengkompilasi file yang kami impor ke dalam pengujian kita.

```javascript
"scripts": {
  "test": "ava",
},
"ava": {
  "files": [
    "test/**/*"
  ]
}
```

Kita akan menulis tes di folder `test`:

```bash
mkdir test
```

Katakanlah kita memiliki halaman di `halaman/index.vue`:

```html
<template>
  <h1 class="red">Hello {{ name }}!</h1>
</template>

<script>
export default {
  data () {
    return { name: 'world' }
  }
}
</script>

<style>
.red {
  color: red;
}
</style>
```

Ketika kita meluncurkan aplikasi kita dengan `npm run dev` dan membuka http://localhost:3000, kita dapat melihat judul `Hello world!` Merah kita.

Kita tambahkan file test `test/index.test.js`:

```js
import { resolve } from 'path'
import test from 'ava'
import { Nuxt, Builder } from 'nuxt'

// Init Nuxt.js dan mulai listening pada localhost:4000
test.before('Init Nuxt.js', async (t) => {
  const rootDir = resolve(__dirname, '..')
  let config = {}
  try { config = require(resolve(rootDir, 'nuxt.config.js')) } catch (e) {}
  config.rootDir = rootDir // project folder
  config.dev = false // production build
  config.mode = 'universal' // Isomorphic application
  const nuxt = new Nuxt(config)
  t.context.nuxt = nuxt // Kita menyimpan referensi ke Nuxt sehingga kita dapat menutup server pada akhir tes
  await new Builder(nuxt).build()
  nuxt.listen(4000, 'localhost')
})

// Contoh pengujian hanya melakukan generate html
test('Route / exists and render HTML', async (t) => {
  const { nuxt } = t.context
  const context = {}
  const { html } = await nuxt.renderRoute('/', context)
  t.true(html.includes('<h1 class="red">Hello world!</h1>'))
})

// Contoh pengujian melalui pemeriksaan DOM
test('Route / exists and renders HTML with CSS applied', async (t) => {
  const { nuxt } = t.context
  const window = await nuxt.renderAndGetWindow('http://localhost:4000/')
  const element = window.document.querySelector('.red')
  t.not(element, null)
  t.is(element.textContent, 'Hello world!')
  t.is(element.className, 'red')
  t.is(window.getComputedStyle(element).color, 'red')
})

// Tutup Nuxt server
test.after('Closing server', (t) => {
  const { nuxt } = t.context
  nuxt.close()
})
```

Sekarang kita dapat meluncurkan tes:

```bash
npm test
```

jsdom memiliki beberapa keterbatasan karena tidak menggunakan browser. Namun, itu akan mencakup sebagian besar pengujian kita. Jika Anda ingin menggunakan browser untuk menguji aplikasi Anda, Anda mungkin perlu memeriksa [Nightwatch.js](http://nightwatchjs.org).

## ESLint and Prettier

> [ESLint](http://eslint.org) adalah alat yang bagus untuk menjaga kode Anda tetap bersih.

> [Prettier](https://prettier.io) adalah formatter kode yang sangat populer.

Anda dapat menambahkan ESLint dengan Prettier dengan cukup mudah pada Nuxt.js, pertama, Anda perlu menambahkan dependensi npm:

```bash
npm install --save-dev babel-eslint eslint eslint-config-prettier eslint-loader eslint-plugin-vue eslint-plugin-prettier prettier
```

Kemudian, Anda dapat mengkonfigurasi ESLint melalui file `.eslintrc.js` di dalam direktori root project anda:
```js
module.exports = {
  root: true,
  env: {
    browser: true,
    node: true
  },
  parserOptions: {
    parser: 'babel-eslint'
  },
  extends: [
    'eslint:recommended',
    // https://github.com/vuejs/eslint-plugin-vue#priority-a-essential-error-prevention
    // pertimbangkan untuk beralih ke `plugin:vue/strongly-recommended` atau `plugin:vue/recommended` untuk aturan yang lebih ketat.
    'plugin:vue/recommended',
    'plugin:prettier/recommended'
  ],
  // diperlukan untuk lint file-file *.vue
  plugins: [
    'vue'
  ],
  // tambahkan custom rules anda disini
  rules: {
    'semi': [2, 'never'],
    'no-console': 'off',
    'vue/max-attributes-per-line': 'off',
    'prettier/prettier': ['error', { 'semi': false }]
  }
}
```

Kemudian, anda dapat menambahkan `lint` dan script `lintfix` untuk `package.json` Anda:

```js
"scripts": {
  "lint": "eslint --ext .js,.vue --ignore-path .gitignore .",
  "lintfix": "eslint --fix --ext .js,.vue --ignore-path .gitignore ."
}
```

Anda sekarang dapat meluncurkan `lint` untuk memeriksa kesalahan:

```bash
npm run lint
```

atau `lintfix` untuk juga memperbaiki apa yang bisa dilakukan

```bash
npm run lintfix
```

ESLint akan melakukan lint semua file JavaScript dan Vue Anda dan akan mengabaikan file yang Anda didefinisikan dalam `.gitignore`.

Juga disarankan untuk mengaktifkan mode hot-reload ESLint melalui webpack. Dengan cara ini ESLint akan berjalan pada save saat `npm run dev`. Cukup tambahkan script dibawah ini ke `nuxt.config.js`:

```js
...
  /*
   ** Build configuration
  */
  build: {
   /*
    ** You can extend webpack config here
   */
   extend(config, ctx) {
      // Run ESLint on save
      if (ctx.isDev && ctx.isClient) {
        config.module.rules.push({
          enforce: "pre",
          test: /\.(js|vue)$/,
          loader: "eslint-loader",
          exclude: /(node_modules)/
        })
      }
    }
  }
```

<div class="Alert Alert--orange">

Salah satu praktik terbaik adalah menambahkan juga `"precommit": "npm run lint"` di dalam package.json untuk melakukan lint kode Anda secara otomatis sebelum melakukan commit code Anda.

</div>
