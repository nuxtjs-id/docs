---
title: 'API: Properti generate'
description: Konfigurasikan generasi aplikasi web universal Anda ke aplikasi web statis.
---

- Type: `Object`

> Konfigurasikan generasi aplikasi web universal Anda ke aplikasi web statis.

Ketika menjalankan `nuxt generate` atau `nuxt.generate()`, Nuxt.js akan menggunakan konfigurasi yang sudah didefinisikan didalam properti `generate`.

nuxt.config.js 
```js
export default {
  generate: {
    ...
  }
}
```

## concurrency

- Type: `Number`
- Default: `500`

Generasi rute konkuren, `generate.concurrency` menentukan jumlah rute yang berjalan dalam satu thread.


## dir

- Type: `String`
- Default: `'dist'`

Nama direktori dibuat oleh `nuxt generate`.

## devtools

- Type: `boolean`
- Default: `false`

Konfigurasikan apakah akan mengizinkan inspeksi [vue-devtools](https://github.com/vuejs/vue-devtools).

Jika Anda sudah mengaktifkannya melalui nuxt.config.js ataupun yang lainnya, devtools akan mengaktifkan apa pun sesuai flag.

## exclude

- Type: `Array`

Ini menerima array ekspresi reguler (regex) dan akan mencegah pembuatan rute yang cocok dengan mereka. Rute masih dapat diakses saat `generate.fallback` digunakan.

Secara default, menjalankan `nuxt generate` akan otomatis membuat file untuk masing-masing rute.

```bash
-| dist/
---| index.html
---| ignore/
-----| about.html
-----| item.html
```

Saat menambahkan ekspresi reguler (regex) untuk semua rute yang cocok dengan kata "ignore", itu akan mencegah pembuatan rute ini.

nuxt.config.js 
```js
export default {
  generate: {
    exclude: [
      /^(?=.*\bignore\b).*$/
    ]
  }
}
```

```bash
-| dist/
---| index.html
```

## fallback

- Type: `String` or `Boolean`
- Default: `200.html`

```js
export default {
  generate: {
    fallback: true
  }
}
```

Jalur fallback ke file HTML. Ini harus ditetapkan sebagai halaman eror, sehingga rute yang tidak dikenal juga dirender melalui Nuxt.
Jika tidak diatur atau diatur menjadi nilai falsy, nama file fallback HTML akan menjadi `200.html`. Jika `true`, maka nama file akan menjadi `404.html`. Jika Anda memberikan nilai string, maka akan digunakan sebagai gantinya.

Ketika menjalankan mode SPA akan lebih idiomatis menggunakan `200.html`, karena itu adalah satu-satunya file yang diperlukan ketika tidak ada rute lain yang dihasilkan.

```js
fallback: false
```

Jika bekerja dengan halaman yang dihasilkan secara statis maka disarankan untuk menggunakan `404.html` untuk menampilkan halaman eror yang dicakup oleh [excludes](https://nuxtjs.org/api/configuration-generate#exclude) (file yang tidak ingin Anda hasilkan sebagai halaman statis).

```js
fallback: true
```

Namun, Nuxt memungkinkan Anda untuk mengonfigurasi halaman mana pun yang Anda suka, jadi jika Anda tidak ingin menggunakan `200.html` atau` 404.html` Anda dapat menambahkan string dan kemudian Anda hanya perlu memastikan bahwa Anda mengarahkan ulang ke halaman tersebut. Tentusaja ini tidak diperlukan dan lebih baik tetap di redirect ke `200.html`/`404.html`.

```js
fallback: 'fallbackPage.html'
```

 *Catatan: Berbagai layanan (seperti Netlify) mendeteksi file `404.html` secara otomatis. Jika Anda mengkonfigurasi server web Anda sendiri, silakan baca dokumentasinya untuk mengetahui cara mengatur halaman eror (dan atur itu supaya mengarah ke file `404.html`) * 
 
## interval

- Type: `Number`
- Default: `0`

Interval antara dua siklus render untuk menghindari potensi membanjiri API dengan panggilan API dari aplikasi web.

## minify

- **Deprecated!**
- Gunakan [build.html.minify](/api/configuration-build#html-minify) sebagai gantinya

## routes

- Type: `Array`

[Dynamic routes](/guide/routing#dynamic-routes) akan diabaikan ketika menggunakan perintah `generate` (yarn generate). Nuxt tidak akan tahu rute seperti apa, sehingga tidak dapat melakukan generate.

Contoh:

```bash
-| pages/
---| index.vue
---| users/
-----| _id.vue
```

Hanya rute `/` yang akan di generate oleh Nuxt.js.

Apabila Anda ingin Nuxt.js melakukan generate dengan parameter dinamis, Anda harus mengatur properti `generate.routes` menjadi rute dinamis berbentuk array.

Kita tambahkan rute untuk `/users/:id` didalam `nuxt.config.js`:

```js
export default {
  generate: {
    routes: [
      '/users/1',
      '/users/2',
      '/users/3'
    ]
  }
}
```

Kemudian ketika kita menjalankan `nuxt generate`:

```bash
[nuxt] Generating...
[...]
nuxt:render Rendering url / +154ms
nuxt:render Rendering url /users/1 +12ms
nuxt:render Rendering url /users/2 +33ms
nuxt:render Rendering url /users/3 +7ms
nuxt:generate Generate file: /index.html +21ms
nuxt:generate Generate file: /users/1/index.html +31ms
nuxt:generate Generate file: /users/2/index.html +15ms
nuxt:generate Generate file: /users/3/index.html +23ms
nuxt:generate HTML Files generated in 7.6s +6ms
[nuxt] Generate done
```

Mantap, tapi bagaimana jika kita mempunyai **parameter dinamis**?

1. Gunakan `Function` yang mengembalikan (return) `Promise`.
2. Gunakan `Function` dengan `callback(err, params)`.

### Fungsi yang mengembalikan Promise

`nuxt.config.js`

```js
import axios from 'axios'

export default {
  generate: {
    routes () {
      return axios.get('https://my-api/users')
        .then((res) => {
          return res.data.map((user) => {
            return '/users/' + user.id
          })
        })
    }
  }
}
```

### Fungsi dengan callback

`nuxt.config.js`

```js
import axios from 'axios'

export default {
  generate: {
    routes (callback) {
      axios.get('https://my-api/users')
        .then((res) => {
          const routes = res.data.map((user) => {
            return '/users/' + user.id
          })
          callback(null, routes)
        })
        .catch(callback)
    }
  }
}
```

### Mempercepat proses generate dynamic route menggunakan `payload`

Pada contoh di atas, kita menggunakan `user.id` dari server untuk melakukan `generate` rute, tetapi ini akan menyia-nyiakan data lainnya. Biasanya, kita perlu melakukan `fetch` kembali di dalam file `/users/_id.vue`. Sementara kita melakukan itu, kita bisa juga mengatur `generate.interval` menjadi `100` supaya tidak membanjiri server. Karena ini akan mempercepat waktu dari proses `generate`, dan lebih baik untuk melewati objek `user` pada konteks didalam file `_id.vue`. Kita akan melakukannya dengan memodifikasi kode di atas menjadi seperti berikut:

`nuxt.config.js`

```js
import axios from 'axios'

export default {
  generate: {
    routes () {
      return axios.get('https://my-api/users')
        .then((res) => {
          return res.data.map((user) => {
            return {
              route: '/users/' + user.id,
              payload: user
            }
          })
        })
    }
  }
}
```

Sekarang kita dapat mengakses `payload` dari `/users/_id.vue` seperti:

```js
async asyncData ({ params, error, payload }) {
  if (payload) return { user: payload }
  else return { user: await backend.fetchUser(params.id) }
}
```

## subFolders

- Type: `Boolean`
- Default: `true`

Secara default, menjalankan `nuxt generate` akan membuat direktori untuk masing-masing rute dan melakukan serve file `index.html`.

Contoh:

```bash
-| dist/
---| index.html
---| about/
-----| index.html
---| products/
-----| item/
-------| index.html
```

Ketika di set false, File-file HTML yang dihasilkan akan sesuai dengan jalur rute:

nuxt.config.js 
```js
export default {
  generate: {
    subFolders: false
  }
}
```

```bash
-| dist/
---| index.html
---| about.html
---| products/
-----| item.html
```



