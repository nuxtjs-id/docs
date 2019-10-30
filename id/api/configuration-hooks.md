---
title: "API: Properti hooks"
description: Hooks adalah event listener di Nuxt yang biasanya digunakan dalam modul Nuxt, tetapi juga tersedia di `nuxt.config.js`.
---

- Type: `Object`

> Hooks adalah [event listener di Nuxt](/api/internals) yang biasanya digunakan dalam modul Nuxt, tetapi juga tersedia di `nuxt.config.js`. [Learn More](/api/internals)

Contoh (`nuxt.config.js`):

```js
import fs from 'fs'
import path from 'path'

export default {
  hooks: {
    build: {
      done (builder) {
        const extraFilePath = path.join(builder.nuxt.options.buildDir, 'extra-file')
        fs.writeFileSync(extraFilePath, 'Something extra')
      }
    }
  }
}
```

Secara internal, hook mengikuti pola penamaan menggunakan titik dua (seperti: `build:done`). Demi kemudahan konfigurasi, Anda dapat menyusunnya sebagai objek hierarkis saat menggunakan `nuxt.config.js` (seperti contoh diatas) untuk mengatur hook Anda sendiri. Lihat [Internal Nuxt](/api/internals) untuk info detail bagaimana dia bekerja.

## Daftar hooks

- [Nuxt hooks](https://nuxtjs.org/api/internals-nuxt#hooks)
- [Renderer hooks](https://nuxtjs.org/api/internals-renderer#hooks)
- [ModulesContainer hooks](https://nuxtjs.org/api/internals-module-container#hooks)
- [Builder hooks](https://nuxtjs.org/api/internals-builder#hooks)
- [Generator hooks](https://nuxtjs.org/api/internals-generator#hooks)

## Contoh

### Redirect ke router.base ketika tidak di dalam root

Katakanlah Anda ingin melakukan serve halaman `/portal` dari pada `/`.

Mungkin ini adalah edge-case, dan poin dari _nuxt.config.js_’ `router.base` adalah ketika server Web akan melakukan serve Nuxt pada tempat lain selain dari domain root.

Tetapi ketika pengembangan lokal, akan menjadi _localhost_, ketika router.base tidak / mengembalikan 404.
Untuk mencegah hal ini, Anda dapat mengatur Hook.

Mungkin dengan melakukan redirect bukan hal terbaik bagi situs web (production), tetapi ini akan membantu Anda meningkatkan Hooks.

Untuk memulai, Anda [dapat merubah `router.base`](/api/configuration-router#base); Update `nuxt.config.js` milik Anda:

```js
// nuxt.config.js
import hooks from './hooks'
export default {
  router: {
    base: '/portal'
  }
  hooks: hooks(this)
}
```

Kemudian, buat beberapa file;

1. `hooks/index.js`, Hooks module

   ```js
   // file: hooks/index.js
   import render from './render'

   export default nuxtConfig => ({
     render: render(nuxtConfig)
   })
   ```

2. `hooks/render.js`, Render hook

   ```js
   // file: hooks/render.js
   import redirectRootToPortal from './route-redirect-portal'

   export default (nuxtConfig) => {
     const router = Reflect.has(nuxtConfig, 'router') ? nuxtConfig.router : {}
     const base = Reflect.has(router, 'base') ? router.base : '/portal'

     return {
       /**
        * 'render:setupMiddleware'
        * {@link node_modules/nuxt/lib/core/renderer.js}
        */
       setupMiddleware (app) {
         app.use('/', redirectRootToPortal(base))
       }
     }
   }
   ```

3. `hooks/route-redirect-portal.js`, Middleware nya sendiri

   ```js
   // file: hooks/route-redirect-portal.js

   /**
    * Nuxt middleware hook untuk mengalihkan dari / ke /portal (atau apa pun yang kita atur di nuxt.config.js router.base)
    *
    * Hendaknya masih dalam versi yang sama sebagai penghubung
    * {@link node_modules/connect/package.json}
    */
   import parseurl from 'parseurl'

   /**
    * Hubungkan middleware untuk menangani pengalihan ke Root Konteks Aplikasi Web yang diinginkan.
    *
    * Perhatikan bahwa Nuxt docs kurang menjelaskan cara menggunakan hooks.
    * Berikut ini adalah contoh router untuk membantu menjelaskan.
    *
    * Lihat implementasi yang bagus berikut ini sebagai inspirasi:
    * - https://github.com/nuxt/nuxt.js/blob/dev/examples/with-cookies/plugins/cookies.js
    * - https://github.com/yyx990803/launch-editor/blob/master/packages/launch-editor-middleware/index.js
    *
    * [http_class_http_clientrequest]: https://nodejs.org/api/http.html#http_class_http_clientrequest
    * [http_class_http_serverresponse]: https://nodejs.org/api/http.html#http_class_http_serverresponse
    *
    * @param {http.ClientRequest} req Node.js internal client request object [http_class_http_clientrequest]
    * @param {http.ServerResponse} res Node.js internal response [http_class_http_serverresponse]
    * @param {Function} next middleware callback
    */
   export default desiredContextRoot =>
     function projectHooksRouteRedirectPortal (req, res, next) {
       const desiredContextRootRegExp = new RegExp(`^${desiredContextRoot}`)
       const _parsedUrl = Reflect.has(req, '_parsedUrl') ? req._parsedUrl : null
       const url = _parsedUrl !== null ? _parsedUrl : parseurl(req)
       const startsWithDesired = desiredContextRootRegExp.test(url.pathname)
       const isNotProperContextRoot = desiredContextRoot !== url.pathname
       if (isNotProperContextRoot && startsWithDesired === false) {
         const pathname = url.pathname === null ? '' : url.pathname
         const search = url.search === null ? '' : url.search
         const Location = desiredContextRoot + pathname + search
         res.writeHead(302, {
           Location
         })
         res.end()
       }
       next()
     }
   ```

Lalu, setiap kali seorang dalam pengembangan secara tidak sengaja mengunjungi `/` untuk mengakses layanan pengembangan web, Nuxt akan secara otomatis melakukan redirect ke `/portal`
