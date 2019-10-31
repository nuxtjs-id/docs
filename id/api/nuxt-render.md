---
title: "API: nuxt.render(req, res)"
description: Anda dapat menggunakan Nuxt.js sebagai middleware untuk server Node.js Anda.
---

- Type: `Function`
- Arguments:
  1. [Request](https://nodejs.org/api/http.html#http_class_http_incomingmessage)
  2. [Response](https://nodejs.org/api/http.html#http_class_http_serverresponse)
- Returns: `Promise`

> Anda dapat menggunakan Nuxt.js sebagai middleware dengan `nuxt.render` untuk server Node.js Anda.

Contoh ketika menggunakan [Express](https://github.com/expressjs/express):

```js
const { Nuxt, Builder } = require('nuxt')

const app = require('express')()
const isProd = (process.env.NODE_ENV === 'production')
const port = process.env.PORT || 3000

// Kita instantiate Nuxt.js dengan opsi
const config = require('./nuxt.config.js')
config.dev = !isProd
const nuxt = new Nuxt(config)

// Render setiap masing-masing rute dengan Nuxt.js
app.use(nuxt.render)

// Build hanya dalam mode dev dengan hot-reload
if (config.dev) {
  new Builder(nuxt).build()
    .then(listen)
} else {
  listen()
}

function listen () {
  // Listen server
  app.listen(port, '0.0.0.0')
  console.log('Server listening on `localhost:' + port + '`.')
}
```

<div class="Alert">

Direkomendasikan untuk memanggil `nuxt.render` pada akhir middleware Anda karena ia akan menangani rendering pada aplikasi web Anda dan tidak akan memanggil `next()`.

</div>
