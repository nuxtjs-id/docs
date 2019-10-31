---
title: "API: Metode validate"
description: Nuxt.js memungkinkan Anda menentukan metode validator di dalam komponen rute dinamis Anda.
---

> Nuxt.js memungkinkan Anda menentukan metode validator di dalam komponen rute dinamis Anda.

- **Type:** `Function` or `Async Function`

```js
validate({ params, query, store }) {
  return true // jika parameter valid
  return false // akan menghentikan Nuxt.js untuk merender route dan akan menampilkan halaman eror
}
```

```js
async validate({ params, query, store }) {
  // await operations
  return true // if the parameter valid
  return false // akan menghentikan Nuxt.js untuk merender route dan akan menampilkan halaman eror
}
```

Anda juga dapat melakukan return promise:

```js
validate({ params, query, store }) {
  return new Promise((resolve) => setTimeout(() => resolve()))
}
```

Nuxt.js memungkinkan Anda menentukan metode validator di dalam komponen route dinamis (Sebagai contoh: `pages/users/_id.vue`).

Jika metode validasi tidak me-return `true`, Nuxt.js akan secara otomatis memuat halaman eror 404.

```js
export default {
  validate ({ params }) {
    // Harus number
    return /^\d+$/.test(params.id)
  }
}
```

Anda juga dapat memeriksa beberapa data di dalam [store](/guide/vuex-store) sebagai contoh (diisi oleh [`nuxtServerInit`](/guide/vuex-store#the-nuxtserverinit-action) sebelum action):

```js
export default {
  validate ({ params, store }) {
    // Periksa apakah `params.id` adalah kategori yang sudah ada
    return store.state.categories.some(category => category.id === params.id)
  }
}
```

Anda juga dapat melakukan throw eror selama menjalankan fungsi validasi:

```js
export default {
  async validate ({ params, store }) {
    // Melempar kesalahan server internal 500 dengan menambahkan pesan khusus
    throw new Error('Under Construction!')
  }
}
```
