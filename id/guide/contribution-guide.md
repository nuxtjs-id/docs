---
title: Panduan Kontribusi
description: Segala kontribusi untuk Nuxt.js lebih dari sekedar diterima!
---

> Segala kontribusi untuk Nuxt.js lebih dari sekedar diterima!

## Laporan Issue

Cara yang bagus untuk berkontribusi pada proyek adalah mengirim laporan terperinci ketika Anda menemukan masalah.
Untuk mempermudah kontributor dan pengelola, kami menggunakan [CMTY](https://cmty.nuxtjs.org/).

Pastikan untuk menyertakan repositori reproduksi atau [CodeSandBox](https://template.nuxtjs.org/)
sehingga bug dapat direproduksi tanpa usaha keras. Semakin baik bug dapat direproduksi, semakin cepat kita dapat mulai memperbaikinya!

## Pull Request

Kami pasti melakukan cek terhadap pull-request Anda, sekalipun itu hanya untuk memperbaiki kesalahan ketik!

Namun, setiap peningkatan signifikan harus dikaitkan dengan yang sudah ada
[request fitur](https://feature.nuxtjs.org/)
atau [lapor bug](https://bug.nuxtjs.org/).

### Mulai

1. [Fork](https://help.github.com/articles/fork-a-repo/) [repositori Nuxt](https://github.com/nuxt/nuxt.js) ke akun GitHub Anda sendiri dan kemudian lakukan [clone](https://help.github.com/articles/cloning-a-repository/) ke perangkat lokal Anda.
2. Jalankan `npm install` atau `yarn install` untuk menginstal dependensi.

> _Perhatikan bahwa kedua **npm** dan **yarn** telah terlihat ketinggalan menginstal dependensi. Untuk mengatasinya, Anda dapat menghapus folder `node_modules` dalam contoh aplikasi Anda dan menginstalnya lagi atau melakukan instalasi lokal dari dependensi yang hilang._

> Jika Anda menambahkan dependency, mohon gunakan `yarn add`. File `yarn.lock` adalah sumber nyata untuk semua dependensi Nuxt.

### Persiapan
 Sebelum menjalankan tes apa pun, pastikan semua dependensi terpenuhi dan build semua package:
 ```sh
yarn
yarn build
```

### Struktur pengujian

PR yang hebat, apakah itu termasuk perbaikan bug atau fitur baru, akan selalu menyertakan tes.
Untuk menulis tes yang bagus, mari kita jelaskan struktur pengujian kami:

#### Perlengkapan (Fixtures)

Fixtures (dapat ditemukan di dalam `tests/fixtures`) mengandung beberapa aplikasi Nuxt. Untuk menjaga waktu build sesingkat mungkin,
kami tidak membangun aplikasi Nuxt milik sendiri per tes. Sebagai gantinya, fixture dibangun (`yarn test:fixtures`) sebelum menjalankan
unit tes yang sebenarnya.

Pastikan untuk **mengubah** atau **menambahkan fixture baru** saat mengirimkan PR untuk mencerminkan perubahan dengan benar.

Juga, jangan lupa untuk **melakukan build ulang** fixture setelah melakukan perubahan dengan menjalankan tes yang sesuai
dengan `jest test/fixtures/my-fixture/my-fixture.test.js`!

#### Tes unit

Tes unit dapat ditemukan di dalam `tests/unit` dan akan dieksekusi setelah membangun fixture. Server Nuxt yang baru akan digunakan
per test sehingga tidak ada status yang di share (kecuali keadaan awal dari langkah build) tersedia.

Setelah menambahkan tes unit Anda, Anda dapat menjalankannya secara langsung:

```sh
jest test/unit/test.js
```

Atau Anda dapat menjalankan seluruh unit test suite:

```sh
yarn test:unit
```

Sekali lagi, perlu diketahui bahwa Anda mungkin harus membangun kembali fixture Anda sebelumnya!

### Menguji perubahan Anda

Saat mengerjakan PR Anda, Anda mungkin ingin memeriksa apakah fixture Anda diatur dengan benar atau debug perubahan Anda saat ini.

Untuk melakukannya, Anda dapat menggunakan skrip Nuxt itu sendiri untuk meluncurkannya sebagai contoh fixture atau aplikasi:

```sh
yarn nuxt examples/your-app
yarn nuxt test/fixtures/your-fixture-app
```

> `npm link` bisa juga (dan memang, sampai batas tertentu) bekerja untuk ini, tetapi telah diketahui menunjukkan beberapa masalah. Itu sebabnya kami menyarankan untuk memanggil `yarn nuxt` secara langsung untuk menjalankan contoh.

### Contoh

Jika Anda sedang mengerjakan fitur yang lebih besar, harap siapkan aplikasi contoh di `example /`.
Ini akan sangat membantu dalam memahami perubahan dan juga membantu pengguna Nuxt untuk memahami fitur yang Anda buat secara mendalam.

### Linting

Seperti yang mungkin telah Anda perhatikan, kami menggunakan ESLint untuk menegakkan standar kode. Silakan jalankan `yarn lint` sebelum commit
perubahan Anda untuk memverifikasi bahwa style kode sudah benar. Jika tidak, Anda dapat menggunakan `yarn lint --fix` or `npm run lint -- --fix` (jangan salah ketik!) Untuk memperbaiki sebagian besar
perubahan style. Jika masih ada kesalahan yang tersisa, Anda harus memperbaikinya secara manual.

### Dokumentasi

Jika Anda menambahkan fitur baru, atau refactoring atau mengubah perilaku Nuxt dengan cara lain, Anda mungkin
ingin mendokumentasikan perubahan. Silakan lakukan dengan PR ke repositori [dokumentasi](https://github.com/nuxt/docs/pulls).
Anda tidak harus segera menulis dokumentasi (tapi tolong lakukan segera setelah permintaan tarik Anda cukup matang).

### Final checklist

Saat mengirimkan PR Anda, ada template sederhana yang harus Anda isi.
Silakan centang semua "jawaban" yang sesuai dalam daftar periksa.

### Penyelesaian masalah (Troubleshooting)

#### Tes debug pada macOS

Mencari `getPort ()` akan mengungkapkan bahwa itu digunakan untuk memulai proses Nuxt baru selama pengujian. Itu terlihat berhenti bekerja pada macOS dan mungkin mengharuskan Anda untuk secara manual mengatur port untuk pengujian.

Masalah umum lainnya adalah proses Nuxt yang mungkin menggantung (hang) di memori saat menjalankan tes fixture. Proses ghost akan sering mencegah tes berikutnya. Jalankan `ps aux | grep -i node` untuk memeriksa semua proses pengujian yang meng-gantung (hang) jika Anda menduga ini terjadi.
