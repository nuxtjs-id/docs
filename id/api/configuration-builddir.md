---
title: "API: Properti buildDir"
description: Mendefinisikan direktori dist untuk Aplikasi Nuxt.js Anda
---

- Type: `String`
- Default: `.nuxt`

> Mendefinisikan direktori dist untuk Aplikasi Nuxt.js Anda

Example (`nuxt.config.js`):

```js
export default {
  buildDir: 'nuxt-dist'
}
```

Secara default, banyak yang berasumsi bahwa nuxt itu `.nuxt` adalah direktori tersembunyi, karena namanya diawali dengan titik. Anda dapat menggunakan opsi ini untuk mencegahnya.
