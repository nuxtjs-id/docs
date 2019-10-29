---
title: "API: Properti cli"
description: Nuxt.js memungkinkan Anda menyesuaikan konfigurasi CLI.
---

> Nuxt.js memungkinkan Anda menyesuaikan konfigurasi CLI.

## bannerColor

- Type: `String`
- Default: `green`

Merubah warna judul 'Nuxt.js' pada banner CLI.

Warna-warna yang tersedia:
- `black`, `red`, `green`, `yellow`, `blue`, `magenta`, `cyan`, `white`, `gray`, `redBright`, `greenBright`, `yellowBright`, `blueBright`, `magentaBright`, `cyanBright`, `whiteBright`

Contoh (`nuxt.config.js`):

```js
export default {
  cli: {
    bannerColor: 'yellow'
  }
}
```
