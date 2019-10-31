---
title: "API: Metode asyncData"
description: Anda mungkin ingin mengambil data dan merendernya di sisi server. Nuxt.js menambahkan metode `asyncData` yang memungkinkan Anda menangani operasi async sebelum mengatur data komponen.
---

> Anda mungkin ingin mengambil data dan merendernya di sisi server. Nuxt.js menambahkan metode `asyncData` yang memungkinkan Anda menangani operasi async sebelum mengatur data komponen.

- **Tipe:** `Function`

<div class="Alert Alert--nuxt-green">

<b>Info:</b> Silahkan lihat [panduan async data](/guide/async-data) juga!

</div>

`asyncData` akan selalu dipanggil sebelum memuat komponen **halaman** dan hanya tersedia untuk itu.
Ini akan memanggil satu kali ke sisi server (pada request pertama ke aplikasi Nuxt) dan sisi klien saat menavigasi ke route selanjutnya. 
Metode ini menerima objek [`context`](/api/context) sebagai argumen pertama, Anda dapat menggunakannya untuk mengambil beberapa data dan mengembalikan data komponen.


Hasil dari asyncData akan **digabung** dengan data.

```js
export default {
  data () {
    return { project: 'default' }
  },
  asyncData (context) {
    return { project: 'nuxt' }
  }
}
```

<div class="Alert Alert--orange">

<b>Perhatian:</b> Anda **tidak** memiliki akses ke instance komponen melalui `this` di dalam `asyncData`, karena ia dipanggil **sebelum memulai** komponen.

</div>
