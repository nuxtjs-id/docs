---
title: "API: Properti env"
description: Berbagi variabel lingkungan (environment) antara klien dan server.
---

- Type: `Object`

> Nuxt.js memungkinkan Anda membuat variabel lingkungan (environment) yang akan dibagikan untuk klien dan sisi-server.

Properti env mendefinisikan variabel lingkungan yang harus tersedia di sisi klien. env tersebut dapat di set menggunakan variabel lingkungan dari sisi server, [modul dotenv](https://github.com/nuxt-community/dotenv-module) salah satunya.

**Pastikan untuk memahami `process.env` dan `process.env == {}` di bawah ini untuk penyelesaian masalah lebih baik.**

Contoh (`nuxt.config.js`):

```js
export default {
  env: {
    baseUrl: process.env.BASE_URL || 'http://localhost:3000'
  }
}
```

Ini memungkin kan Anda membuat properti `baseUrl` yang akan sama dengan env variable `BASE_URL` yang ada di sisi server jika tersedia atau terdefinisi. Jika tidak, `baseUrl` pada sisi klien akan sama dengan `'http://localhost:3000'`. Oleh sebab itu variabel BASE_URL dari sisi server disalin ke sisi klien melalui properti `env` di dalam `nuxt.config.js`. 
Atau, nilai lainnya didefinisikan (http://localhost:3000). 

Kemudian, Kita dapat mengakses variabel `baseUrl` dalam 2 cara:

1. Melalui `process.env.baseUrl`.
2. Melalui `context.env.baseUrl`, lihat [Api konteks](/api/context).

Sebagai contoh, Anda dapat menggunakan properti `env` untuk memberikan token publik.

Untuk contoh diatas, kita dapat menggunakannya untuk mengkonfigurasikan [axios](https://github.com/mzabriskie/axios).

`plugins/axios.js`:

```js
import axios from 'axios'

export default axios.create({
  baseURL: process.env.baseUrl
})
```

Kemudian, didalam halaman Anda dapat memasukan (impor) axios seperti ini: `import axios from '~/plugins/axios'`

## Menginjek otomatis variabel lingkungan (environment variables)

Jika Anda mendefinisikan variabel lingkungan yang dimulai dengan `NUXT_ENV_` pada tahapan build (misalnya `NUXT_ENV_COOL_WORD=freezing nuxt build`, maka akan secara otomatis di injek kedalam proses environtment. Hati-hati bahwa cara ini akan diutamakan daripada variabel yang sudah didefinisikan di `nuxt.config.js` dengan nama yang sama.

## process.env == {}

Sebagai catatan bahwa Nuxt menggunakan `definePlugin` webpack untuk menentukan variabel lingkungan. Berarti `process` atau `process.env` dari Node.js tidak tersedia atau terdefinisi. Masing-masing properti `env` terdefinisi di dalam nuxt.config.js secara individual yang dipetakan menjadi `process.env.xxxx` dan dikonversi selama kompilasi.

Maksudnya, `console.log(process.env)` akan menghasilkan `{}` tetapi `console.log(process.env.your_var)` akan menghasilkan nilai yang Anda tentukan. Ketika webpack mengkompile kode Anda, itu akan menggantikan semua instance pada `process.env.your_var` menjadi nilai yang Anda set. sebagai contoh: `env.test = 'testing123'`. Jika Anda menggunakan `process.env.test` didalam kode dimanapun itu, akan di terjemahkan menjadi 'testing123'.

before

```js
if (process.env.test == 'testing123')
```

after

```js
if ('testing123' == 'testing123')
```

## serverMiddleware

Karena [serverMiddleware](/api/configuration-servermiddleware) terpisah dari build Nuxt (main Nuxt build), maka variabel-variabel `env` yang terdefinisi di dalam `nuxt.config.js` tidak akan tersedia disana.
