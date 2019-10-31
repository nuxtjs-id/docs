---
title: "API: Properti halaman `transition`"
description: Nuxt.js menggunakan komponen `<transition>` untuk memungkinkan Anda membuat transisi/animasi luar biasa di antara halaman Anda.
---

> Nuxt.js menggunakan komponen [`<transition>`](https://vuejs.org/v2/guide/transitions.html#Transitioning-Single-Elements-Components) untuk memungkinkan Anda membuat transisi/animasi luar biasa di antara halaman Anda.

- **Type:** `String` or `Object` or `Function`

Untuk menentukan transisi khusus untuk rute tertentu, cukup tambahkan kunci `transisi` ke komponen halaman.

```js
export default {
  // Dapat berupa String
  transition: ''
  // Atau Object
  transition: {}
  // atau Function
  transition (to, from) {}
}
```

## String

Jika key `transisi` diset sebagai string, itu akan digunakan sebagai` transition.name`.

```js
export default {
  transition: 'test'
}
```

Nuxt.js akan menggunakan pengaturan ini untuk mengatur komponen sebagai berikut:

```html
<transition name="test">
```

## Object

Jika key `transisi` ditetapkan sebagai objek:

```js
export default {
  transition: {
    name: 'test',
    mode: 'out-in'
  }
}
```

Nuxt.js akan menggunakan pengaturan ini untuk mengatur komponen sebagai berikut:

```html
<transition name="test" mode="out-in">
```

Objek `transisi` dapat memiliki properti berikut:

| key                | Tipe      | Default    | Penjelasan                                                                                                                                                                                                                 |
|--------------------|-----------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`             | `String`  | `"page"`   | Nama transisi diterapkan pada semua transisi rute.                                                                                                                                                                  |
| `mode`             | `String`  | `"out-in"` | Mode transisi diterapkan pada semua rute, lihat [dokumentasi Vue.js](https://vuejs.org/v2/guide/transitions.html#Transition-Modes).                                                                                       |
| `css`              | `Boolean` | `true`     | Apakah akan menerapkan kelas transisi CSS. Default adalah `true`. Jika diganti menjadi `false`, hanya akan memicu hook JavaScript yang didaftarkan melalui event component.                                                                        |
| `duration`         | `Integer` | n/a        | Durasi (dalam milidetik) yang diterapkan pada transisi, lihat [dokumentasi Vue.js](https://vuejs.org/v2/guide/transitions.html#Explicit-Transition-Durations).                                                           |
| `type`             | `String`  | n/a        | Tentukan jenis event transisi untuk menunggu menentukan waktu akhir transisi. Nilai yang tersedia adalah `"transisi"` dan `"animasi"`. Secara default, ini akan secara otomatis mendeteksi tipe yang memiliki durasi lebih lama. |
| `enterClass`       | `String`  | n/a        | Keadaan awal dari kelas transisi (transition class). Lihat [dokumentasi Vue.js](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes).                                                                             |
| `enterToClass`     | `String`  | n/a        | Status akhir untuk transisi. Lihat [dokumentasi Vue.js](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes).                                                                                    |
| `enterActiveClass` | `String`  | n/a        | Class yang diterapkan di seluruh durasi transisi. Lihat [dokumentasi Vue.js](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes).                                                                |
| `leaveClass`       | `String`  | n/a        | Keadaan awal dari kelas transisi (transition class). Lihat [dokumentasi Vue.js](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes).                                                                             |
| `leaveToClass`     | `String`  | n/a        | Status akhir untuk transisi (transition class). Lihat [dokumentasi Vue.js](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes).                                                                                    |
| `leaveActiveClass` | `String`  | n/a        | Kelas yang diterapkan di seluruh durasi transisi. Lihat [dokumentasi Vue.js](https://vuejs.org/v2/guide/transitions.html#Custom-Transition-Classes).                                                                |

Anda juga dapat mendefinisikan metode dalam `pageTransition` untuk [hook JavaScript](https://vuejs.org/v2/guide/transitions.html#JavaScript-Hooks):

- `beforeEnter(el)`
- `enter(el, done)`
- `afterEnter(el)`
- `enterCancelled(el)`
- `beforeLeave(el)`
- `leave(el, done)`
- `afterLeave(el)`
- `leaveCancelled(el)`

*Catatan: itu juga merupakan ide yang baik untuk secara eksplisit menambahkan `css: false` untuk transisi JavaScript-only sehingga Vue dapat melewati deteksi CSS. Ini juga mencegah aturan CSS (CSS rules) dari kesalahan transisi secara tidak sengaja.*

### Mode Transition

**Mode transisi default untuk halaman berbeda dari mode default di Vue.js**. Mode `transition` yang secara default diatur adalah `out-in`. Jika Anda ingin menjalankan leaving dan entering transisi secara bersamaan, Anda harus mengatur mode ke string kosong `mode: ''`. 

```js
export default {
  transition: {
    name: 'test',
    mode: ''
  }
}
```

## Fungsi

Jika tombol `transisi` diatur sebagai fungsi:

```js
export default {
  transition (to, from) {
    if (!from) { return 'slide-left' }
    return +to.query.page < +from.query.page ? 'slide-right' : 'slide-left'
  }
}
```

Transisi yang diterapkan pada navigasi:

- `/` to `/posts` => `slide-left`,
- `/posts` to `/posts?page=3` => `slide-left`,
- `/posts?page=3` to `/posts?page=2` => `slide-right`.
