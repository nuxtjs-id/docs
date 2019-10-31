---
title: "API: Properti watchers"
description: Properti watchers memungkinkan Anda menimpa konfigurasi watchers.
---

- Type: `Object`
- Default: `{}`

> Properti watchers memungkinkan Anda menimpa konfigurasi watchers di dalam nuxt.config.js.

## chokidar

- Type: `Object`
- Default: `{}`

Untuk mempelajari lebih lanjut tentang opsi chokidar, lihat [API chokidar](https://github.com/paulmillr/chokidar#api).

## webpack

- Type: `Object`
- Default:

```js
watchers: {
  webpack: {
    aggregateTimeout: 300,
    poll: 1000
  }
}
```

Untuk mempelajari lebih lanjut tentang opsi watch webpack, lihat [dokumentasi webpack](https://webpack.js.org/configuration/watch/#watchoptions).
