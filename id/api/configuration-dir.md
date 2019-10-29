---
title: "API: Properti dir"
description: Mendefinisikan kustom direktori untuk aplikasi Nuxt.js Anda
---

- Type: `Object`
- Default:

```js
{
  assets: 'assets',
  app: 'app',
  layouts: 'layouts',
  middleware: 'middleware',
  pages: 'pages',
  static: 'static',
  store: 'store'
}
```

> Mendefinisikan kustom direktori untuk aplikasi Nuxt.js Anda

Example (`nuxt.config.js`):

```js
export default {
  dir: {
    assets: 'custom-assets',
    app: 'custom-app',
    layouts: 'custom-layouts',
    middleware: 'custom-middleware',
    pages: 'custom-pages',
    static: 'custom-static',
    store: 'custom-store'
  }
}
```
