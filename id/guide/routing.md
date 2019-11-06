---
title: Routing
description: Nuxt.js menggunakan sistem file untuk menghasilkan rute aplikasi web Anda.
---

> Nuxt.js secara otomatis melakukan generate konfigurasi [vue-router](https://github.com/vuejs/vue-router) berdasarkan susunan file Vue Anda di dalam direktori `pages`.

<div class="Alert Alert--grey">

Untuk menavigasi antar halaman, kami sarankan untuk menggunakan komponen [`<nuxt-link>`](/api/components-nuxt-link).

</div>

Sebagai contoh:

```html
<template>
  <nuxt-link to="/">Home page</nuxt-link>
</template>
```

## Route Dasar

Susunan (file tree):

```bash
pages/
--| user/
-----| index.vue
-----| one.vue
--| index.vue
```

akan otomatis meng-generate:

```js
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'user',
      path: '/user',
      component: 'pages/user/index.vue'
    },
    {
      name: 'user-one',
      path: '/user/one',
      component: 'pages/user/one.vue'
    }
  ]
}
```

## Route Dinamis

Untuk menentukan rute dinamis menggunakan parameter, Anda perlu mendefinisikan file .vue atau direktori **yang di awali oleh underscore**.

<div class="Promo__Video">
  <a href="https://vueschool.io/lessons/nuxtjs-dynamic-routes?friend=nuxt" target="_blank">
    <p class="Promo__Video__Icon">
      Watch a free lesson about <strong>dynamic routes</strong> on Vue School 
    </p>
  </a>
</div>

Susunan (file tree):

```bash
pages/
--| _slug/
-----| comments.vue
-----| index.vue
--| users/
-----| _id.vue
--| index.vue
```

akan otomatis meng-generate:

```js
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'users-id',
      path: '/users/:id?',
      component: 'pages/users/_id.vue'
    },
    {
      name: 'slug',
      path: '/:slug',
      component: 'pages/_slug/index.vue'
    },
    {
      name: 'slug-comments',
      path: '/:slug/comments',
      component: 'pages/_slug/comments.vue'
    }
  ]
}
```

Seperti yang Anda lihat rute bernama `users-id` memiliki path `:id?` yang akan membuatnya bersifat opsional, jika Anda ingin membuatnya diperlukan (required), buat file `index.vue` di dalam direktori `users/_id` sebagai gantinya.

<div class="Alert Alert--orange">

**Peringatan:** rute dinamis akan diabaikan oleh perintah `generate`: [API Konfigurasi Generate](/api/configuration-generate#routes)

</div>

### Validasi Parameter Route

Nuxt.js memungkinkan Anda menentukan metode validator di dalam komponen rute dinamis Anda.

Sebagai contoh: `pages/users/_id.vue`

```js
export default {
  validate ({ params }) {
    // Harus sebuah number
    return /^\d+$/.test(params.id)
  }
}
```

Jika metode yang divalidasi tidak mengembalikan (return) `true` atau `Promise` yang me-resolve `true`, atau throws Error, Nuxt.js akan secara otomatis memuat halaman kesalahan 404 atau halaman kesalahan 500 jika terjadi eror.

Informasi lebih lanjut tentang metode validasi: [API Validasi Halaman](/api/pages-validate)

## Route bersarang (nested)

Nuxt.js memungkinkan Anda membuat rute bersarang dengan menggunakan rute anak (child) dari vue-router.

Untuk menentukan komponen parent dari rute bersarang, Anda perlu membuat file Vue dengan **nama yang sama dengan direktori** yang mengandung tampilan (view) child Anda.

<div class="Alert Alert--orange">

<b>Peringatan:</b> jangan lupa memasukan `<nuxt-child/>` di dalam komponen parent file (<code>.vue</code>).

</div>

Susunan (file tree):

```bash
pages/
--| users/
-----| _id.vue
-----| index.vue
--| users.vue
```

akan otomatis meng-generate:

```js
router: {
  routes: [
    {
      path: '/users',
      component: 'pages/users.vue',
      children: [
        {
          path: '',
          component: 'pages/users/index.vue',
          name: 'users'
        },
        {
          path: ':id',
          component: 'pages/users/_id.vue',
          name: 'users-id'
        }
      ]
    }
  ]
}
```

## Rute Bersarang Dinamis (Dynamic Nested Routes)

Skenario ini seharusnya tidak sering terjadi, tetapi mungkin dengan Nuxt.js: memiliki child yang dinamis di dalam parent yang dinamis.

Susunan (file tree):

```bash
pages/
--| _category/
-----| _subCategory/
--------| _id.vue
--------| index.vue
-----| _subCategory.vue
-----| index.vue
--| _category.vue
--| index.vue
```

akan otomatis meng-generate:

```js
router: {
  routes: [
    {
      path: '/',
      component: 'pages/index.vue',
      name: 'index'
    },
    {
      path: '/:category',
      component: 'pages/_category.vue',
      children: [
        {
          path: '',
          component: 'pages/_category/index.vue',
          name: 'category'
        },
        {
          path: ':subCategory',
          component: 'pages/_category/_subCategory.vue',
          children: [
            {
              path: '',
              component: 'pages/_category/_subCategory/index.vue',
              name: 'category-subCategory'
            },
            {
              path: ':id',
              component: 'pages/_category/_subCategory/_id.vue',
              name: 'category-subCategory-id'
            }
          ]
        }
      ]
    }
  ]
}
```

### Rute Bersarang Dinamis yang Tidak Dikenal

Jika Anda tidak tahu kedalaman struktur URL Anda, Anda dapat menggunakan `_.vue` untuk secara dinamis mencocokkan jalur bersarang (nested path).
Ini akan menangani request yang tidak cocok dengan request yang _lebih spesifik_.

Susunan (file tree):

```bash
pages/
--| people/
-----| _id.vue
-----| index.vue
--| _.vue
--| index.vue
```

Akan menangani request seperti ini:

Path | File
--- | ---
`/` | `index.vue`
`/people` | `people/index.vue`
`/people/123` | `people/_id.vue`
`/about` | `_.vue`
`/about/careers` | `_.vue`
`/about/careers/chicago` | `_.vue`

__Catatan:__ Menangani 404 halaman sekarang sudah sesuai dengan logic halaman `_.vue`. [Lebih lanjut tentang 404 redirect dapat ditemukan di sini](/guide/async-data#handling-errors).

### Tampilan Bernama (Named Views)

Untuk membuat tampilan bernama, Anda dapat menggunakan `<nuxt name="top"/>` atau komponen `<nuxt-child name="top"/>` di dalam layout/page. Untuk menentukan tampilan halaman yang bernama, kita perlu memperluas konfigurasi router file `nuxt.config.js`:
  
``` js
export default {
  router: {
    extendRoutes (routes, resolve) {
      const index = routes.findIndex(route => route.name === 'main')
      routes[index] = {
        ...routes[index],
        components: {
          default: routes[index].component,
          top: resolve(__dirname, 'components/mainTop.vue')
        },
        chunkNames: {
          top: 'components/mainTop'
        }
      }
    }
  }
}
```
Perlu melakukan extend rute dengan 2 properti `components` dan `chunkNames`. Tampilan bernama dalam contoh konfigurasi ini memiliki nama `top`.

Untuk melihat contoh, silahkan lihat [contoh named-views](/examples/named-views).

### SPA fallback

Anda juga dapat mengaktifkan fallback SPA untuk rute dinamis. Nuxt.js akan menampilkan file ekstra yang sama dengan `index.html` yang akan digunakan pada `mode: 'spa'`. Sebagian besar layanan hosting statis dapat dikonfigurasi untuk menggunakan template SPA jika tidak ada file yang cocok. Itu tidak akan termasuk informasi `head` atau HTML apa pun, tetapi masih akan melakukan resolve dan memuat data dari API.

Kami aktifkan ini pada file `nuxt.config.js` file:

``` js
export default {
  generate: {
    fallback: true, // jika Anda ingin menggunakan '404.html' dan bukan '200.html'
    fallback: 'my-fallback/file.html' // jika hosting Anda memerlukan lokasi khusus
  }
}
```

#### Implementasi untuk Surge

Surge [dapat menangani](https://surge.sh/help/adding-a-custom-404-not-found-page) kedua file `200.html` dan `404.html`. `generate.fallback` di atur ke `200.html` secara default, jadi tidak perlu mengubahnya.

#### Implementasi untuk Halaman GitHub dan Netlify

GitHub Pages dan Netlify mengenali file `404.html` secara otomatis, jadi cukup set `generate.fallback` menjadi `true` yang harus kita lakukan!

#### Implementation for Firebase Hosting

Firebase Hosting [dapat menangani](https://firebase.google.com/docs/hosting/full-config#404) file `404.html` secara otomatis, jadi cukup set `generate.fallback` menjadi `true` akan merender halaman eror dengan kode respons default 404.

## Transisi

Nuxt.js menggunakan komponen [`<transition>`](http://vuejs.org/v2/guide/transitions.html#Transitioning-Single-Elements-Components) untuk membiarkan Anda membuat transisi / animasi luar biasa di antara rute Anda.

### Pengaturan Global

<div class="Alert Alert--nuxt-green">

<b>Info:</b> Nama transisi default Nuxt.js adalah `"page"`.

</div>

Untuk menambahkan transisi fade ke setiap halaman aplikasi Anda, kita membutuhkan file CSS yang dibagikan di semua rute, jadi kita mulai dengan membuat file di folder `assets`.

Global css kita di dalam `assets/main.css`:

```css
.page-enter-active, .page-leave-active {
  transition: opacity .5s;
}
.page-enter, .page-leave-to {
  opacity: 0;
}
```

Kemudian kita tambahkan pathnya ke dalam array `css` pada file `nuxt.config.js` kita:

```js
export default {
  css: [
    '~/assets/main.css'
  ]
}
```

Informasi lebih lanjut tentang key transisi: [API Konfigurasi Transisi](/api/pages-transition)

### Pengaturan Halaman (Page)

Anda juga dapat menentukan transisi khusus untuk halaman tertentu dengan properti `transition`.

Kita tambahkan class baru di global css kita di `assets/main.css`:

```css
.test-enter-active, .test-leave-active {
  transition: opacity .5s;
}
.test-enter, .test-leave-active {
  opacity: 0;
}
```

Kemudian kita gunakan properti transisi untuk menentukan nama class yang akan digunakan untuk transisi halaman ini:

```js
export default {
  transition: 'test'
}
```

Informasi lebih lanjut tentang properti transisi: [API Transisi Halaman](/api/pages-transition)

## Middleware

> Middleware memungkinkan Anda menentukan fungsi khusus yang dapat dijalankan sebelum merender halaman atau grup halaman.

**Setiap middleware harus ditempatkan di dalam direktori `middleware/`.** Nama file akan menjadi nama middleware (`middleware/auth.js` akan menjadi middleware `auth`).

Middleware menerima [konteks](/api/context) sebagai argument pertama:

```js
export default function (context) {
  context.userAgent = process.server ? context.req.headers['user-agent'] : navigator.userAgent
}
```
Pada mode Universal, middlewares akan dipanggil pada server-side sekali (pada request pertama ke aplikasi Nuxt atau pada saat halaman di refresh) dan pada sisi klien ketika melakukan navigasi route. Pada mode SPA, middlewares akan dipanggil pada sisi klien berdasarkan permintaan pertama dan saat menavigasi ke rute selanjutnya.

Middleware akan dieksekusi secara berurutan seperti berikut:

1. `nuxt.config.js` (in the order within the file)
2. Layout yang cocok (match)
3. Halaman yang cocok (match)

Middleware bisa asynchronous. Untuk melakukan ini, return `Promise` sederhana atau gunakan argumen `callback` kedua:

`middleware/stats.js`

```js
import axios from 'axios'

export default function ({ route }) {
  return axios.post('http://my-stats-api.com', {
    url: route.fullPath
  })
}
```

Kemudian, di dalam `nuxt.config.js`, gunakan key `router.middleware`:

`nuxt.config.js`

```js
export default {
  router: {
    middleware: 'stats'
  }
}
```

Sekarang middleware `stats` akan dipanggil pada setiap perubahan route.

Anda juga dapat menambahkan middleware Anda pada layout atau halaman (page) tertentu:


`pages/index.vue` atau `layouts/default.vue`

```js
export default {
  middleware: 'stats'
}
```

Untuk melihat contoh penggunaan middleware secara nyata, silahkan lihat [example-auth0](https://github.com/nuxt/example-auth0) di GitHub.
