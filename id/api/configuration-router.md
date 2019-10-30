---
title: "API: Properti router"
description: Properti router memungkinkan Anda untuk mengkustomize router Nuxt.js.
---

> Properti router memungkinkan Anda menyesuaikan router Nuxt.js ([vue-router](https://router.vuejs.org/en/)).

## base

- Type: `String`
- Default: `'/'`

URL `base` pada aplikasi. Misalnya, jika seluruh aplikasi satu halaman disajikan di bawah `/app/`, maka `base` harus menggunakan nilai `'/app/'`.

Ini bisa berguna jika Anda perlu melayani Nuxt sebagai root konteks yang berbeda, dari dalam situs Web yang lebih besar. Perhatikan bahwa Anda mungkin atau juga mungkin tidak melakukan pengaturan Server Web Proxy Depan (Front Proxy Web Server).

Jika Anda ingin memiliki redirect ke `router.base`, kamu bisa melakukannya [menggunakan Hook, lihat *Redirect ke router.base saat tidak di root*](/api/configuration-hooks#redirect-to-router-base-when-not-on-root).

Contoh (`nuxt.config.js`):
```js
export default {
  router: {
    base: '/app/'
  }
}
```

<div class="Alert Alert-blue">

Ketika menetapkan `base`, Nuxt.js juga akan menambahkan dalam header dokumen `<base href="{{ router.base }}"/>`.

</div>

> Opsi ini diberikan langsung ke vue-router [base](https://router.vuejs.org/api/#base).

## routeNameSplitter

- Type: `String`
- Default: `'-'`

Anda mungkin ingin mengubah pemisah (separator) antara nama rute yang digunakan Nuxt.js. Anda dapat melakukannya melalui opsi `routeNameSplitter` di dalam file konfigurasi Anda.
Bayangkan kita memiliki file halaman `pages/posts/_id.vue`. Nuxt akan menghasilkan nama rute secara program, dalam kasus ini `posts-id`. Merubak config `routeNameSplitter` menjadi `/` maka akan berubah menjadi `posts/id`.

Contoh (`nuxt.config.js`):
```js
export default {
  router: {
    routeNameSplitter: '/'
  }
}
```

## extendRoutes

- Type: `Function`

Anda mungkin ingin mengembangkan rute yang dibuat oleh Nuxt.js. Anda bisa melakukannya melalui opsi `extendRoutes`.

Contoh menambahkan rute khusus:

`nuxt.config.js`
```js
export default {
  router: {
    extendRoutes (routes, resolve) {
      routes.push({
        name: 'custom',
        path: '*',
        component: resolve(__dirname, 'pages/404.vue')
      })
    }
  }
}
```

Jika Anda ingin melakukan sortir terhadap rute Anda, Anda dapat menggunakan fungsi `sortRoutes(routes)` dari `@nuxt/utils`:

`nuxt.config.js`
```js
import { sortRoutes } from '@nuxt/utils'
export default {
  router: {
    extendRoutes (routes, resolve) {
      // Tambahkan beberapa rute disini ...

      // kemudian urutkan (sorting) mereka
      sortRoutes(routes)
    }
  }
}
```

Skema rute harus mengedepankan skema [vue-router](https://router.vuejs.org/en/).

<div class="Alert Alert--orange">

<b>Peringatan:</b> saat menambahkan rute yang digunakan [Named Views](/guide/routing#named-views), jangan lupa untuk menambahkan `chunkNames` yang sesuai dengan nama` components`.

</div>

`nuxt.config.js`
```js
export default {
  router: {
    extendRoutes (routes, resolve) {
      routes.push({
        path: '/users/:id',
        components: {
          default: resolve(__dirname, 'pages/users'), // atau routes[index].component
          modal: resolve(__dirname, 'components/modal.vue')
        },
        chunkNames: {
          modal: 'components/modal'
        }
      })
    }
  }
}
```

## fallback

- Type: `boolean`
- Default: `false`

Mengontrol apakah router harus kembali (melakukan fallback) ke mode hash ketika browser tidak mendukung history.pushState tetapi mode diatur ke histori.

Menyetel ini ke false pada dasarnya membuat setiap navigasi router-link menjadi full refresh di IE9. Ini berguna ketika aplikasi server-rendered dan perlu bekerja di IE9, karena URL mode hash tidak berfungsi dengan SSR.

> Opsi ini diberikan langsung ke vue-router [fallback](https://router.vuejs.org/api/#fallback).

## linkActiveClass

- Type: `String`
- Default: `'nuxt-link-active'`

Secara global mengkonfigurasikan default [`<nuxt-link>`](/api/components-nuxt-link) active class.

Contoh (`nuxt.config.js`):

```js
export default {
  router: {
    linkActiveClass: 'active-link'
  }
}
```

> Pilihan ini diberikan langsung ke vue-router [linkactiveclass](https://router.vuejs.org/api/#linkactiveclass).

## linkExactActiveClass

- Type: `String`
- Default: `'nuxt-link-exact-active'`

Secara global mengkonfigurasikan default [`<nuxt-link>`](/api/components-nuxt-link) exact active class.

Contoh (`nuxt.config.js`):

```js
export default {
  router: {
    linkExactActiveClass: 'exact-active-link'
  }
}
```

> Pilihan ini diberikan langsung ke vue-router [linkexactactiveclass](https://router.vuejs.org/api/#linkexactactiveclass).

## linkPrefetchedClass

- Type: `String`
- Default: `false`

Secara global mengkonfigurasikan default [`<nuxt-link>`](/api/components-nuxt-link) prefetch class (fitur di nonaktifkan secara default)

Contoh (`nuxt.config.js`):

```js
export default {
  router: {
    linkPrefetchedClass: 'nuxt-link-prefetched'
  }
}
```

## middleware

- Type: `String` or `Array`
  - Items: `String`

Menetapkan middleware default untuk setiap halaman aplikasi.

Contoh:

`nuxt.config.js`

```js
export default {
  router: {
    // Jalankan middleware/user-agent.js di setiap halaman
    middleware: 'user-agent'
  }
}
```

`middleware/user-agent.js`
```js
export default function (context) {
  // Tambah properti userAgent dalam konteks (tersedia dalam `data` dan `fetch`)
  context.userAgent = process.server ? context.req.headers['user-agent'] : navigator.userAgent
}
```

Untuk mempelajari lebih lanjut tentang middleware, lihat [panduan middleware](/guide/routing#middleware).

## mode

- Type: `String`
- Default: `'history'`

Mengkonfigurasi mode router, ini tidak disarankan untuk diubah karena berkaitan dengan `rendering` sisi-server (SSR).

Contoh (`nuxt.config.js`):

```js
export default {
  router: {
    mode: 'hash'
  }
}
```

> Pilihan ini diberikan langsung ke vue-router [mode](https://router.vuejs.org/api/#mode).

## parseQuery / stringifyQuery

- Type: `Function`

Memberikan fungsi parse / stringify string kueri secara kustom. Mengganti default.

> Pilihan ini diberikan langsung ke the vue-router [parseQuery / stringifyQuery](https://router.vuejs.org/api/#parsequery-stringifyquery).

## prefetchLinks

> Ditambahkan di Nuxt v2.4.0

- Type: `Boolean`
- Default: `true`

Konfigurasikan `<nuxt-link>` untuk mengambil (prefetch) halaman *code-splitted* ketika terdeteksi dalam viewport.
Membutuhkan [IntersectionObserver](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) untuk didukung (lihat [CanIUse](https://caniuse.com/#feat=intersectionobserver)).

Kami sarankan untuk melakukan polyfilling secara kondisional pada fitur ini dengan layanan seperti [Polyfill.io](https://polyfill.io):

`nuxt.config.js`

```js
export default {
  head: {
    script: [
      { src: 'https://polyfill.io/v2/polyfill.min.js?features=IntersectionObserver', body: true }
    ]
  }
}
```

Untuk me-nonaktifkan prefetching pada link spesifik, Anda dapat menggunakan prop `no-prefetch`. Sejak Nuxt.js v2.10.0, Anda juga dapat menggunakan prop `prefetch` yang disetel ke` false`:

```html
<nuxt-link to="/about" no-prefetch>About page not pre-fetched</nuxt-link>
<nuxt-link to="/about" :prefetch="false">About page not pre-fetched</nuxt-link>
```

Untuk menonaktifkan prefetching pada semua link, atur `prefetchLinks` ke `false`:

```js
// nuxt.config.js
export default {
  router: {
    prefetchLinks: false
  }
}
```

Sejak Nuxt.js v2.10.0, jika Anda sudah mengatur `prefetchLinks` ke `false` tetapi Anda menginginkan untuk melakukan prefetch pada spesifik link, Anda dapat menggunakan prop `prefetch`:

```html
<nuxt-link to="/about" prefetch>About page pre-fetched</nuxt-link>
```

## scrollBehavior

- Type: `Function`

Opsi `scrollBehavior` memungkinkan Anda menentukan perilaku secara kustom untuk posisi scroll yang berada di antara rute. Metode ini dipanggil setiap kali halaman di-render. Untuk mempelajari lebih lanjut tentang itu, lihat [vue-router scrollBehavior documentation](https://router.vuejs.org/guide/advanced/scroll-behavior.html).

<div class="Alert Alert-blue">

Mulai dari v2.9.0, Anda dapat menggunakan file untuk menimpa scrollBehavior router, file ini harus ditempatkan di `~/app/router.scrollBehavior.js`.

</div>

Anda dapat melihat file Nuxt default `router.scrollBehavior.js` disini: [packages/vue-app/template/router.scrollBehavior.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/vue-app/template/router.scrollBehavior.js).

Contoh memaksa posisi scroll ke atas untuk setiap rute:

`app/router.scrollBehavior.js`
```js
export default function (to, from, savedPosition) {
  return { x: 0, y: 0 }
}
```

## trailingSlash

- Type: `Boolean` or `undefined`
- Default: `undefined`
- Available since: v2.10

Jika opsi ini disetel ke true, garis miring akan ditambahkan ke setiap rute. Jika disetel ke false, maka mereka akan dihapus.

**Perhatian**: Opsi ini tidak boleh ditetapkan tanpa persiapan dan harus diuji secara menyeluruh. Ketika melakukan pengaturan `router.trailingSlash` untuk sesuatu yang lain selain `undefined`, maka rute yang berlawanan akan berhenti bekerja. Dengan demikian 301 redirect harus ada dan *internal linking* Anda harus diadaptasi dengan benar. Jika Anda mengatur `trailingSlash` menjadi `true`, kemudian hanya `example.com/abc/` akan berfungsi tetapi tidak dengan `example.com/abc`. Pada posisi false, dan sebaliknya.
