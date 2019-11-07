---
title: Plugin
description: Nuxt.js memungkinkan Anda untuk menentukan plugin JavaScript yang akan dijalankan sebelum instantiating Aplikasi root Vue.js. Ini sangat membantu saat menggunakan librari Anda sendiri atau modul eksternal.
---

> Nuxt.js memungkinkan Anda untuk menentukan plugin JavaScript yang akan dijalankan sebelum instantiating Aplikasi root Vue.js. Ini sangat membantu saat menggunakan librari Anda sendiri atau modul eksternal.

<div class="Alert">

Penting untuk mengetahui hal itu di dalam Vue [instance lifecycle](https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram), hanya hooks `beforeCreate` dan `created` yang keduanya dipanggil, **dari sisi klien dan sisi server**. Sementara semua hooks lainnya hanya dipanggil dari sisi klien.

</div>

## Package Eksternal

Mungkin kita ingin menggunakan paket/modul eksternal dalam aplikasi (satu contoh yang bagus adalah [axios](https://github.com/mzabriskie/axios)) untuk membuat request HTTP untuk server dan klien.

Pertama-tama, kita harus menginstalnya melalui npm:

```bash
npm install --save axios
```

Kemudian kita dapat menggunakannya langsung di komponen halaman:

```html
<template>
  <h1>{{ title }}</h1>
</template>

<script>
import axios from 'axios'

export default {
  async asyncData ({ params }) {
    let { data } = await axios.get(`https://my-api/posts/${params.id}`)
    return { title: data.title }
  }
}
</script>
```

## Plugin Vue

Jika kita ingin menggunakan plugin Vue, seperti [vue-notifications](https://github.com/se-panfilov/vue-notifications) untuk menampilkan pemberitahuan pada aplikasi kita, kita perlu mengatur plugin sebelum meluncurkan aplikasi.

Kita buat file `plugins/vue-notifications.js`:

```js
import Vue from 'vue'
import VueNotifications from 'vue-notifications'

Vue.use(VueNotifications)
```

Kemudian kita tambahkan path file di dalam key `plugins` di `nuxt.config.js`:

```js
export default {
  plugins: ['~/plugins/vue-notifications']
}
```

Untuk mempelajari lebih lanjut tentang key konfigurasi `plugins`, silahkan cek [api plugins](/api/configuration-plugins).

### Plugin ES6

Jika plugin terletak di `node_modules` dan mengekspor modul ES6, Anda mungkin perlu menambahkannya ke dalam opsi build `transpile`:

```js
module.exports = {
  build: {
    transpile: ['vue-notifications']
  }
}
```
Anda dapat merujuk ke dokumentasi [configuration build](/api/configuration-build/#transpile) untuk opsi build lainnya.

## Injeksi pada $root & context

Terkadang Anda ingin membuat fungsi atau value yang tersedia di seluruh aplikasi.
Anda bisa melakukan injeksi variabel-variabel tersebut ke dalam instance Vue (sisi klien), konteks (sisi server) dan bahkan di Vuex Store.
Ini adalah konvensi untuk prefix fungsi tersebut dengan `$`.

### Injeksi pada instance Vue

Melakukan injeksi konteks ke instance Vue berfungsi sama dengan ketika melakukan ini di aplikasi Vue standar.

`plugins/vue-inject.js`:

```js
import Vue from 'vue'

