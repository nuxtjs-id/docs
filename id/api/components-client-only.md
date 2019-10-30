---
title: "API: Komponen <client-only>"
description: Hanya merender komponen pada sisi klien, dan menampilkan text placeholder pada sisi server.
---

> Komponen ini dibuat untuk me-render komponen hanya pada sisi klien.

<div class="Alert Alert--orange">

**Warning:** Jika kamu menggunakan Nuxt versi < `v2.9.0`, gunakan `<no-ssr>` daripada `<client-only>`

</div>


**Props**:
- placeholder: `string`
  - Menggunakan teks placeholder sampai `<client-only />` di mount pada sisi klien.

```html
<template>
  <div>
    <sidebar />
    <client-only placeholder="Loading...">
      <!-- komponen ini hanya akan di render pada sisi klien -->
      <comments />
    </client-only>
  </div>
</template>
```

**Slots**:

- placeholder:
  - Menggunakan slot sebagai placeholder sampai `<client-only />` di mount pada sisi client.
 
 ```html
<template>
  <div>
    <sidebar />
    <client-only>
      <!-- komponen ini hanya akan di render pada sisi klien -->
      <comments />
  
      <!-- indikator loading, dirender pada sisi server -->
      <comments-placeholder slot="placeholder" />
    </client-only>
  </div>
</template>
```

Komponen ini di impor dari [egoist/vue-client-only](https://github.com/egoist/vue-client-only). Terimakasih [@egoist](https://github.com/egoist)!
