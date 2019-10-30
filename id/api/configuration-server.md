---
title: "API: Properti server"
description: Nuxt.js memungkinkan Anda menentukan variabel koneksi server untuk aplikasi Anda di dalam `nuxt.config.js`.
---

- Type: `Object`

> Nuxt.js memungkinkan Anda menentukan variabel koneksi server untuk aplikasi Anda di dalam `nuxt.config.js`.

## Contoh dasar (`nuxt.config.js`):

```js
export default {
  server: {
    port: 8000, // default: 3000
    host: '0.0.0.0', // default: localhost,
    timing: false
  }
}
```

Ini memungkinkan Anda menentukan [host dan port](/faq/host-port) untuk instance server Nuxt.js Anda.

## Contoh menggunakan konfigurasi HTTPS

```js
import path from 'path'
import fs from 'fs'

export default {
  server: {
    https: {
      key: fs.readFileSync(path.resolve(__dirname, 'server.key')),
      cert: fs.readFileSync(path.resolve(__dirname, 'server.crt'))
    }
  }
}
```

## Contoh menggunakan konfigurasi soket

```js
export default {
  server: {
    socket: '/tmp/nuxt.socket'
  }
}
```

## timing

- Type: `Object` or `Boolean`
- Default: `false`

Mengaktifkan opsi `server.timing` menambahkan middleware untuk mengukur waktu yang berlalu selama rendering sisi server dan menambahkannya ke header sebagai 'Server-Timing'

### Contoh menggunakan konfigurasi pengaturan waktu (timing)

`server.timing` dapat berupa objek untuk memberikan opsi. Saat ini, hanya `total` yang di support (yang secara langsung melacak seluruh waktu yang dihabiskan untuk rendering sisi server)

```js
export default {
  server: {
    timing: {
      total: true
    }
  }
}
```

### Menggunakan timing api

Api `timing` juga disuntikkan ke dalam `response` pada sisi server ketika `server.time` aktif.

#### Syntax

```js
res.timing.start(name, description)
res.timing.end(name)
```

#### Contoh menggunakan timing di servermiddleware

```js
export default function (req, res, next) {
  res.timing.start('midd', 'Middleware timing description')
  // pengoperasian pada sisi server..
  // ...
  res.timing.end('midd')
  next()
}
```

Kemudian head `server-timing` akan dimasukkan dalam header respons seperti:

```bash
Server-Timing: midd;desc="Middleware timing description";dur=2.4
```

Silakan merujuk [Server-Timing MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Server-Timing) untuk lebih jelasnya.
