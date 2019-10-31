---
title: "API: Pengantar Modul Nuxt"
description: Lebih memahami Nuxt internal
---

Nuxt.js memiliki arsitektur modular sepenuhnya yang memungkinkan pengembang memperluas bagian Nuxt Core menggunakan API yang fleksibel.

Lihat [Panduan Modules](/guide/modules) untuk informasi lebih rinci jika tertarik mengembangkan modul Anda sendiri.

Bagian ini membantu membiasakan diri dengan Nuxt internal dan dapat digunakan sebagai referensi untuk memahaminya dengan lebih baik saat menulis modul Anda sendiri.

### Core

Kelas-kelas ini adalah jantung dari Nuxt dan harus ada pada saat runtime dan pada saat build.

#### Nuxt

- [Kelas `Nuxt`](/api/internals-nuxt)
- Sumber: [core/nuxt.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/core/src/nuxt.js)

#### Renderer

- [Kelas `Renderer`](/api/internals-renderer)
- Sumber: [vue-renderer/renderer.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/vue-renderer/src/renderer.js)

#### ModuleContainer

- [Kelas `ModuleContainer`](/api/internals-module-container)
- Sumber: [core/module.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/core/src/module.js)

### Build

Kelas-kelas ini hanya diperlukan untuk mode build atau dev.

#### Builder

- [Kelas `Builder`](/api/internals-builder)
- Sumber: [builder/builder.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/builder/src/builder.js)

#### Generator

- [Kelas `Generator`](/api/internals-generator)
- Sumber: [generator/generator.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/generator/src/generator.js)

### Common

#### Utils

- Sumber: [utils/src](https://github.com/nuxt/nuxt.js/blob/dev/packages/utils/src)

#### Options

- Sumber: [config/options.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/config/src/options.js)

## Pengemasan (Packaging) & Penggunaan

Nuxt mengekspor semua kelas secara default. Untuk mengimpornya:

```js
import { Nuxt, Builder, Utils } from 'nuxt'
```

## Pola umum (Common patterns)

Semua kelas Nuxt memiliki referensi ke instance dan opsi `nuxt`, dengan cara ini kami selalu memiliki API yang konsisten di seluruh kelas untuk mengakses` opsi` dan `nuxt`.

```js
class SomeClass {
  constructor (nuxt) {
    super()
    this.nuxt = nuxt
    this.options = nuxt.options
  }

  someFunction () {
    // Kita memiliki akses ke `this.nuxt` dan` this.options`
  }
}
```

Kelas-kelas bersifat *plugable* sehingga harus mendaftarkan sebuah plugin pada container `nuxt` utama untuk mendaftarkan lebih banyak hook.

```js
class FooClass {
  constructor (nuxt) {
    super()
    this.nuxt = nuxt
    this.options = nuxt.options

    this.nuxt.callHook('foo', this)
  }
}
```

Jadi kita bisa menghubungkan ke modul `foo` seperti ini:

```js
nuxt.hook('foo', (foo) => {
  // ...
})
```
