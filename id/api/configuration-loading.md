---
title: "API: Properti loading"
description: Nuxt.js menggunakan komponennya sendiri untuk menunjukkan progress bar
  di antara rute. Anda dapat menyesuaikannya (kostumisasi), menonaktifkannya atau membuat komponen Anda sendiri.
---

- Type: `Boolean` or `Object` or `String`

> Nuxt.js memberi Anda komponen loading progress bar yang ditampilkan di antara rute. Anda dapat menyesuaikannya, menonaktifkannya atau membuat komponen Anda sendiri.

Loading bar juga dapat dimulai secara program pada komponen Anda dengan memanggil `this.$Nuxt.$Loading.start()` untuk memulai loading bar dan `this.$Nuxt.$Loading.finish()` untuk membuatnya selesai.

Selama proses pemasangan komponen halaman Anda, properti `$loading` tidak selalu tersedia dengan segera untuk dapat diakses. Untuk mengatasi ini, jika Anda ingin memulai pemuat dalam metode `mounted`, pastikan untuk melakukan pembungkusan (wrap) metode`$loading` Anda dipanggil di dalam ` this.$nextTick` seperti berikut:

```javascript
export default {
  mounted () {
    this.$nextTick(() => {
      this.$nuxt.$loading.start()

      setTimeout(() => this.$nuxt.$loading.finish(), 500)
    })
  }
}
```

## Non-aktifkan the Progress Bar

- Type: `Boolean`

Jika Anda tidak ingin menampilkan progress bar, cukup tambahkan `loading: false` di dalam file `nuxt.config.js`:

```js
export default {
  loading: false
}
```

## Memodifikasi Progress Bar

- Type: `Object`

Di antara properti lainnya, warna, ukuran, durasi dan arah dari progress bar dapat di modifikasi sesuai dengan kebutuhan aplikasi Anda. Ini dilakukan dengan memperbarui properti `loading` pada `nuxt.config.js` dengan properti yang sesuai.

For example, to set a blue progress bar with a height of 5px, we update the `nuxt.config.js` to the following:

```js
export default {
  loading: {
    color: 'blue',
    height: '5px'
  }
}
```

Daftar properti untuk memodifikasi progress bar.

| Key | Tipe | Default | Penjelasan |
|-----|------|---------|-------------|
| `color` | String | `'black'` | Warna CSS the progress bar |
| `failedColor` | String | `'red'` | Warna CSS progress bar ketika terjadi eror selama me-render route (sebagai contoh, jika `data` atau `fetch` memunculkan error). |
| `height` | String | `'2px'` | Tinggi dari progress bar (digunakan di dalam properti `style` pada progress bar) |
| `throttle` | Number | `200` | Dalam milliseconds, waktu tunggu yang ditentukan sebelum menampilkan progress bar. Berguna untuk mencegah progressbar berkedip. |
| `duration` | Number | `5000` | Dalam milliseconds, durasi maksimal duration progress bar, Nuxt.js mengasumsikan bahwa rute akan di-render sebelum 5 detik. |
| `continuous` | Boolean | `false` | Membuat progress bar berkelanjutan secara terus menerus ketika waktu loading melebihi `duration`. |
| `css` | Boolean | `true` | Set menjadi false untuk menghapus style default progress bar (dan tambahkan style css milik Anda). |
| `rtl` | Boolean | `false` | Mengatur arah progress bar dari kanan ke kiri. |


## Progress Bar Internal

Sayangnya, tidak mungkin bagi komponen Loading untuk mengetahui berapa lama waktu memuat halaman baru. Oleh sebab itu, tidak mungkin untuk secara akurat menggerakkan bilah kemajuan hingga 100% dari waktu loading.

Komponen loading Nuxt akan menyelesaikan ini dengan memungkinkan Anda mengatur `duration`, hendaknya di set ke _guestimate_ tentang berapa lama proses loading process akan diambil. Unless you use a custom loading component, the progress bar will always move from 0% to 100% in `duration` time (regardless of actual progression). When the loading takes longer than `duration` time, the progress bar will stay at 100% until the loading finishes.

Anda dapat mengubah perilaku default dengan mengatur `continuous` menjadi true, kemudian setelah mencapai 100% progress bar akan menyusut kembali menjadi 0% dalam waktu `duration`. Ketika loading masih belum selesai setelah mencapai 0% akan memulai progress dari 0% ke 100% lagi, yang mana akan terus diulang sampai loading selesai.

*Contoh progressbar berkelanjutan:*


<img src="/api-continuous-loading.gif" alt="continuous loading"/>


## Menggunakan Komponen Loading secara Kustom

- Type: `String`

Anda juga bisa membuat komponen Anda sendiri, Nuxt.js akan memanggilnya dan mengabaikan komponen progressbar default loading. Untuk melakukannya, Anda perlu memberikan path ke komponen Anda di opsi `loading`. Lalu, komponen milik Anda akan dipanggil secara langsung oleh Nuxt.js.

**Komponen Anda harus memaparkan beberapa metode ini:**

| Metode | Required | Penjelasan |
|--------|----------|-------------|
| `start()` | Required | Dipanggil ketika route berubah, disini dimana Anda menampilkan komponen loading. |
| `finish()` | Required | Dipanggil ketika route ter-load (dan data berhasil diambil), disini tempat Anda menyembunyikan komponen loading. |
| `fail()` | *Optional* | Dipanggil ketika route tidak bisa di load (gagal mengambil data). |
| `increase(num)` | *Optional* | Dipanggil selama loading komponen route, `num` adalah Integer < 100. |

Kita akan membuat komponen kustom pada file `components/loading.vue`:
```html
<template lang="html">
  <div class="loading-page" v-if="loading">
    <p>Loading...</p>
  </div>
</template>

<script>
export default {
  data: () => ({
    loading: false
  }),
  methods: {
    start () {
      this.loading = true
    },
    finish () {
      this.loading = false
    }
  }
}
</script>

<style scoped>
.loading-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.8);
  text-align: center;
  padding-top: 200px;
  font-size: 30px;
  font-family: sans-serif;
}
</style>
```

Lalu, kita update `nuxt.config.js` untuk memberitahukan Nuxt.js untuk menggunakan komponen kita:

```js
export default {
  loading: '~/components/loading.vue'
}
```
