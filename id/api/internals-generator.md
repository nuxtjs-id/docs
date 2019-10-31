---
title: "API: Kelas Generator (The Generator Class)"
description: Kelas Generator Nuxt
---

- Sumber: **[generator/generator.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/generator/src/generator.js)**

## Hooks

Kita dapat mendaftarkan hook pada event siklus hidup (life cycle events) tertentu.

Hook                    | Arguments                   | Ketika
------------------------|-----------------------------|-----------------------------------------------
`generate:before`       | (nuxt, generateOptions)     | Hook sebelum melakukan generate
`generate:distRemoved`  | (nuxt)                      | Hook pada folder tujuan dibersihkan
`generate:distCopied`   | (nuxt)                      | Hook pada salinan file yang statis dan file yang dibangun
`generate:page`         | ({route, path, html})       | Hook untuk membiarkan pengguna memperbarui path & html
`generate:routeCreated` | ({route, path, errors})     | Hook berhasil menyimpan generate halaman
`generate:extendRoutes` | (routes)                    | Hook untuk memungkinkan pengguna memperbarui route yang akan dibuat
`generate:routeFailed`  | (route, errors)             | Hook gagal menyimpan generate halaman
`generate:done`         | (nuxt, errors)              | Hook proses generate selesai
