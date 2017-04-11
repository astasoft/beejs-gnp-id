# 1. Pengenalan

Hei! Pemrograman socket membuat kamu frustasi? Apakah topik ini memang sedikit
susah dicari dari sebuah _man page_? Kamu ingin melakukan pemograman Internet
yang keren, tapi kamu tidak ada waktu untuk menyelami sebuah `struct` demi
ingin memahami jika kamu harus memanggil `bind()` dulu sebelum `connect()`,
dll, dll.

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

Laman official untuk dokumen ini adalah http://beej.us/guide/bgnet/. Disana
kamu juga akan menemukan contoh kode dan terjemahan untuk panduan ini dalam
berbagai bahasa.

Untuk membeli salinan cetak (sebagian menyebutnya "buku"), kunjungi 
http://beej.us/guide/url/bgbuy. Saya akan mengapresiasi pembelian tersebut
karena membantu saya tetap dapat mempertahankan gaya hidup menulis saya.
