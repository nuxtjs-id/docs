---
title: "API: Kelas (The Nuxt Class)"
description: Kelas inti dari Nuxt
---

- Sumber: **[core/nuxt.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/core/src/nuxt.js)**

Ini adalah container inti yang memungkinkan semua modul dan kelas saling berkomunikasi. Semua modul memiliki akses ke Nuxt instance menggunakan `this.nuxt`.

## Hooks

Kita dapat mendaftarkan hook pada event siklus hidup (life cycle events) tertentu.

```js
nuxt.hook('ready', async (nuxt) => {
  // Kustomisasi kode anda di sini
})
```

Plugin   | Arguments              | Ketika
---------|------------------------|------------------------------------------------------------------------------
`ready`  | (nuxt)                 | Nuxt siap bekerja (`ModuleContainer` dan `Renderer` telah ready).
`error`  | (error)                | Kesalahan tidak tertangani saat memanggil hook.
`close`  | (nuxt)                 | Contoh Nuxt ditutup secara perlahan.
`listen` | (server, {host, port}) | Server Nuxt **internal** mulai melakukan listening. (Gunakan `nuxt start` atau `nuxt dev`).
