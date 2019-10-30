---
title: "API: Properti loading indicator"
description: Menampilkan loading indicator pada saat halaman SPA dimuat!
---

> Menampilkan loading indicator pada saat halaman SPA dimuat!

Ketika menjalankan Nuxt.js pada mode SPA, disana tidak ada konten dari sisi server pada saat pertamakali load. Jadi, daripada menampilkan halaman kosong, lebih baik menampilkan semacam spinner.

Properti ini memiliki 3 perbedaan tipe: `string` atau `false` atau `object`. Jika nilai string diberikan maka akan di konversi menjadi object.

Nilai default nya adalah:
```js
{
  name: 'circle',
  color: '#3B8070',
  background: 'white'
}
```

## Indikator Built-in

Indikator ini di-porting dari proyek [Spinkit](http://tobiasahlin.com/spinkit) project. Anda bisa menggunakan halaman demo untuk melihatnya.

- circle
- cube-grid
- fading-circle
- folding-cube
- chasing-dots
- nuxt
- pulse
- rectangle-bounce
- rotating-plane
- three-bounce
- wandering-cubes

Indikator built-in mendukung opsi `color` dan opsi `background`.

## Indikator custom

Jika Anda memerlukan indikator milik Anda sendiri, nilai String atau Name key juga bisa menjadikannya sebagai path pada templat html dari kode sumber (source code) indikator! Semua pengaturan juga melewati templat.

Nuxt's built-in [sumber kode](https://github.com/nuxt/nuxt.js/tree/dev/packages/vue-app/template/views/loading) juga tersedia jika Anda membutuhkan dasar!
