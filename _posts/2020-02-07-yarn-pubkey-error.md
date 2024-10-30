---
layout: post
title:  "Yarn Pubkey Error"
author: tap
categories: [ Logs, Indonesian ]
tags: [ ElementaryOS, Ubuntu, Pubkey, Yarn ]
image: assets/images/yarn-error.webp
beforetoc: "Sistem update software di laptopku tiba-tiba error (minor) dengan pesan seperti pada gambar tangkapan layar di atas"
toc: true
---
Sebenarnya bukan masalah kritis, tapi cukup mengganggu saja dengan tampilan yang tidak bersih seperti ini.


#### Solusi

Jalanin

```shell-session
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
```

atau

```shell-session
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 23E7166788B63E1E
```

#### Detail

Masalah ini terjadi karena kunci sertifikat unutk menandatangani repositori (penyimpanan) update si yarn ini sudah kedaluarsa (berakhir sekitar tiga hari lalu). Karena dianggap kadaluarsa maka kunci tersebut tidak lagi dipercaya sehingga pembaruan - jika ada - dalam repositorinya akan diabaikan. Solusinya adalah dengan mendapatkan lagi kunci tanda tangan yang baru (dengan curl) dan didaftarkan dalam sistem update di komputer kita (dengan apt-key), seperti yang dilakukan oleh perintah di atas.

Ternyata masalah yang sama pernah muncul [taun maren](https://github.com/yarnpkg/yarn/issues/6865#issuecomment-450789338 "Issues @ Github"). Kayaknya maintainernya rada males yah... :) Kita liat apa taun depan masih berulang? Mudah-mudahan aja nggak.