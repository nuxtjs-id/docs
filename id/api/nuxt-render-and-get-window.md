---
title: "API: nuxt.renderAndGetWindow(url, options)"
description: Mendapatkan `window` dari URL yang diberikan Aplikasi Nuxt.js.
---

- Type: `Function`
- Argument: `String`
  1. `String`: URL to render
  2. *Optional*, `Object`: options
    - virtualConsole: `Boolean` (default: `true`)
- Returns: `Promise`
  - Returns: `window`

> Mendapatkan `window` dari URL yang diberikan Aplikasi Nuxt.js.

<div class="Alert Alert--orange">

Metode ini dibuat untuk [tujuan pengujian](/guide/development-tools#end-to-end-testing).

</div>

Untuk menggunakan fungsi ini, Anda harus menginstal `jsdom` terlebih dahulu:

```bash
npm install --save-dev jsdom
```

Contoh:

```js
const { Nuxt, Builder } = require('nuxt')

const config = require('./nuxt.config.js')
config.dev = false

const nuxt = new Nuxt(config)

nuxt.renderAndGetWindow('http://localhost:3000')
  .then((window) => {
  // Tampilkan head `<title>`
    console.log(window.document.title)
  })
```
