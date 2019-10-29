---
title: "API: The <nuxt> Component"
description: Menampilkan komponen halaman di dalam tata letak
---

> Komponen ini hanya digunakan di dalam [tata letak](/guide/views#layouts) untuk menampilkan komponen halaman.

Contoh (`layouts/default.vue`):

```html
<template>
  <div>
    <div>My nav bar</div>
    <nuxt/>
    <div>My footer</div>
  </div>
</template>
```

Untuk melihat contoh, silakan lihat [contoh layouts](/examples/layouts).

**Props**:

- nuxtChildKey: `string`
  - Prop ini akan di set ke `<router-view/>`, berguna untuk membuat transisi didalam halaman dinamis dan rute yang berbeda.
  - Default: `$route.path`

Berikut merupakan 3 cara untuk menghandle internal `key` prop `<router-view/>`.

1. `nuxtChildKey` prop

  ```html
  <template>
     <div>
       <nuxt :nuxt-child-key="someKey"/>
     </div>
  </template>
  ```

2. `key` option in page components: `string` or `function`

  ```js
  export default {
    key (route) {
      return route.fullPath
    }
  }
  ```

- name: `string` (_introduced with Nuxt v2.4.0_)
  - Prop ini akan di set ke `<router-view/>`, digunakan untuk me-render named-view pada komponen halaman.
  - Default: `default`

Untuk melihat contoh, silahkan cek [contoh named-views](/examples/named-views).
