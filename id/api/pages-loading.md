---
title: "API: Properti loading"
description: Properti `loading` memberi Anda opsi untuk menonaktifkan loading progresbar default pada halaman tertentu.
---

> Properti `loading` memberi Anda opsi untuk menonaktifkan loading progresbar default pada halaman tertentu.

- **Type:** `Boolean` (default: `true`)

Secara default, Nuxt.js menggunakan komponennya sendiri untuk menampilkan progress bar antara rute.

Anda dapat menonaktifkan atau melakukan penyesuaian secara global melalui [Opsi Konfigurasi loading](/api/configuration-loading), tetapi juga menonaktifkannya untuk halaman tertentu dengan mengatur properti `memuat` menjadi `false`:

```html
<template>
  <h1>Halaman saya</h1>
</template>

<script>
export default {
  loading: false
}
</script>
```

Anda dapat menemukan contoh menggunakan properti ini [di sini](/examples/custom-page-loading).
