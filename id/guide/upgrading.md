---
title: Upgrade
description: Melakukan upgrade Nuxt.js sangat cepat, tetapi lebih terlibat daripada memperbarui package.json Anda
---

> Upgrading Nuxt.js is quick, but more involved than updating your package.json

## Mulai

1. Periksa [catatan rilis](/guide/release-notes) untuk versi yang ingin Anda tingkatkan untuk melihat apakah ada instruksi tambahan untuk rilis tertentu.
2. Perbarui versi yang ditentukan untuk package `nuxt` di dalam file `package.json`.

Setelah langkah ini, instruksi bervariasi tergantung pada apakah Anda menggunakan Yarn or NPM. _[Yarn](https://yarnpkg.com/en/docs/usage) adalah manajer paket yang disukai untuk bekerja dengan Nuxt karena itu adalah alat pengembangan yang ujiannya telah ditentang._

## Yarn

1. hapus file `yarn.lock`
2. hapus direktori `node_modules`
3. Jalankan perintah `yarn`
4. Setelah instalasi selesai dan Anda telah menjalankan tes Anda, pertimbangkan untuk melakukan upgrade dependensi lainnya juga. Perintah `yarn outdated` dapat anda gunakan.

## NPM

1. hapus file `package-lock.json`
2. hapus direktori `node_modules`
3. Jalankan perintah `npm install`
4. Setelah instalasi selesai dan Anda telah menjalankan tes Anda, pertimbangkan untuk melakukan upgrade dependensi lainnya juga. Perintah `npm outdated` dapat anda gunakan.
