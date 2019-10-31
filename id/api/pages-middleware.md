---
title: "API: Properti middleware"
description: Menetapkan middleware untuk halaman tertentu dari aplikasi.
---

- Type: `String` or `Array` or `Function`
  - Items: `String` or `Function`

Mengatur middleware untuk halaman aplikasi tertentu.

## middleware yang diberi nama

Anda dapat membuat middleware yang diberi nama dengan cara membuat file di dalam direktori `middleware/`, nama file akan menjadi nama middleware.

`middleware/authenticated.js`:

```js
export default function ({ store, redirect }) {
  // Jika user tidak ter-autentikasi
  if (!store.state.authenticated) {
    return redirect('/login')
  }
}
```

`pages/secret.vue`:

```html
<template>
  <h1>Halaman ter-autentikasi</h1>
</template>

<script>
export default {
  middleware: 'authenticated'
}
</script>
```

## Anonymous middleware

Jika Anda perlu menggunakan middleware hanya untuk halaman tertentu, Anda dapat langsung menggunakan fungsi untuk itu (atau array fungsi):

`pages/secret.vue`:

```html
<template>
  <h1>Halaman ter-autentikasi</h1>
</template>

<script>
export default {
  middleware ({ store, redirect }) {
    // Jika user tidak ter-autentikasi
    if (!store.state.authenticated) {
      return redirect('/login')
    }
  }
}
</script>
```

Untuk mempelajari lebih lanjut tentang middleware, lihat [panduan middleware](/guide/routing#middleware).
