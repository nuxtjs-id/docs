---
title: "API: Konteks"
description: `context` menyediakan objek / params tambahan dari Nuxt yang secara default tidak tersedia pada komponen Vue. `Konteks` secara spesial tersedia di area siklus hidup nuxt seperti` asyncData`, `plugins`,` middlewares`, `modules`, dan` store/nuxtServerInit`.
---

> `context` menyediakan objek / params tambahan dari komponen Nuxt ke Vue dan tersedia di area siklus hidup nuxt secara khusus seperti[`asyncData`](/api), [`fetch`](/api/pages-fetch), [`plugins`](/guide/plugins), [`middleware`](/guide/routing#middleware) dan [`nuxtServerInit`](/guide/vuex-store#the-nuxtserverinit-action).

```js
function (context) {
  // Universal keys
  const {
    app,
    store,
    route,
    params,
    query,
    env,
    isDev,
    isHMR,
    redirect,
    error
  } = context
  // Sisi server
  if (process.server) {
    const { req, res, beforeNuxtRender } = context
  }
  // Sisi klien
  if (process.client) {
    const { from, nuxtState } = context
  }
}
```

## Universal keys

Keys ini tersedia di sisi klien dan di sisi server.

### `app` (*NuxtAppOptions*)

Opsi instance root Vue yang mencakup semua plugin Anda. Misalnya, ketika menggunakan `i18n`, Anda bisa mendapatkan akses ke`$i18n` melalui `context.app.i18n`.

### `store` ([*Vuex Store*](https://vuex.vuejs.org/en/api.html#vuexstore-instance-properties))

Instance Vuex Store. **Hanya tersedia jika [vuex store](/guide/vuex-store) diatur**.

### `route` ([*Vue Router Route*](https://router.vuejs.org/en/api/route-object.html))

Instance Rute Vue Router.

### `params` (*Object*)

Alias ​​dari `route.params`.

### `query` (*Object*)

Alias ​​dari `route.query`.

### `env` (*Object*)

Variabel lingkungan (environment) ditetapkan di dalam `nuxt.config.js`, lihat [env api](/api/configuration-env).

### `isDev` (*Boolean*)

Boolean untuk memberi tahu Anda jika Anda dalam mode dev, dapat berguna untuk menyimpan (cache) beberapa data pada mode produksi.

### `isHMR` (*Boolean*)

Boolean memberi tahu Anda jika metode / middleware dipanggil dari penggantian modul hot webpack (*true hanya pada sisi klien dalam mode dev*).

### `redirect` (*Function*)

Gunakan metode ini untuk mengarahkan ulang pengguna ke rute lain, kode status digunakan di sisi server, default ke `302`. `redirect([status,] path [, query])`.

### `error` (*Function*)

Gunakan metode ini untuk menampilkan halaman kesalahan: `error(params)`. `params` seharusnya mempunyai properti `statusCode` dan `message`.

## Server-side keys

Keys ini hanya tersedia di sisi server.

### `req` ([*http.Request*](https://nodejs.org/api/http.html#http_class_http_incomingmessage))

Request dari server Node.js. Jika Nuxt digunakan sebagai middleware, objek request mungkin berbeda tergantung pada kerangka yang Anda gunakan.<br>**Tidak tersedia di `nuxt generate`**.  

### `res` ([*http.Response*](https://nodejs.org/api/http.html#http_class_http_serverresponse))

Response dari server Node.js. Jika Nuxt digunakan sebagai middleware, objek res mungkin berbeda tergantung pada kerangka yang Anda gunakan.<br>**Tidak tersedia di `nuxt generate`**.

### `beforeNuxtRender(fn)` (*Function*)

Gunakan metode ini untuk memperbarui variabel `__NUXT__` yang diberikan di sisi klien, `fn` (bisa asynchronous) dipanggil dengan `{ Components, nuxtState }`, lihat [contoh](https://github.com/nuxt/nuxt.js/blob/cf6b0df45f678c5ac35535d49710c606ab34787d/test/fixtures/basic/pages/special-state.vue).

## Client-side keys

Keys ini hanya tersedia di sisi klien.

### `from` ([*Vue Router Route*](https://router.vuejs.org/en/api/route-object.html))

Darimana route di navigasi.

### `nuxtState` *(Object)*

State Nuxt, berguna untuk plugin yang menggunakan `beforeNuxtRender` untuk mendapatkan status nuxt di sisi klien sebelum hidrasi (hydration). **Hanya tersedia pada mode `universal`**.
