---
title: 'API: Metode fetch'
description: Metode `fetch` digunakan untuk mengisi store sebelum me-render halaman, ia seperti metode `asyncData` tetapi ia tidak mengatur data komponen.
---

> Metode fetch digunakan untuk mengisi store sebelum me-render halaman, ia seperti metode `asyncData` tetapi ia tidak mengatur data komponen.

- **Type:** `Function`

Metode `fetch`, *jika diatur*, akan dipanggil setiap kali sebelum memuat komponen (**hanya untuk komponen halaman**). Ini akan dipanggil pada sisi server sekali (pada saat request pertama ke aplikasi Nuxt) dan pada sisi klien ketika menavigasi ke route selanjutnya.

Metode `fetch` menerima objek [`context`](/api/context) sebagai argumen pertama, kita dapat menggunakannya untuk mengambil beberapa data dan mengisi store. Untuk membuat metode `fetch` asinkron, **mengembalikan Promise**, Nuxt.js akan menunggu Promise untuk diselesaikan sebelum merender komponen.


<div class="Alert Alert--orange">

**Peringatan**: Anda **tidak** memiliki akses instance komponen melalui `this` di dalam `fetch` karena disebut **sebelum memulai** komponen.

</div>


Contoh pada `pages/index.vue`:

```html
<template>
  <h1>Stars: {{ $store.state.stars }}</h1>
</template>

<script>
export default {
  fetch ({ store, params }) {
    return axios.get('http://my-api/stars')
    .then((res) => {
      store.commit('setStars', res.data)
    })
  }
}
</script>
```

Anda juga bisa menggunakan `async`/`await` untuk membuat kode Anda lebih jelas:

```html
<template>
  <h1>Stars: {{ $store.state.stars }}</h1>
</template>

<script>
export default {
  async fetch ({ store, params }) {
    let { data } = await axios.get('http://my-api/stars')
    store.commit('setStars', data)
  }
}
</script>
```

## Vuex

Jika Anda ingin memanggil aksi dari store (store action), gunakan `store.dispatch` di dalam `fetch`, pastikan untuk menunggu aksi berakhir dengan menggunakan `async`/`await` di dalam:

```html
<script>
export default {
  async fetch ({ store, params }) {
    await store.dispatch('GET_STARS');
  }
}
</script>
```

`store/index.js`

```js
// ...
export const actions = {
  async GET_STARS ({ commit }) {
    const { data } = await axios.get('http://my-api/stars')
    commit('SET_STARS', data)
  }
}
```

## Mendengarkan (Listen) perubahan queri string

Metode `fetch` **tidak dipanggil** pada saat perubahan query string secara default. Jika Anda ingin mengubah perilaku ini, misalnya saat membangun komponen pagination, Anda dapat mengatur parameter yang harus didengarkan melalui properti `watchQuery` dari komponen halaman Anda. Pelajari lebih lanjut pada [halaman API `watchQuery`](/api/pages-watchquery).
