---
layout: post
title:  "Instalasi dan Konfigurasi DNScrypt Proxy di Linuxmint"
author: tap
categories: [ Logs, Indonesian ]
tags: [ Linuxmint, DNScrypt Proxy ]
image: s/images/dnscrypt-proxy.webp
#beforetoc: "It was a fresh install but WPS can not export to PDF"
#toc: true
---

Kelanjutan dari instalasi ulang Linuxmint 22 Xfce di laptop saya, seperti disebut artikel sebelumnya yang berbahasa Inggris, ternyata saya lupa memasang DNScrypt Proxy. Artikel ini sengaja ditulis dalam Bahasa Indonesia karena mungkin dialami akibat berada di Indonesia, karena **pemblokiran situs oleh pemerintah!** 

Tidak jarang pemblokiran juga dilakukan pada situs yang bersifat teknis seperti yang menggunakan domain netlify.app, selain tentu saja yang aneh adalah mesin pencari DuckDuckGo.com.

Kasarnya dengan **DNScrypt Proxy** ini kita menggunakan DNS luar secara proxy atau me*relay* DNS luar dimana koneksi ke DNS tersebut menggunakan sambungan tersandi/terenkripsi yang tidak bisa disusupi oleh pihak lain, ISP misalnya. Karena pemblokiran yang dilakukan adalah dengan menutupi atau mengaburkan DNS-nya (yang dikontrol pemerintah dan ISP) maka dengan DNScrypt proxy ini kita tidak lagi melewati penyaringan yang dilakukan, dan akses ke situs yang diblokir tersebut kembali tersedia. Alur kerjanya mungkin bisa dilihat pada gambar di atas.

Tidak seperti menggunakan VPN dimana kita menyamarkan alamat IP kita, dengan DNScrypt justru menghilangkan samaran atau saringan DNS yang dilakukan ISP/pemerintah.

Versi baru Linuxmint (juga yang berbasis Ubuntu) sudah menyertakan DNScrypt Proxy dalam repositori resminya. Berikut ini adalah panduan instalasinya dalam Linuxmint 22.

### **1. Instalasi DNSCrypt Proxy**
Jalankan perintah berikut:

```bash
sudo apt update
sudo apt install dnscrypt-proxy
```
### **2. Konfigurasi DNSCrypt Proxy**
Konfigurasi bawaannya disimpan dalam berkas `/etc/dnscrypt-proxy/dnscrypt-proxy.toml`.

1. Buka berkas konfigurasi:
   ```bash
   sudo nano /etc/dnscrypt-proxy/dnscrypt-proxy.toml
   ```

2. Edit pengaturan di bawah ini jika dibutuhkan:
   - **Pilih resolvers**: *Uncomment* dan ganti parameter `server_names` dengan resolver DNS yang ingin digunakan. Misalnya:
     ```toml
     server_names = ['cloudflare', 'google']
     ```
     Pada kasus di laptop saya, hanya menggunakan satu
     ```toml
     server_names = ['cloudflare']
     ```
   - **Listen locally**: Hanya menggunakan localhost pada alamat `listen`:
     ```toml
     listen_addresses = ['127.0.2.1:53']
     ```

3. Simpan (`Ctrl+O`, `Enter`, `Ctrl+X`).
  Dan berikut ini adalah contoh pada laptop saya
  ```toml
  # Empty listen_addresses to use systemd socket activation
  listen_addresses = ['127.0.2.1:53']
  server_names = ['cloudflare']
  
  [query_log]
  file = '/var/log/dnscrypt-proxy/query.log'
  
  [nx_log]
  file = '/var/log/dnscrypt-proxy/nx.log'
  
  [sources]
  [sources.'public-resolvers']
  url = 'https://download.dnscrypt.info/resolvers-list/v2/public-resolvers.md'
  cache_file = '/var/cache/dnscrypt-proxy/public-resolvers.md'
  minisign_key = 'RWQf6LRCGA9i53mlYecO4IzT51TGPpvWucNSCh1CBM0QTaLn73Y7GFO3'
  refresh_delay = 72
  prefix = ''

  ```

### **3. Menyetel DNSCrypt Proxy sebagai Resolver Sistem**

Ini dilakukan dengan mengubah pengaturan Network Connection, yang pada kasus laptop saya menggunakan koneksi WiFi ke tappoint.

![Netconf](https://static.tapsaja.eu.org/s/images/con-setting.png)

Klik tab `IPv4 Settings` dan pada bagian `Method` pilih **Automatic (DHCP) addresses only**, lalu di bagian `DNS servers` isi dengan **127.0.2.1** (seperti yang disetelah pada bagian `listen` di atas)

Lalu jalankan ulang NetworkManager
```bash
sudo systemctl restart NetworkManager
```

### **4. Aktifkan dan Mulai DNSCrypt Proxy**

Sebenarnya pada versi akhir Linuxmint (atau yang berbasis Ubuntu lainnya) saat menginstall aplikasi yang mempunyai service biasanya sudah otomatis diaktifkan dan dijalankan, tetapi untuk memastikannya kita coba saja.
- Aktifkan service untuk dimulai otomatis saat komputer dijalankan (boot)
  ```bash
  sudo systemctl enable dnscrypt-proxy
  ```
- Mulai jalankan service
  ```bash
  sudo systemctl start dnscrypt-proxy
  ```
- Periksa statusnya
  ```bash
  sudo systemctl status dnscrypt-proxy
  ```
  ![Status](https://static.tapsaja.eu.org/s/images/dnscrypt-proxy-status.png)

### **5. Verifikasi Hasil**
- Pengujian resolusi DNS lewat DNScrypt-proxy
  ```bash
  dig @127.0.2.1 example.com
  ```
- Pengujian kueri DNS tersandi/terenkripsi 
  ```bash
  dnscrypt-proxy -resolve example.com -config /etc/dnscrypt-proxy/dnscrypt-proxy.toml
  ```
  ![Encrypted](https://static.tapsaja.eu.org/s/images/dnscrypt-proxy-resolve.png)
- Lakukan pengujian DNS leak (mis., [dnsleaktest.com](https://dnsleaktest.com)) untuk memastikan tidak ada kebocoran.
- Kunjungi situs yang sebelumnya tidak bisa diakses
  ![contoh saja](https://static.tapsaja.eu.org/s/images/reddit.webp)

### Catatan
- Konfigurasi `listen_addresses` bawaan pada Linux Mint menggunakan `127.0.2.1:53` bukannya `127.0.0.1:53` untuk menghindari konflik dengan resolver DNS lainnya. Pastikan pada tahap nomor 3 di atas sudah sesuai.
- Jika ingin melakukan tweak lebih lanjut, silakan pelajari `/etc/dnscrypt-proxy/dnscrypt-resolvers.csv` untuk melihat daftar resolver yang tersedia.