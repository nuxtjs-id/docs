---
title: "API: The <nuxt-child> Component"
description: Display the current page.
---

> Komponen ini digunakan untuk menampilkan "children" komponen pada [rute bersarang](/guide/routing#nested-routes).

Contoh:

```bash
-| pages/
---| parent/
------| child.vue
---| parent.vue
```

File tree di atas akan menghasilkan rute sebagai berikut:

```js
[
  {
    path: '/parent',
    component: '~/pages/parent.vue',
    name: 'parent',
    children: [
      {
        path: 'child',
        component: '~/pages/parent/child.vue',
        name: 'parent-child'
      }
    ]
  }
]
```

Untuk menampilkan komponen `child.vue`, kita harus memasukkan `<nuxt-child/>` di dalam `pages/parent.vue`:

```html
<template>
  <div>
    <h1>Saya adalah tampilan utama</h1>
    <nuxt-child :foobar="123" />
  </div>
</template>
```

`<nuxt-child/>` dapat menerima `keep-alive` dan `keep-alive-props`:

```html
<template>
  <div>
    <nuxt-child keep-alive :keep-alive-props="{ exclude: ['modal'] }" />
  </div>
</template>

<!-- akan di konversi menjadi seperti ini -->
<div>
  <keep-alive :exclude="['modal']">
    <router-view />
  </keep-alive>
</div>
```

> Komponen child dapat juga menerima properti seperti komponen Vue regular.

Untuk melihat contoh, silahkan lihat [contoh nested-routes](/examples/nested-routes).

## Named View

> Diperkenalkan di Nuxt v2.4.0

`<nuxt-child/>` menerima prop `name` untuk me-render named-view:

```html
<template>
  <div>
    <nuxt-child name="top" />
    <nuxt-child />
  </div>
</template>
```

Untuk melihat contoh, silahkan lihat [contoh named-views](/examples/named-views).
