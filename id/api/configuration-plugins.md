---
title: 'API: Properti plugins'
description: 'Menggunakan plugin vue.js dengan opsi plugins pada Nuxt.js.'
---

**Catatan**: Sejak Nuxt.js 2.4, `mode` telah diperkenalkan sebagai opsi `plugins` untuk menentukan jenis plugin, nilai yang mungkin adalah: `client` atau `server`. `ssr: false` akan disesuaikan dengan `mode: 'client'` dan tidak akan lagi digunakan dalam rilis besar berikutnya.

- Type: `Array`
  - Items: `String` or `Object`

Jika item berupa objek, maka propertinya adalah:

  - src: `String` (file path)
  - mode: `String` (bisa `client` atau `server`) *Jika terdefinisi, file hanya akan disertakan pada masing-masing sisi (client atau server).*

**Note**: Versi lawas

- Type: `Array`
  - Items: `String` or `Object`

Jika item berupa objek, maka propertinya adalah:

  - src: `String` (file path)
  - ssr: `Boolean` (default nya adalah `true`) *Jila false, maka file hanya akan disertakan di sisi klien.*

> Properti plugins memungkinkan Anda menambahkan plugin vue.js dengan mudah ke aplikasi utama Anda.

Contoh (`nuxt.config.js`):

```js
export default {
  plugins: [
    { src: '~/plugins/both-sides.js' },
    { src: '~/plugins/client-only.js', mode: 'client' },
    { src: '~/plugins/server-only.js', mode: 'server' }
  ]
}
```

Contoh kerangka (framework) UI (`nuxt.config.js`):

```js
export default {
  plugins: ['@/plugins/ant-design-vue']
}
```

Isi dari file `plugins/ant-design-vue.js`:

```js
import Vue from 'vue'
import Antd from 'ant-design-vue'
import 'ant-design-vue/dist/antd.css' // Per Ant Design's docs

Vue.use(Antd)
```

Perhatikan bahwa css telah [diimpor sesuai Dokumentasi Desain Ant](https://vue.ant.design/docs/vue/getting-started/#3.-Use-antd's-Components "Tips eksternal yang relevan untuk melakukan build plugin")


Semua path didefinisikan dalam properti `plugins` akan **diimpor** sebelum menginisialisasi aplikasi utama.

## Kapan saya menggunakan plugin?

Setiap kali Anda perlu menggunakan `Vue.use()`, Anda harus membuat file di dalam `plugins/` dan menambahkan pathnya ke `plugins` di dalam `nuxt.config.js`.

Untuk mempelajari lebih lanjut cara menggunakan plugin, lihat [dokumentasi panduan](/guide/plugins#vue-plugins).
