---
title: Async Data
description: Anda mungkin ingin mengambil data dan me-rendernya di sisi-server. Nuxt.js menambahkan metode `asyncData` untuk memungkinkan Anda menangani operasi async sebelum menyetel data komponen.
---

> Anda mungkin ingin mengambil data dan me-rendernya di sisi-server. Nuxt.js menambahkan metode `asyncData` untuk memungkinkan Anda menangani operasi async sebelum menyetel data komponen.

<div>
  <a href="https://vueschool.io/courses/async-data-with-nuxtjs?friend=nuxt" target="_blank" class="Promote">
    <img src="/async-data-with-nuxtjs.png" srcset="/async-data-with-nuxtjs-2x.png 2x" alt="AsyncData by vueschool"/>
    <div class="Promote__Content">
      <h4 class="Promote__Content__Title">Async Data pada Nuxt.js</h4>
      <p class="Promote__Content__Description">Pelajadi bagaimana mengatur asynchronous data pada Nuxt.js.</p>
      <p class="Promote__Content__Signature">Video kursus dibuat oleh VueSchool untuk mendukung pengembangan Nuxt.js.</p>
    </div>
  </a>
</div>

## Metode asyncData

Terkadang Anda hanya ingin mengambil data dan melakukan pre-render di server tanpa menggunakan store. 
`asyncData` dipanggil setiap kali sebelum memuat komponen **halaman**.
Ini akan dipanggil sisi server sekali (pada saat melakukan request pertama ke aplikasi Nuxt) dan dipanggil pada sisi klien ketika menavigasi ke rute selanjutnya. 
Metode ini menerima [konteks](/api/context) sebagai argumen pertama, Anda dapat menggunakannya untuk mengambil beberapa data dan Nuxt.js akan menggabungkannya dengan data komponen.

Nuxt.js akan secara otomatis menggabungkan objek yang di-return dengan data komponen.

<div class="Alert Alert--orange">

Anda **TIDAK** memiliki akses contoh komponen melalui `this` di dalam `asyncData` karena ia dipanggil **sebelum memulai** komponennya.

</div>

Nuxt.js menawarkan berbagai cara untuk menggunakan `asyncData`. Pilih yang paling Anda lazimi:

1. Mengembalikan `Promise`. Nuxt.js akan menunggu promise untuk diselesaikan sebelum melakukan render komponen.
2. Menggunakan [async/await](https://javascript.info/async-await) ([pelajari lebih lanjut tentang itu](https://zeit.co/blog/async-and-await))

<div class="Alert Alert--grey">

Kami menggunakan [axios](https://github.com/mzabriskie/axios) untuk membuat berbagai HTTP request isomorfis, kami <strong>sangat merekomendasikan</strong> untuk menggunakan [axios module](https://axios.nuxtjs.org/) untuk project Nuxt Anda.

</div>

Jika Anda menggunakan `axios` langsung dari` node_modules` dan menggunakan `axios.interceptors` untuk menambahkan interseptor untuk mengubah data, pastikan membuat instance sebelum menambahkan interseptor. Jika tidak, saat Anda me-refresh halaman serverRender, interseptor akan ditambahkan multiple, yang akan menyebabkan eror data.

```js
import axios from 'axios'
const myaxios = axios.create({
  // ...
})
myaxios.interceptors.response.use(function (response) {
  return response.data
}, function (error) {
  // ...
})
```

### Mengembalikan (return) Promise

```js
export default {
  asyncData ({ params }) {
    return axios.get(`https://my-api/posts/${params.id}`)
      .then((res) => {
        return { title: res.data.title }
      })
  }
}
```

### Menggunakan async/await

```js
export default {
  async asyncData ({ params }) {
    const { data } = await axios.get(`https://my-api/posts/${params.id}`)
    return { title: data.title }
  }
}
```


### Menampilkan data

Hasil dari asyncData akan **digabung** dengan data.
Anda dapat menampilkan data di dalam template Anda seperti yang biasa Anda lakukan:

```html
<template>
  <h1>{{ title }}</h1>
</template>
```

## Konteks (context)

Untuk melihat daftar key yang tersedia di dalam `context`, lihat [API Essential `context`](/api/context).

### Gunakan objek `req`/`res`

Ketika `asyncData` dipanggil di sisi server, Anda memiliki akses ke objek` req` dan `res` dari request pengguna.

```js
export default {
  async asyncData ({ req, res }) {
    // Silakan periksa apakah Anda berada di sisi server sebelumnya
    // menggunakan req and res
    if (process.server) {
      return { host: req.headers.host }
    }

    return {}
  }
}
```

### Mengakses data route dinamis

Anda dapat menggunakan parameter `context` untuk mengakses data rute dinamis juga!
Misalnya, parameter rute dinamis dapat diambil menggunakan nama file atau folder yang mengkonfigurasinya.
Jika Anda telah mendefinisikan file dengan nama `_slug.vue` di dalam folder `pages` Anda, Anda dapat mengakses nilai melalui `context.params.slug`:

```js
export default {
  async asyncData ({ params }) {
    const slug = params.slug // Saat memanggil /abc maka slug akan "abc"
    return { slug }
  }
}
```


### Melakukan Listen terhadap perubahan query

Metode `asyncData` **tidak dipanggil** pada perubahan query string secara default.
Jika Anda ingin mengubah perilaku ini, misalnya saat membangun komponen pagination,
Anda dapat mengatur parameter yang harus didengarkan (listen) dengan properti `watchQuery` dari komponen halaman Anda.
Pelajari lebih lanjut di [halaman API `watchQuery`](/api/pages-watchquery).

## Melakukan Handle Error

Nuxt.js menambahkan metode `error(params)` di dalam `konteks`, yang dapat Anda panggil untuk menampilkan halaman eror. `params.statusCode` juga akan digunakan untuk merender kode status yang tepat dari sisi server.

Contoh dengan menggunakan `Promise`:

```js
export default {
  asyncData ({ params, error }) {
    return axios.get(`https://my-api/posts/${params.id}`)
      .then((res) => {
        return { title: res.data.title }
      })
      .catch((e) => {
        error({ statusCode: 404, message: 'Post not found' })
      })
  }
}
```


Untuk melakukan modifikasi halaman eror, lihat [panduan tampilan](/guide/views#layouts).