Vue.prototype.$myInjectedFunction = string => console.log('Ini hanya contoh', string)
```

`nuxt.config.js`:

```js
export default {
  plugins: ['~/plugins/vue-inject.js']
}
```

Anda sekarang dapat menggunakan fungsi pada semua komponen Vue Anda.

`example-component.vue`:

```js
export default {
  mounted () {
    this.$myInjectedFunction('test')
  }
}
```


### Injeksi pada context

Melakukan injeksi konteks ke instance Vue berfungsi sama dengan ketika melakukan ini di aplikasi Vue standar.

`plugins/ctx-inject.js`:

```js
export default ({ app }, inject) => {
  // Mengatur fungsi langsung pada objek context.app
  app.myInjectedFunction = string => console.log('Oke, ini fungsi lainnya', string)
}
```

`nuxt.config.js`:

```js
export default {
  plugins: ['~/plugins/ctx-inject.js']
}
```

Fungsi sekarang tersedia setiap kali Anda memiliki akses ke `context` (sebagai contoh di dalam `asyncData` dan `fetch`).

`ctx-example-component.vue`:

```js
export default {
  asyncData (context) {
    context.app.myInjectedFunction('ctx!')
  }
}
```

### Kombinasi inject

Jika Anda memerlukan fungsi pada `context`, Instance Vue dan bahkan mungkin di dalam Vuex store, anda dapat menggunakan fungsi `inject`, yang merupakan parameter kedua dari fungsi plugin yang diekspor.

Melakukan injek konten ke instance Vue berfungsi sama dengan ketika melakukan ini di aplikasi Vue standar. `$` akan ditambahkan secara otomatis ke fungsi.

`plugins/combined-inject.js`:

```js
export default ({ app }, inject) => {
  inject('myInjectedFunction', string => console.log('That was easy!', string))
}
```

`nuxt.config.js`:

```js
export default {
  plugins: ['~/plugins/combined-inject.js']
}
```

Sekarang fungsi tersebut dapat digunakan dari `context`, melalui `this` pada Instance Vue dan melalui `this` di dalam store `actions`/`mutations`.

`ctx-example-component.vue`:

```js
export default {
  mounted () {
    this.$myInjectedFunction('works in mounted')
  },
  asyncData (context) {
    context.app.$myInjectedFunction('works with context')
  }
}
```

`store/index.js`:

```js
export const state = () => ({
  someValue: ''
})

export const mutations = {
  changeSomeValue (state, newValue) {
    this.$myInjectedFunction('accessible in mutations')
    state.someValue = newValue
  }
}

export const actions = {
  setSomeValueToWhatever ({ commit }) {
    this.$myInjectedFunction('accessible in actions')
    const newValue = 'whatever'
    commit('changeSomeValue', newValue)
  }
}

```


## Hanya sisi klien

Beberapa plugin mungkin berfungsi **hanya pada browser** karena mereka kekurangan dukungan SSR.
Dalam situasi ini Anda dapat menggunakan opsi `mode`: `client` di dalam `plugins` untuk menambahkan plugin hanya pada sisi klien.

Contoh:

`nuxt.config.js`:

```js
export default {
  plugins: [
    { src: '~/plugins/vue-notifications', mode: 'client' }
  ]
}
```

`plugins/vue-notifications.js`:

```js
import Vue from 'vue'
import VueNotifications from 'vue-notifications'

Vue.use(VueNotifications)
```

Jika Anda perlu mengimpor beberapa perpustakaan hanya dalam plugin *sisi server*, Anda dapat memeriksa apakah variabel `process.server` diatur ke `true`.

Juga, jika Anda perlu tahu apakah Anda berada di dalam aplikasi yang di generate (melalui `nuxt generate`), Anda dapat memeriksa apakah `process.static` di set ke `true`. Ini hanya terjadi selama dan setelah generasi.

Anda juga dapat menggabungkan kedua opsi untuk mencapai sasaran saat sebuah halaman di-render server oleh `nuxt generate` sebelum tersimpan (`process.static && process.server`).

**Note**: Sejak Nuxt.js 2.4, `mode` telah diperkenalkan sebagai opsi `plugins` untuk menentukan jenis plugin, nilai yang mungkin adalah: `client` atau `server`. `ssr: false` akan disesuaikan menjadi `mode: 'client'` dan akan deprecated pada mayor rilis berikutnya.

Contoh:

`nuxt.config.js`:

```js
export default {
  plugins: [
    { src: '~/plugins/both-sides.js' },
    { src: '~/plugins/client-only.js', mode: 'client' },
    { src: '~/plugins/server-only.js', mode: 'server' }
  ]
}
```

### Nama plugin konvensional

Jika plugin diasumsikan dijalankan hanya di sisi klien atau server, `.client.js` atau `.server.js` dapat diterapkan sebagai ekstensi file plugin, file akan secara otomatis dimasukkan pada sisi yang sesuai.

Contoh:

`nuxt.config.js`:

```js
export default {
  plugins: [
    '~/plugins/foo.client.js', // hanya sisi klien
    '~/plugins/bar.server.js', // hanya sisi server
    '~/plugins/baz.js' // kedua klien & server
  ]
}
```
