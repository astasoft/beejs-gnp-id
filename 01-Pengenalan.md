# 1. Pengenalan

Hei! Pemrograman socket membuat kamu frustasi? Apakah topik ini memang sedikit
susah dicari dari sebuah _man page_? Kamu ingin melakukan pemograman Internet
yang keren, tapi kamu tidak ada waktu untuk menyeberangi `struct` demi ingin 
memahami jika kamu harus memanggil `bind()` dulu sebelum `connect()`, dll, dll.

Jadi, coba tebak! Saya sudah pernah mengalami urusan buruk tersebut, dan saya
setengah mati ingin membagikan informasi ini ke semua orang! Kamu datang ke
tempat yang tepat. Dokumen ini harusnya dapat memberikan panduan kepada
programmer C yang berkemampuan rata-rata untuk dapat memulai langkah pada
pada kegaduhan pemograman jaringan ini.

Dan coba lihat: Akhirnya saya dapat mengejar ketertinggalan waktu
(pada saat yang tepat juga!) dan memperbaharui panduan ini untuk untuk IPv6!
Selamat Menikmati!

## 1.1. Pembaca

Dokumen ini ditulis dalam bentuk sebuah panduan, bukan sebuah rujukan lengkap.
Panduan ini mungkin lebih cocok untuk individu yang baru saja belajar tentang
pemograman _socket_ and masih mencari cara untuk memulai. Panduan ini sudah
pasti bukanlah panduan yang lengkap dan menyeluruh untuk pemograman _socket_.

Harapannya, adalah, panduan ini cukup untuk membuat istilah-istilah di
_man page_ menjadi masuk akal... :-)

## 1.2. Platform dan Compiler

Kode yang terdapat pada dokumen ini dikompilasi pada sebuah PC Linux
menggunakan compiler GNU *gcc*. Tetapi harusnya, kode ini dapat dibangun 
pada _platform_ apa saja yang menggunakan *gcc*. Secara alami, ini tidak 
berlaku jika kamu memprogram di Windows--lihat 
[bagian untuk untuk Pemograman Windows](#1-5-catatan-untuk-programmer-windows)
di bawah.

## 1.3. Laman Resmi and Buku untuk Dijual

Laman resmi untuk dokumen ini adalah http://beej.us/guide/bgnet/. Disana
kamu juga akan menemukan contoh kode dan terjemahan untuk panduan ini dalam
berbagai bahasa.

Untuk membeli salinan cetak (sebagian menyebutnya "buku"), kunjungi 
http://beej.us/guide/url/bgbuy. Saya akan mengapresiasi pembelian tersebut
karena membantu saya tetap dapat mempertahankan gaya hidup menulis saya.

## 1.4. Catatan untuk Pemogram Solaris/SunOS

Ketika mengkompilasi untuk Solaris atau SunOS, kamu perlu untuk memberikan
beberapa perintah tambahan untuk menghubungkan pustaka yang tepat. Untuk 
melakukan, cukup tambahkan `-lnsl -lscoket -lresolv` pada akhir perintah
kompilasi, seperti berikut:

```
$ cc -o server server.c -lnsl -lsocket -lresolv
```

Jika kamu masih tetap mendapat kesalahan, kamu dapat mencoba lebih jauh dengan
menambahkan sebuah `-lxnet` pada akhir baris perintah. Saya tidak tahu apa 
artinya tambahan tersebut, tapi beberapa orang tampaknya membutuhkan.

Tempat lain yang kamu mungkin akan menukan masalah adalah dalam pemanggilan ke
`setsockopt()`. Purwarupanya berbeda dengan yang ada pada mesin Linux saya, 
jadi dibanding:

```
int yes=1;
```

Masukkan ini:

```
char yes='1';
```

Karena saya tidak memiliki mesin Sun, saya belum mencoba informasi di atas--
itu hanya yang dikatakan orang kepada saya melalui email.

## 1.5. Catatan untuk Pemrogram Windows

Pada titik ini, secara waktu, saya sudah menghabiskan cukup banyak waktu 
bergulat di Windows, hanya saja saya tidak terlalu menyukainya. Tetapi saya
harus adil dan mengatakan kepada kamu jika Windows memilih basis instalasi 
yang sangat besar dan Windows merupakan sistem operasi yang cukup baik.

Orang mengatakan ketika lama tidak bertemu maka hati jadi rindu, dan dalam
hal ini, saya percaya itu benar. (Atau mungkin karena usia.) Tetapi apa yang
dapat saya katakan adalah setelah lebih dari satu dekade tidak menggunakan
sistem operasi dari Microsoft untuk pekerjaan pribadi saya, saya merasa sangat
senang! Oleh karenanya, saya bisa duduk dengan santai dan bilang, "Tentu, 
silahkan saja menggunakan Windows!"... Ya, itu membuat merapatkan gigi ketika
mengatakan itu.

Jadi saya tetep mendorong kamu untuk tetap mencoba [Linux](www.linux.com), 
[BSD](www.bsd.org) atau varian Unix yang lain.

Tapi orang menyukai apa yang mereka suka, dan kalian pengguna Windows akan 
sangat senang mengetahui bahwa informasi ini secara umum dapat diterapkan
kepada kalian, dengan sedikit perubahan, jika ada.

TBC
