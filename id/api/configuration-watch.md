---
title: "API: Properti watch"
description: Properti watch memungkinkan Anda untuk melakukan "watch" pada kustom file untuk restart server.
---

- Type: `Object`
- Default: `[]`

> Properti watch memungkinkan Anda untuk melakukan "watch" pada kustom file untuk restart server.

```js
watch: ['~/custom/*.js']
```

[chokidar](https://github.com/paulmillr/chokidar) digunakan untuk mengatur watchers. Untuk mempelajari lebih lanjut tentang opsi pola chokidar, see the [chokidar API](https://github.com/paulmillr/chokidar#api).
