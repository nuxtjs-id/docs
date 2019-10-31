---
title: "API: Properti scrollToTop"
description: Properti `scrollToTop`  memungkinkan Anda memberitahu Nuxt.js untuk menggulir (scroll) ke atas sebelum me-render halaman.
---

> Properti scrollToTop memungkinkan Anda memberi tahu Nuxt.js untuk melakukan scroll ke atas sebelum merender halaman.

- **Type:** `Boolean` (default: `false`)

Secara default, Nuxt.js melakukan scroll ke atas ketika Anda mengunjungi halaman lain. Tetapi pada route child, Nuxt.js akan menjaga posisi scroll. Jika Anda ingin memberi tahu Nuxt.js untuk melakukan scroll ke atas saat merender route child Anda, atur `scrollToTop` menjadi `true`:

```html
<template>
  <h1>My child component</h1>
</template>

<script>
export default {
  scrollToTop: true
}
</script>
```

Dan sebaliknya, Anda dapat secara manual mengatur `scrollToTop` menjadi `false` pada rute induk juga.

Jika Anda ingin menimpa perilaku (memodifikasi) scroll default Nuxt.js, lihat [opsi scrollBehavior](/api/configuration-router#scrollbehavior).
