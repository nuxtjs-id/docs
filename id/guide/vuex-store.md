---
title: Vuex Store
description: Menggunakan store untuk mengelola state adalah penting untuk setiap aplikasi besar. Itu sebabnya Nuxt.js mengimplementasikan Vuex di dalam core-nya.
---

> Menggunakan store untuk mengelola state adalah penting untuk setiap aplikasi besar. Itu sebabnya Nuxt.js mengimplementasikan [Vuex](https://vuex.vuejs.org/en/) di dalam core-nya.

<div class="Promo__Video">
  <a href="https://vueschool.io/lessons/utilising-the-vuex-store-nuxtjs?friend=nuxt" target="_blank">
    <p class="Promo__Video__Icon">
      Lihat pembelajaran gratis tentang <strong>Nuxt.js dan Vuex</strong> di Vue School 
    </p>
  </a>
</div>

## Aktifkan Store

Nuxt.js akan mencari direktori `store`, jika ditemukan, maka akan:

1. Melakukan import Vuex,
2. Dan opsi `store` ke dalam Instance root Vue.

Nuxt.js memungkinkan Anda memutuskan di antara **2 mode store**. Anda dapat memilih yang Anda inginkan:

- **Modules:** setiap file `.js` di dalam direktori `store` ditransformasikan sebagai [modul namespaced](http://vuex.vuejs.org/en/modules.html) (`index` menjadi modul root).
- **Classic (__deprecated__):** `store/index.js` mengembalikan method untuk membuat instance store.

Terlepas dari mode, nilai `state` anda hendaknya **selalu berupa `function`** untuk menghindari *pembagian (shared)* state yang tidak diinginkan pada sisi.

## Mode Modules

> Nuxt.js memungkinkan Anda memiliki direktori `store` dengan setiap file yang terkait dengan modul.

Untuk memulai, cukup ekspor state sebagai fungsi, mutations dan actions sebagai objek di dalam `store/index.js`:

```js
export const state = () => ({
  counter: 0
})

export const mutations = {
  increment (state) {
    state.counter++
  }
}
```

Kemudian, anda mempunyai file `store/todos.js`:

```js
export const state = () => ({
  list: []
})

export const mutations = {
  add (state, text) {
    state.list.push({
      text,
      done: false
    })
  },
  remove (state, { todo }) {
    state.list.splice(state.list.indexOf(todo), 1)
  },
  toggle (state, todo) {
    todo.done = !todo.done
  }
}
```

Maka, store akan otomatis dibuat seperti:

```js
new Vuex.Store({
  state: () => ({
    counter: 0
  }),
  mutations: {
    increment (state) {
      state.counter++
    }
  },
  modules: {
    todos: {
      namespaced: true,
      state: () => ({
        list: []
      }),
      mutations: {
        add (state, { text }) {
          state.list.push({
            text,
            done: false
          })
        },
        remove (state, { todo }) {
          state.list.splice(state.list.indexOf(todo), 1)
        },
        toggle (state, { todo }) {
          todo.done = !todo.done
        }
      }
    }
  }
})
```

Dan di dalam `pages/todos.vue` Anda, gunakan modul `todos`:

```html
<template>
  <ul>
    <li v-for="todo in todos">
      <input type="checkbox" :checked="todo.done" @change="toggle(todo)">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
    </li>
    <li><input placeholder="What needs to be done?" @keyup.enter="addTodo"></li>
  </ul>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  computed: {
    todos () {
      return this.$store.state.todos.list
    }
  },
  methods: {
    addTodo (e) {
      this.$store.commit('todos/add', e.target.value)
      e.target.value = ''
    },
    ...mapMutations({
      toggle: 'todos/toggle'
    })
  }
}
</script>

<style>
.done {
  text-decoration: line-through;
}
</style>
```

> Metode modul juga berfungsi untuk pendefinisian tingkat atas tanpa menerapkan sub-direktori pada direktori `store`.

Contoh untuk state: Anda buat file `store/state.js` dan tambahkan baris berikut:

```js
export default () => ({
  counter: 0
})
```

Dan mutasi yang sesuai (corresponding mutations) bisa ada di file `store/mutations.js`

```js
export default {
  increment (state) {
    state.counter++
  }
}
```

### File Module

Secara opsional, Anda juga dapat memecah file modul menjadi file terpisah: `state.js`, `actions.js`, `mutations.js` dan `getters.js`. Jika Anda maintain file `index.js` dengan state, getters dan mutations selama memiliki satu file terpisah untuk actions, itu masih akan dikenali dengan benar.

> Catatan: Saat menggunakan modul file-terpisah, Anda harus ingat bahwa menggunakan arrow functions, ```this``` hanya tersedia secara leksikal. Ruang lingkup leksikal berarti bahwa ```this``` selalu merujuk kepada pemilik arrow function. Jika tidak ada arrow function maka ```this``` akan menjadi undefined. Solusinya adalah menggunakan fungsi "normal" yang menghasilkan cakupannya sendiri dan dengan demikian ```this``` akan tersedia.

### Plugin

Anda dapat menambahkan plugin tambahan ke store (dalam mode modul) dengan menempatkan mereka ke dalam file `store/index.js`:

```js
import myPlugin from 'myPlugin'

export const plugins = [ myPlugin ]

export const state = () => ({
  counter: 0
})

export const mutations = {
  increment (state) {
    state.counter++
  }
}
```

Informasi lebih lanjut tentang plugin: [Dokumentasi Vuex](https://vuex.vuejs.org/en/plugins.html).

## Metode fetch

> Metode `fetch` digunakan untuk mengisi store sebelum merender halaman, itu seperti metode `asyncData` kecuali bahwa metode fetch tidak mengatur data komponen.

Informasi lebih lanjut tentang metode fetch: [Halaman API fetch](/api/pages-fetch).

## Action nuxtServerInit

Jika action `nuxtServerInit` terdefinisi di dalam store, Nuxt.js akan memanggilnya beserta konteks (hanya dari sisi server). Ini berguna ketika kita memiliki beberapa data di server yang ingin kita berikan langsung ke sisi klien.

Sebagai contoh, katakanlah kita memiliki session di sisi server dan kita dapat mengakses pengguna yang terhubung melalui `req.session.user`. Untuk dapat memberikan autentikasi pengguna ke store kita, kita akan melakukan update file `store/index.js` menjadi seperti berikut:

```js
actions: {
  nuxtServerInit ({ commit }, { req }) {
    if (req.session.user) {
      commit('user', req.session.user)
    }
  }
}
```

> Jika Anda menggunakan mode _Modules_ pada Vuex store, hanya modul utama (di dalam `store/index.js`) akan menerima tindakan ini. Anda perlu mengaitkan actions modul dari sana.

[context](/api/context) diberikan kepada `nuxtServerInit` sebagai argumen kedua, sama seperti metode `asyncData` atau `fetch`.

> Catatan: Actions Asynchronous `nuxtServerInit` harus me-return Promise atau mengungkit async/await untuk mengijinkan server `nuxt` menunggunya.

```js
actions: {
  async nuxtServerInit({ dispatch }) {
    await dispatch('core/load')
  }
}
```

## Mode Vuex Strict

Mode Strict diaktifkan secara default pada mode dev dan dimatikan dalam mode produksi. Untuk menonaktifkan mode strict di dev, silahkan ikuti contoh berikut pada file `store/index.js`:

`export const strict = false`

## Mode Classic

> Fitur ini sudah deprecated dan akan dihapus di Nuxt 3.

Untuk mengaktifkan store dengan mode klasik, kita buat file `store/index.js` yang seharusnya mengekspor metode yang me-return instance Vuex:

```js
import Vuex from 'vuex'

const createStore = () => {
  return new Vuex.Store({
    state: () => ({
      counter: 0
    }),
    mutations: {
      increment (state) {
        state.counter++
      }
    }
  })
}

export default createStore
```

> Kita tidak perlu menginstal `vuex` karena itu sudah include dengan Nuxt.js.

Kita sekarang bisa menggunakan `this.$store` di dalam komponen kita:

```html
<template>
  <button @click="$store.commit('increment')">{{ $store.state.counter }}</button>
</template>
```
