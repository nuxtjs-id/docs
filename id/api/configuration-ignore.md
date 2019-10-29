---
title: "API: Properti ignore"
description: Mendefinisikan file-file yang diabaikan untuk Aplikasi Nuxt.js A
---

## .nuxtignore

Anda dapat menggunakan file `.nuxtignore` supaya Nuxt.js mengabaikan file-file `layout`, `page`, `store` dan `middleware` di dalam direktori root proyek Anda (`rootDir`) selama fase build.
File `.nuxtignore` merupakan subyek pada spesifikasi yang sama seperti file-file `.gitignore` dan `.eslintignore`, di mana setiap baris adalah pola glob yang menunjukkan file mana yang harus diabaikan.

Sebagai Contoh:

```
# ignore layout foo.vue
layouts/foo.vue
# ignore layout files whose name ends with -ignore.vue
layouts/*-ignore.vue

# ignore page bar.vue
pages/bar.vue
# ignore page inside ignore folder
pages/ignore/*.vue

# ignore store baz.js
store/baz.js
# ignore store files match *.test.*
store/ignore/*.test.*

# mengabaikan file-file middleware dibawah folder foo kecuali foo/bar.js
middleware/foo/*.js
!middleware/foo/bar.js
```

> Lebih lanjut tentang [dokumentasi gitignore](https://git-scm.com/docs/gitignore)

## Properti ignorePrefix

- Type: `String`
- Default: `'-'`

> Semua file di dalam pages/, layout/, middleware/ atau store/ akan diabaikan selama proses build apabila nama file nya dimulai dengan prefix yang di spesifikasi oleh `ignorePrefix`.

Secara default semua file yang dimulai dengan `-` akan diabaikan, seperti `store/-foo.js` dan `pages/-bar.vue`. Hal ini memungkinkan untuk melakukan test co-locating, utilitas, dan komponen dengan penghubung mereka tanpa di konversi ke dalam routes, stores, etc.

**Catatan:** Opsi ini akan ditinggalkan pada Nuxt.js 3. Kami merekomendasikan untuk menggunakan file `.nuxtignore`.

## Properti ignore

- Type: `Array`
- Default: `['**/*.test.*']`

> Lebih dapat dikustomisasi daripada `ignorePrefix`: semua file yang cocok dengan pola glob yang ditentukan di dalam `ignore` akan diabaikan ketika build.

**Catatan:** Opsi ini akan ditinggalkan pada Nuxt.js 3. Kami merekomendasikan untuk menggunakan file `.nuxtignore`.
