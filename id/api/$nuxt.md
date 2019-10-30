---
title: "$nuxt: Bantuan NuxtJS"
description: $nuxt adalah bantuan yang dirancang untuk meningkatkan pengalaman pengguna.
---

`$nuxt` adalah bantuan yang dirancang untuk meningkatkan pengalaman pengguna.

- `isOffline`
  - Type: `Boolean`
  - Description: `true` ketika koneksi internet offline
- `isOnline`
  - Type: `Boolean`
  - Description: Kebalikan dari `isOffline`

Contoh:

`layouts/default.vue`:

```html
<template>
  <div>
    <div v-if="$nuxt.isOffline">Anda sedang Offline</div>
    <nuxt/>
  </div>
</template>
```
