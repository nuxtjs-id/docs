---
title: "API: Metode head"
description: Nuxt.js menggunakan vue-meta untuk memperbarui `headers` dan `HTML attributes` pada aplikasi Anda.
---

> Nuxt.js menggunakan [vue-meta](https://github.com/nuxt/vue-meta) untuk memperbarui `header` dan` atribut html` aplikasi Anda.

- **Type:** `Object` or `Function`

Gunakan metode `head` untuk mengatur tag HTML Head untuk halaman saat ini.

Data komponen Anda tersedia dengan `this` di dalam metode `head`, Anda dapat menggunakan tag meta khusus yang disetel dengan data halaman. Pastikan juga untuk melihat [FAQ Nuxt](https://nuxtjs.org/faq/).

```html
<template>
  <h1>{{ title }}</h1>
</template>

<script>
export default {
  data () {
    return {
      title: 'Hello World!'
    }
  },
  head () {
    return {
      title: this.title,
      meta: [
        // hid digunakan sebagai pengidentifikasi unik. Jangan gunakan `vmid` karena tidak akan berfungsi
        { hid: 'description', name: 'description', content: 'Contoh deskripsi saya' }
      ]
    }
  }
}
</script>
```

<div class="Alert Alert--teal">

<b>Info:</b> Untuk menghindari duplikasi meta tag ketika digunakan dalam komponen child, atur pengidentifikasi unik dengan kunci `hid` untuk elemen meta Anda ([baca lebih lanjut](https://vue-meta.nuxtjs.org/api/#tagidkeyname)) .

</div>
