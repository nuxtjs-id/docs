---
title: "API: Properti render"
description: Nuxt.js memungkinkan Anda menyesuaikan opsi runtime untuk merender halaman
---

> Nuxt.js memungkinkan Anda menyesuaikan opsi runtime untuk merender halaman

## bundleRenderer

- Type: `Object`

> Gunakan opsi ini untuk menyesuaikan bundel SSR vue renderer. Opsi ini akan dilewati pada mode spa.

```js
export default {
  render: {
    bundleRenderer: {
      directives: {
        custom1 (el, dir) {
          // something ...
        }
      }
    }
  }
}
```

Pelajari lebih lanjut tentang opsi yang tersedia di [Referensi API SSR Vue](https://ssr.vuejs.org/en/api.html#renderer-options).
Disarankan untuk tidak menggunakan opsi ini karena Nuxt.js sudah memberikan standar SSR terbaik dan kesalahan konfigurasi dapat menyebabkan masalah SSR.

## etag

- Type: `Object`
  - Default: `{ weak: true }`

Untuk menonaktifkan etag untuk set halaman `etag: false`

Lihat dokumentasi [etag](https://www.npmjs.com/package/etag) untuk melihat opsi apa saja yang dipakai.

Anda dapat menggunakan fungsi hash Anda sendiri dengan menentukan `etag.hash`:

`nuxt.config.js`
```js
import { murmurHash128 } from 'murmurhash-native'

export default {
  render: {
    etag: {
      hash: html => murmurHash128(html)
    }
  }
}
```

Dalam hal ini kami menggunakan [murmurhash-native](https://github.com/royaltm/node-murmurhash-native), yang lebih cepat untuk ukuran body html yang lebih besar. Perhatikan bahwa opsi `weak` diabaikan ketika menentukan fungsi hash Anda sendiri.

## compressor

- Type `Object`
  - Default: `{ threshold: 0 }`

Pada saat menyediakan objek, middleware [compression](https://www.npmjs.com/package/compression) akan digunakan (dengan opsi masing-masing).

Jika Anda ingin menggunakan middleware compression Anda sendiri, Anda bisa merujuknya
secara lagsung (sebagai contoh: `otherComp({ myOptions: 'example' })`).

Untuk me-nonaktifkan compression, gunakan `compressor: false`.

## fallback

- Type `Object`
  - Default: `{ dist: {}, static: { skipUnknown: true } }`

> Opsi untuk middleware [serve-placeholder](https://github.com/nuxt/serve-placeholder).

Jika Anda ingin menonaktifkan salah satu atau keduanya, Anda bisa memberikan nilai (falsy).

## http2

- Type `Object`
  - Default: `{ push: false, pushAssets: null }`

> Aktifkan header push HTTP2.

Anda dapat mengontrol tautan apa yang akan di push menggunakan fungsi `pushAssets`.

Contoh:
```js
pushAssets: (req, res, publicPath, preloadFiles) => preloadFiles
  .filter(f => f.asType === 'script' && f.file === 'runtime.js')
  .map(f => `<${publicPath}${f.file}>; rel=preload; as=${f.asType}`)
```

Anda juga dapat menambahkan aset Anda sendiri ke array.
Menggunakan `req` dan `res` Anda dapat memutuskan tautan mana yang akan di push berdasarkan request header, misalnya menggunakan cookie dengan versi aplikasi.

Aset akan digabungkan bersama `,` dan diteruskan sebagai header `Link` tunggal.

## injectScripts

- Type: `Boolean`
  - Default: `true`

> Menambahkan `<script>` untuk bundel Nuxt, atur ke `false` untuk membuat HTML murni tanpa JS (tersedia pada versi `2.8.0+`)

## resourceHints

- Type: `Boolean`
  - Default: `true`

> Tambahkan link `prefetch` dan `preload` untuk waktu pemuatan halaman awal yang lebih cepat.

Anda mungkin ingin menonaktifkan opsi ini ketika Anda memiliki banyak halaman dan rute.

## ssr

- Type: `Boolean`
  - Default: `true` on universal mode and `false` on spa mode

> Mengaktifkan render SSR

Opsi ini secara otomatis ditetapkan berdasarkan nilai `mode` jika tidak disediakan.
Ini berguna untuk mengaktifkan / menonaktifkan SSR secara dinamis saat runtime setelah build image (misalnya, menggunakan docker).

## ssrLog

- Type: `Boolean` | `String`
  - Default: `true` dalam mode dev dan `false` dalam produksi

> Meneruskan log sisi-server ke browser untuk debugging yang lebih baik (hanya tersedia dalam mode pengembangan/dev)

Untuk menutup log, gunakan nilai `'collapsed'`.

## static

- Type: `Object`
  - Default: `{}`

> Konfigurasikan behaviour direktori `static /`

Lihat dokumentasi [serve-static](https://www.npmjs.com/package/serve-static) docs untuk opsi yang digunakan.

Sebagai tambahan, kami memperkenalkan opsi `prefix` yang secara default adalah `true`.
Ini akan menambah router base ke aset statis Anda.

**Contoh:**

* Assets: `favicon.ico`
* Router base: `/t`
* With `prefix: true` (default): `/t/favicon.ico`
* With `prefix: false`: `/favicon.ico`

**Peringatan:**

Beberapa penulisan ulang URL (URL reqrites) mungkin mengabaikan prefix.

## dist

- Type: `Object`
  - Default: `{ maxAge: '1y', index: false }`

> Opsi yang digunakan untuk menyajikan (serving) file distribusi. Hanya berlaku dalam mode produksi.

Lihat dokumentasi [serve-static](https://www.npmjs.com/package/serve-static) untuk opsi yang digunakan.

## csp

- Type: `Boolean` or `Object`
  - Default: `false`

> Gunakan ini untuk mengkonfigurasi ketika melakukan load (external resources) pada Content-Security-Policy

Perhatikan bahwa hash CSP tidak akan ditambahkan jika kebijakan `script-src` mengandung` 'unsafe-inline'`. 
Ini karena browser mengabaikan `'unsafe-inline'` apabila terdapat hash. Atur opsi `unsafeInlineCompatibility` menjadi `true` jika Anda ingin kedua hash dan `'unsafe-inline'` untuk kompatibilitas CSPv1.

Contoh (`nuxt.config.js`)

```js
export default {
  render: {
    csp: true
  }
}

// OR

export default {
  render: {
    csp: {
      hashAlgorithm: 'sha256',
      policies: {
        'script-src': [
          'https://www.google-analytics.com',
          'https://name.example.com'
        ],
        'report-uri': [
          'https://report.example.com/report-csp-violations'
        ]
      }
    }
  }
}

// OR 
/*
  Contoh berikut mengijinkan Google Analytics, LogRocket.io, dan Sentry.io
  untuk melakukan penelusuran (logging) dan pelacakan analitik.

  Tinjau blog ini di Sentry.io
  https://blog.sentry.io/2018/09/04/how-sentry-captures-csp-violations

  Untuk mempelajari tautan pelacakan apa yang harus Anda gunakan.
*/
const PRIMARY_HOSTS = `loc.example-website.com`
export default {
  csp: {
    reportOnly: true,
    hashAlgorithm: 'sha256',
    policies: {
      'default-src': ["'self'"],
      'img-src': ['https:', '*.google-analytics.com'],
      'worker-src': ["'self'", `blob:`, PRIMARY_HOSTS, '*.logrocket.io'],
      'style-src': ["'self'", "'unsafe-inline'", PRIMARY_HOSTS],
      'script-src': ["'self'", "'unsafe-inline'", PRIMARY_HOSTS, 'sentry.io', '*.sentry-cdn.com', '*.google-analytics.com', '*.logrocket.io'],
      'connect-src': [PRIMARY_HOSTS, 'sentry.io', '*.google-analytics.com'],
      'form-action': ["'self'"],
      'frame-ancestors': ["'none'"],
      'object-src': ["'none'"],
      'base-uri': [PRIMARY_HOSTS],
      'report-uri': [
        `https://sentry.io/api/<project>/security/?sentry_key=<key>`
      ]
    }
  }
}

```
