---
title: "API: Properti srcDir"
description: Menentukan direktori asal pada aplikasi Nuxt.js Anda
---

- Type: `String`
- Default: [rootDir value](/api/configuration-rootdir)

> Menentukan direktori asal pada aplikasi nuxt.js Anda.

Contoh (`nuxt.config.js`):

```js
export default {
  srcDir: 'client/'
}
```

Kemudian, struktur aplikasi seperti:
```bash
-| app/
---| node_modules/
---| client/
------| assets/
------| components/
------| layouts/
------| middleware/
------| pages/
------| plugins/
------| static/
------| store/
---| nuxt.config.js
---| package.json
```

Opsi ini berguna ketika memiliki server kustom yang menggunakan Nuxt.js, jadi semua dependensi npm dapat dikelompokkan kembali dalam satu `package.json`.
