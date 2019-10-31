---
title: "API: Properti watchQuery"
description: Melakukan watch query string  dan jalankan metode komponen terhadap perubahan (asyncData, fetch, validate, layout, ...)
---

> Melakukan watch query string  dan jalankan metode komponen terhadap perubahan (asyncData, fetch, validate, layout, ...)
- **Type:** `Boolean` or `Array` or `Function` (default: `[]`)

Gunakan key `watchQuery` untuk mengatur watcher pada query string. Jika string yang didefinisikan berubah, semua metode komponen (asyncData, fetch, validate, layout, ...) akan dipanggil. Watch dinonaktifkan secara default untuk meningkatkan performa.

Jika Anda ingin menyiapkan watcher untuk semua query string, atur `watchQuery: true`.

```js
export default {
  watchQuery: ['page']
}
```

Anda juga dapat menggunakan fungsi ini `watchQuery(newQuery, oldQuery)` untuk lebih memperhalus watchers.

```js
export default {
  watchQuery (newQuery, oldQuery) {
    // Hanya menjalankan metode komponen jika query string lama berisikan `bar`
    // dan query string yang baru berisikan `foo`
    return newQuery.foo && oldQuery.bar
  }
}
```
