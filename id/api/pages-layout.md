---
title: "API: Properti layout"
description: Setiap file (tingkat pertama) dalam direktori `layouts` akan membuat layouts khusus yang dapat diakses dengan properti tata letak di dalam komponen halaman.
---

> Setiap file (tingkat pertama) dalam direktori `layouts` akan membuat layouts khusus yang dapat diakses dengan properti tata letak di dalam komponen halaman.

- **Type:** `String` or `Function` (default: `'default'`)

Gunakan tombol `layout` di komponen halaman Anda untuk menentukan tata letak mana yang akan digunakan:

```js
export default {
  layout: 'blog',
  // atau
  layout (context) {
    return 'blog'
  }
}
```

Dalam contoh ini, Nuxt.js akan menyertakan file `layouts/blog.vue` sebagai tata letak untuk komponen halaman ini.

Cek [video demonstrasi](https://www.youtube.com/watch?v=YOKnSTp7d38) untuk melihat bagaimana itu bekerja.

Untuk memahami bagaimana tata letak bekerja dengan Nuxt.js, lihat [dokumentasi layout](/guide/views#layouts).
