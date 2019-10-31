---
title: "API: Properti key"
description: Atur properti `key` internal komponen `<router-view>`
---

> Atur properti `key` internal komponen `<router-view>`
- **Type:** `String` or `Function`

Properti `key` disebarkan ke` <router-view> `, yang berguna untuk melakukan transisi di dalam halaman dinamis dan rute yang berbeda. Result key yang berbeda saat melakukan rerendering komponen halaman.

Ada beberapa cara untuk mengatur kunci. Untuk detail lebih lanjut, silakan merujuk ke prop `nuxtChildKey` di [komponen nuxt](/api/components-nuxt).

```js
export default {
  key (route) {
    return route.fullPath
  }
}
```
