---
title: "API: Properti globalName"
description: Nuxt.js memungkinkan Anda untuk dapat memodifikasi ID global yang digunakan di dalam main Template HTML serta nama (main instance Vue) dan opsi lainnya.
---

> Nuxt.js memungkinkan Anda untuk dapat memodifikasi ID global yang digunakan di dalam main Template HTML serta nama (main instance Vue) dan opsi lainnya.

- Type: `String`
- Default: `nuxt`

Contoh:

`nuxt.config.js`

```js
{
  globalName: 'myCustomName'
}
```

Ini harus menjadi pengidentifikasi JavaScript yang valid.

## Properti-properti global

> Kustomisasi nama-nama global spesifik yang secara default berbasis pada `globalName`.

- Type: `Object`
- Default:

```js
{
  id: globalName => `__${globalName}`,
  nuxt: globalName => `$${globalName}`,
  context: globalName => `__${globalName.toUpperCase()}__`,
  pluginPrefix: globalName => globalName,
  readyCallback: globalName => `on${_.capitalize(globalName)}Ready`,
  loadedCallback: globalName => `_on${_.capitalize(globalName)}Loaded`
},
```

