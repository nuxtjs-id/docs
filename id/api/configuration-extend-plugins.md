---
title: "API: Properti extendPlugins"
description: Properti extendPlugins memungkinkan Anda untuk dapat melakukan kustomize plugin Nuxt.js.
---

> Properti extendPlugins memungkinkan Anda untuk dapat melakukan kustomize plugin Nuxt.js. ([options.plugins](/api/configuration-plugins)).

- Type: `Function`
- Default: `undefined`

Anda mungkin ingin memperluas plugin atau mengubah urutan plugin yang dibuat oleh Nuxt.js.
Fungsi ini menerima array objek [plugin](/api/configuration-plugins) dan harus mengembalikan array objek plugin.

Contoh mengubah urutan plugin (`nuxt.config.js`):

```js
export default {
  extendPlugins (plugins) {
    const pluginIndex = plugins.findIndex(
      ({ src }) => src === '~/plugins/shouldBeFirst.js'
    )
    const shouldBeFirstPlugin = plugins[pluginIndex]

    plugins.splice(pluginIndex, 1)
    plugins.unshift(shouldBeFirstPlugin)

    return plugins
  }
}
```
