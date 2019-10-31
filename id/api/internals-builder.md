---
title: "API: Kelas Builder (The Builder Class)"
description: Kelas Nuxt `Builder`
---

- Sumber: **[builder/builder.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/builder/src/builder.js)**

## Hooks

Kita dapat mendaftarkan hooks pada event (life cycle) tertentu.

```js
// Tambahkan hook untuk melakukan build
this.nuxt.hook('build:done', (builder) => {
  ...
})
```

Hook                 | Arguments                                  | Ketika
---------------------|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------
`build:before`       | (nuxt, buildOptions)                       | Sebelum Nuxt build dimulai
`builder:prepared`     | (nuxt, buildOptions)                       | Direktori build telah dibuat
`builder:extendPlugins`| (plugins)                                  | Melakukan generate plugin
`build:templates`    | ({ templatesFiles, templateVars, resolve }) | Melakukan generate file template `.nuxt`
`build:extendRoutes` | (routes, resolve)                          | Melakukan generate routes
`build:config`       | (webpackConfigs)                           | Sebelum konfigurasi kompiler
`build:compile`      | ({ name, compiler })                       | Sebelum kompilasi webpack (compiler adalah instance `Compiler` webpack), jika mode universal, dipanggil dua kali dengan nama `'client'` dan `'server'`
`build:compiled`     | ({ name, compiler, stats })                | Build webpack selesai
`build:done`         | (nuxt)                                     | Build Nuxt selesai
