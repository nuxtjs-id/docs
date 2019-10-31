---
title: "API: Kelas ModuleContainer (The ModuleContainer Class)"
description: Kelas ModuleContainer Nuxt
---

- Sumber: **[core/module.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/core/src/module.js)**

Semua [module](/guide/modules) akan dipanggil dalam konteks instance `ModuleContainer`.

## Tapable plugins

Kami dapat mendaftarkan hook pada event siklus hidup tertentu (life cycle events).

```js
nuxt.moduleContainer.plugin('ready', async (moduleContainer) => {
  // Lakukan disini setelah semua modul siap
})
```

Di dalam konteks [modul](/guide/modules) kita bisa menggunakan ini sebagai gantinya:

```js
this.plugin('ready', async (moduleContainer) => {
  // Lakukan disini setelah semua modul siap
})
```

Plugin | Arguments       | Ketika
-------|-----------------|-----------------------------------------------------
`ready`| moduleContainer | Semua modul di dalam `nuxt.config.js` telah diinisialisasi


## Metode (Methods)

### addVendor (vendor)

**Tidak digunakan lagi sebagai `vendor`**

Tambahkan ke `options.build.vendor` dan terapkan filter unik.

### addTemplate (template)

- **template**: `String` or `Object`
    - `src`
    - `options`
    - `fileName`

Render diberikan template menggunakan [template lodash](https://lodash.com/docs/4.17.4#template) selama build ke dalam project `buildDir` (`.nuxt`).

Jika `fileName` tidak disediakan atau `template` merupakan string, maka nama file target default adalah `[dirName].[fileName].[pathHash].[ext]`.

Metode ini mengembalikan object final `{ dist, src, options }`.

### addPlugin (template)

Mendaftarkan plugin menggunakan `addTemplate` dan menambahkannya ke opsi `plugins[]` pertama.

Anda dapat menggunakan `template.ssr: false` untuk me-nonaktifkan plugin yang termasuk dalam bundel SSR.

### addServerMiddleware (middleware)

Push middleware ke [options.serverMiddleware](/api/configuration-servermiddleware).

### extendBuild (fn)

Mengizinkan memperluas (extend) konfigurasi build webpack dengan melakukan (chaining) fungsi [options.build.extend](/api/configuration-build#extend).

### extendRoutes (fn)

Mengizinkan memperluas (extend) routes dengan melakukan (chaining) fungsi [options.build.extendRoutes](/api/configuration-router#extendroutes).

### extendPlugins (fn)

Mengizinkan memperluas (extend) plugins dengan melakukan (chaining) fungsi [options.extendPlugins](/api/configuration-extend-plugins).

### addModule (moduleOpts, requireOnce)

*Async function*

Mendaftarkan modul. `moduleOpts` bisa berupa string atau array (`[src, options]`). 
Jika `requireOnce` adalah `true` dan module yang di resolve melakukan exports `meta`, itu akan mencegah mendaftarkan modul yang sama dua kali (duplikasi).

### requireModule (moduleOpts)

*Async function*

Adalah jalan pintas untuk `addModule(moduleOpts, true)`

## Hooks

Kita dapat mendaftarkan hook pada event siklus hidup (life cycle events) tertentu.

Hook                      | Arguments                  | Ketika
--------------------------|----------------------------|--------------------------------------------------------------------------------------
 `modules:before`         | (moduleContainer, options) | Dipanggil sebelum membuat kelas ModuleContainer, berguna untuk membebani metode dan opsi.
 `modules:done`           | (moduleContainer)          | Dipanggil ketika semua modul telah dimuat.

