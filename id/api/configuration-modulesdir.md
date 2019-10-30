---
title: "API: Properti modulesDir"
description: Tentukan direktori modul untuk aplikasi Nuxt.js Anda
---

- Type: `Array`
- Default: `['node_modules']`

> Digunakan untuk mengatur direktori modul untuk penyelesaian jalur (path resolving), contoh: Webpack's `resolveLoading`, `nodeExternals` dan `postcss`. Konfigurasi path relatif ke [options.rootDir](/api/configuration-rootdir) (default: `process.cwd()`).

Contoh (`nuxt.config.js`):

```js
export default {
  modulesDir: ['../../node_modules']
}
```

Pengaturan bidang ini mungkin diperlukan jika proyek Anda disusun sebagai Yarn workspace-styled mono-repository.

