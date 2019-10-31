---
title: "API: nuxt.renderRoute(route, context)"
description: Render rute spesifik dengan konteks yang diberikan.
---

- Type: `Function`
- Arguments:
  1. `String` : route untuk render
  2. *Opsional*, `Object`, context diberikan, keys yang tersedia: `req` & `res`
- Returns: `Promise`
  - `html`: `String`
  - `error`: `null` or `Object`
  - `redirected`: `false` or `Object`

> Render rute spesifik dengan konteks yang diberikan.

Metode ini harus digunakan sebagian besar untuk [tujuan pengujian](/guide/development-tools#end-to-end-testing) juga dengan [`nuxt.renderAndGetWindow`](/api/nuxt-render-and-get-window).

<div class="Alert Alert--orange">

`nuxt.renderRoute` harus dijalankan setelah proses build pada mode produksi (`dev: false`).

</div>

Contoh:

```js
const { Nuxt, Builder } = require('nuxt')

const config = require('./nuxt.config.js')
config.dev = false

const nuxt = new Nuxt(config)

new Builder(nuxt)
  .build()
  .then(() => nuxt.renderRoute('/'))
  .then(({ html, error, redirected }) => {
  // `html` akan selalu string

    // `error` bukan null ketika tata letak kesalahan ditampilkan, format kesalahan adalah:
    // { statusCode: 500, message: 'My error message' }

  // `redirected` bukan `false` ketika `redirect()` sudah digunakan di dalam `asyncData()` atau `fetch()`
  // { path: '/other-path', query: {}, status: 302 }
  })
```
