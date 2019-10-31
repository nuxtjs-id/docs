---
title: "API: Kelas Renderer (The Renderer Class)"
description: Kelas Renderer Nuxt
---

- Sumber: **[vue-renderer/renderer.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/vue-renderer/src/renderer.js)**

Kelas ini mengekspor middleware terhubung yang menangani dan melayani semua permintaan SSR dan aset.

## Hooks

Kita dapat mendaftarkan hook pada event siklus hidup (life cycle events) tertentu.

Hook                      | Arguments                | Ketika
--------------------------|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 `render:before`          | (renderer, options)      | Sebelum menyiapkan middleware dan resources untuk kelas Renderer, berguna untuk membebani beberapa metode atau opsi.
 `render:setupMiddleware` | (app) *connect instance* | Sebelum Nuxt menambahkan tumpukan middleware. Kita dapat menggunakannya untuk mendaftarkan middleware pada sisi server kustom.
 `render:errorMiddleware` | (app) *connect instance* | Sebelum menambahkan middleware error Nuxt, berguna untuk menambahkan middleware Anda sendiri sebelum menggunakan Nuxt's. Lihat [Sentry module](https://github.com/nuxt-community/sentry-module/blob/master/lib/module.js#L122) untuk info lebih lanjut.
 `render:resourcesLoaded` | (resources)              | Dipanggil setelah resources untuk renderer dimuat (client manifest, server bundle, dll).
 `render:done`            |  (renderer)              | SSR Middleware dan semua resources sudah siap (Renderer siap)
 `render:routeContext`    |  (context.nuxt)          | *Setiap kali sebuah rute render oleh server, dan sebelum hook `render:route`*. Dipanggil sebelum membuat serial konteks Nuxt ke dalam `window .__ NUXT__`, berguna untuk menambahkan beberapa data yang dapat Anda ambil di sisi klien.
 `render:route`           |  (url, result, context)  | *Setiap kali rute diberikan oleh server (server-rendered)*. Dipanggil sebelum mengirim kembali request ke browser.
 `render:routeDone`       |  (url, result, context)  | *Setiap kali rute diberikan oleh server (server-rendered)*. Dipanggil setelah response dikirim ke browser.
