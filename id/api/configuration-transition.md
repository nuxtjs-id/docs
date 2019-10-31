---
title: "API: Properti transition"
description: Mengatur properti default dari transisi halaman dan tata letak.
---

## Properti pageTransition

> Nuxt v2.7.0 memperkenalkan "pageTransition" yang mendukung "transisi" untuk mengkonsolidasikan penamaan dengan kunci transisi tata letak.

- Type: `String` or `Object`

> Digunakan untuk mengatur properti default transisi halaman.

Default:
```js
{
  name: 'page',
  mode: 'out-in'
}
```

Contoh (`nuxt.config.js`):

```js
export default {
  pageTransition: 'page'
  // or
  pageTransition: {
    name: 'page',
    mode: 'out-in',
    beforeEnter (el) {
      console.log('Before enter...');
    }
  }
}
```

Key transisi pada `nuxt.config.js` digunakan untuk mengatur properti default untuk transisi halaman. Untuk mempelajari lebih lanjut tentang key yang tersedia ketika kunci `transisi` merupakan objek, lihat [properti transisi halaman](/api/pages-transition#object).

## Properti layoutTransition

- Type: `String` or `Object`

> Digunakan untuk mengatur properti default transisi tata letak. Konfigurasi sama dengan `layout`

Default:

```js
{
  name: 'layout',
  mode: 'out-in'
}
```

Contoh (`nuxt.config.js`):

```js
export default {
  layoutTransition: 'layout'
  // or
  layoutTransition: {
    name: 'layout',
    mode: 'out-in'
  }
}
```

Contoh global `css`:

```css
.layout-enter-active, .layout-leave-active {
  transition: opacity .5s
}
.layout-enter, .layout-leave-active {
  opacity: 0
}
```
