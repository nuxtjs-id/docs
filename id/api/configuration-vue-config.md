---
title: "API: Properti vue.config"
description: Objek konfigurasi untuk Vue.config
---

- Type: `Object`
- Default: `{ silent: !isDev, performance: isDev }`

> Properti vue.config menyediakan jembatan konfigurasi secara langsung untuk `Vue.config`


**Contoh**

```js
export default {
  vue: {
    config: {
      productionTip: true,
      devtools: false
    }
  }
}
```

Konfigurasi ini akan mengarah ke Vue.config dibawah ini:

``` js
Vue.config.productionTip // true
Vue.config.devtools // false
Vue.config.silent // !isDev [default value]
Vue.config.performance // isDev [default value]
```


Untuk mempelajari lebih lanjut tentang API `Vue.config`, lihat [dokumentasi Vue official](https://vuejs.org/v2/api/#Global-Config)
