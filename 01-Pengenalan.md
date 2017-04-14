# 1. Pengenalan

Hei! Pemrograman _socket_ membuat kamu frustasi? Apakah topik ini memang sedikit
susah dicari dari sebuah _man page_? Kamu ingin melakukan pemrograman Internet
yang keren, tapi kamu tidak ada waktu untuk menyeberangi `struct` demi ingin
memahami jika kamu harus memanggil `bind()` dulu sebelum `connect()`, dll, dll.

Jadi, coba tebak! Saya sudah pernah mengalami urusan buruk tersebut, dan saya
setengah mati ingin membagikan informasi ini ke semua orang! Kamu datang ke
tempat yang tepat. Dokumen ini harusnya dapat memberikan panduan kepada
programmer C yang berkemampuan rata-rata untuk dapat memulai langkah pada
kegaduhan pemograman jaringan ini.

Dan coba lihat: Akhirnya saya dapat mengejar ketertinggalan waktu
(pada saat yang tepat juga!) dan memperbaharui panduan ini untuk untuk IPv6!
Selamat Menikmati!

## 1.1. Pembaca

Dokumen ini ditulis dalam bentuk sebuah panduan, bukan sebuah rujukan lengkap.
Panduan ini mungkin lebih cocok untuk individu yang baru saja belajar tentang
pemograman _socket_ and masih mencari cara untuk memulai. Panduan ini sudah
pasti bukanlah panduan yang lengkap dan menyeluruh untuk pemograman _socket_.

Harapannya, adalah, panduan ini cukup untuk membuat istilah-istilah di
_man page_ menjadi mulai masuk akal... :-)

## 1.2. Platform dan Compiler

Kode yang terdapat pada dokumen ini dikompilasi pada sebuah PC Linux
menggunakan compiler GNU *gcc*. Tetapi harusnya, kode ini dapat dibangun
pada _platform_ apa saja yang menggunakan *gcc*. Namun secara umum, ini tidak
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
melakukannya, cukup tambahkan `-lnsl -lscoket -lresolv` pada akhir perintah
kompilasi, seperti berikut:

```
$ cc -o server server.c -lnsl -lsocket -lresolv
```

Jika kamu masih tetap mendapat kesalahan, kamu dapat mencoba lebih jauh dengan
menambahkan sebuah `-lxnet` pada akhir baris perintah. Saya tidak tahu apa
artinya tambahan tersebut, tapi beberapa orang tampaknya membutuhkan.

Tempat lain yang kamu mungkin akan menemukan masalah adalah dalam pemanggilan
ke `setsockopt()`. Purwarupanya berbeda dengan yang ada pada mesin Linux saya,
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
harus adil dan mengatakan kepada kamu jika Windows memiliki basis instalasi
yang sangat besar dan Windows merupakan sistem operasi yang cukup baik.

Orang mengatakan ketika lama tidak bertemu maka hati jadi rindu, dan dalam
hal ini, saya percaya itu benar. (Atau mungkin karena usia.) Tetapi apa yang
dapat saya katakan adalah setelah lebih dari satu dekade tidak menggunakan
sistem operasi dari Microsoft untuk pekerjaan pribadi saya, saya merasa sangat
senang! Oleh karenanya, saya bisa duduk dengan santai dan bilang, "Tentu,
silahkan saja menggunakan Windows!"... Ya, itu membuat saya merapatkan gigi
ketika mengatakan itu.

Jadi saya tetep mendorong kamu untuk tetap mencoba [Linux](www.linux.com),
[BSD](www.bsd.org) atau varian Unix yang lain.

Tapi orang menyukai apa yang mereka suka, dan kalian pengguna Windows akan
sangat senang mengetahui bahwa informasi ini secara umum dapat diterapkan
kepada kalian, dengan sedikit perubahan, jika ada.

Satu hal keren yang bisa kamu lakukan adalah menginstal
[Cygwin](http://www.cygwin.com/), yang merupakan sebuah koleksi dari
perangkat-perangkat Unix untuk Windows. Saya pernah mendengar kabar jika
menginstal Cygwin dapat membuat semua program ini dikompilasi tanpa
modifikasi.

Tapi beberapa dari kalian mungkin ingin melakukannya murni dengan Cara Windows.
Kamu sangat berani kalau begitu, dan inilah yang harus kamu lakukan:
keluar dan segara dapatkan Unix sekarang juga! Tidak, tidak--saya hanya
bercanda. Saya seharusnya menjadi ramah ke Windows sekarang...

Ini yang harus kamu lakukan (kecuali kamu menginstal
[Cygwin](http://www.cygwin.com/)!): pertama, abaikan sebagian besar _file_
sistem _header_ yang saya sebutkan disini. Hal yang kamu perlukan adalah
memasukkan:

```
#include <winsock.h>
```

Tunggu! kamu juga harus memanggil `WSAStartup()` sebelum melakukan apapun
dengan pustaka _socket_. Kode untuk melakukan itu akan terlihat seperti
berikut:

```
#include <winsock.h>

{
    WSADATA wsaData;   // jika ini tidak bekerja
    //WSAData wsaData; // coba yang ini

    // MAKEWORD(1,1) for Winsock 1.1, MAKEWORD(2,0) for Winsock 2.0:

    if (WSAStartup(MAKEWORD(1,1), &wsaData) != 0) {
        fprintf(stderr, "WSAStartup failed.\n");
        exit(1);
    }
```

Kamu juga harus memberitahu _compiler_ kamu untuk menghubungkan ke pustaka
Winsock, biasanya bernama `wsock32.lib` atau `winsock32.lib,`, atau
`ws2_32.lib` untuk Winsock 2.0. Pada VC++, ini bisa dilakukan melalui menu
`Project`, dibawah `Setting...` Klik tab `Link`, dan lihat pada kotak yang
bertuliskan "Object/library modules". Tambahkan "wsock32.lib" (atau
apapun pustaka yang menjadi preferensimu) ke dalam daftar.

Jadi saya mendengarkan.

Akhirnya, kamu perlu memanggil `WSACleanup()` ketika selesai dengan pustaka
_socket_. Lihat bantuan _online_ milik _compiler_mu untuk lebih lanjut.

Ketika kamu sudah melakukan itu, sisa dari contoh-contoh yang ada di panduan
ini secara umum dapat diterapkan, dengan sedikit pengecualian. Satu hal lagi,
kamu tidak bisa menggunakan `close()` untuk menutup _socket_--kamu perlu untuk
menggunakan `closesocket()`. Dan lagi, `select()` hanya dapat bekerja dengan
_socket descriptor_, bukan _file descriptor_ (seperti `0` untuk `stdin`).

Terdapat juga sebuah kelas _socket_ yang dapat kamu gunakan, `CSocket`. Periksa
halaman bantuan pengompilasi milikmu untuk informasi lebih lanjut.

Untuk mendapatkan informasi tentang Winsock, baca
[Winsock FAQ](http://tangentsoft.net/wskfaq/) dan mulai dari sana.

Terkahir, saya mendengar bahwa Windows tidak memiliki pemanggil sistem `fork()`
yang merupakan, sayangnya, digunakan pada beberapa contoh saya. Mungkin kamu
harus menghubungkan pada sebuah pustaka POSIX atau semacam itu untuk membuatnya
dapat berjalan, atau kamu dapat menggunakan `CreateProcess()`. `fork()` tidak
perlu argumen, dan `CreateProcess()` memerlukan sekitar 48 milyar argumen.
Jika kamu tidak suka itu, `CreateThread()` lebih mudah untuk digunakan...tapi
sayangnya diskusi tentang _multithreading_ diluar cakupan dokumen ini. Saya
hanya bicara sampai disitu, kamu tahukan!

## 1.6. Kebijakan Email

Secara umum saya bersedia membantu menjawab pertanyaan-pertanyaan lewat email
jadi jangan ragu untuk bertanya, tapi saya tidak menjamin akan membalas. Saya
memiliki kegiatan yang padat dan terkadang saya tidak memiliki waktu untuk
pertanyaan yang kamu kirim. Ketika itu terjadi, saya biasanya menghapus pesan
tersebut. Jangan diambil hati; Saya pasti tidak memiliki waktu untuk memberikan
jawaban secara detail seperti yang kamu butuhkan.

Sebagai aturan, semakin rumit pertanyaan, maka semakin kecil peluangnya untuk
saya respon. Jika kamu bisa memperinci pertanyaanmu sebelum mengirimkannya dan
pastikan untuk menyertakan semua informasi terkait (seperti _platform_,
_compiler_, pesan error yang didapat, dan lainnya yang kamu pikir dapat
membantu saya untuk memecahkan masalahnya), maka kamu lebih berkesempatan
untuk mendapat respon. Untuk petunjuk lainnya, silahkan baca dokumen ESR,
[Bagaimana Bertanya dengan Cara yang Pintar](http://www.catb.org/~esr/faqs/smart-questions.html).

Jika kamu tidak mendapat respon, teliti lebih dalam lagi, untuk menemukan
jawabannya, dan jika masih sulit dipahami, maka kirimkan lagi ke saya dengan
informasi yang baru saja kamu temukan dan semoga itu cukup untuk dapat
membuat saya membantu kamu.

Sekarang saya sudah mendesak kepada kamu tentang bagaimana cara menulis dan
bagaimana cara untuk tidak menulis ke saya, saya hanya ingin kamu tahu bahwa
saya _sangat_ menghargai semua pujian untuk panduan ini yang saya dapat selama
beberapa tahun. Itu benar-benar mengangkat moral saya, dan menyenangkan bagi
saya ketika mendengar bahwa itu digunakan untuk tujuan yang baik Terima Kasih!

## 1.7. Duplikasi

Kamu diperbolehkan untuk menduplikasi situs ini, apakah secara publik atau
secara privat. Jika kamu menduplikasi situs ini dan menginginkan saya untuk
menaruh tautan pada halaman utama, kirim saja informasinya ke beej@beej.us.

## 1.8. Catatan untuk Penerjemah

Jika kamu ingin menerjemahkan panduan ini kedalam bahasa lain, tulis email ke
saya di beej@beej.us dan saya akan memasukkan tautan ke hasil terjemahan kamu
dari halaman utama. Kamu dipersilahkan untuk menambahkan nama kamu dan
informasi kontak pada hasil terjemahan.

Mohon untuk diperhatikan batasan-batasan lisensi pada bagian Hak Cipta dan
Distribusi di bawah.

Jika kamu ingin saya untuk menempatkan hasil terjemahan kamu, cukup tanya saja.
Saya juga akan mentautkan ke tempat kamu jika kamu ingin memasang sendiri
hasil terjemahannya; keduanya tidak masalah.

## 1.9. Hak Cipta dan Distribusi

Beej's Guide to Network Programming adalah Hak Cipta © 2015 dari Brian "Beej
Jorgensen" Hall.

Dengen pengecualian spesifik untuk sumber kode dan hasil terjemahan, di bawah,
hasil kerja ini dilisensikan dibawah lisensi Creative Commons Attribution-
Noncommercial- No Derivative Works 3.0. Untuk melihat salinan dari lisensi ini,
kunjungi http://creativecommons.org/licenses/by-nc-nd/3.0/ atau kirim sebuah
surat ke Creative Commons, 171 Second Street, Suite 300, San Fransisco,
California, 94105, USA.

Satu pengecualian khusus untuk lisensi bagian "No Derivative Works" (baca: Tidak
boleh ada hasil karya turunan) adalah sebagai berikut: panduan ini boleh
diterjemahkan kedalam berbagai bahasa, hasil terjemahan haruslah akurat, dan
panduan ini dicetak seluruhnya dalam terjemahan tersebut. Lisensi yang sama
diterapkan pada terjemahan seperti halnya panduan asli. Hasil terjemahan dapat
memuat nama dan informai kontak dari penerjemah.

Sumber kode untuk C pada dokumen ini ditujukan untuk publik, dan oleh karenanya
bebas dari batasan-batasan lisensi.

Para pendidik dapat secara bebas merekomendasikan atau memberikan salinan dari
panduan ini untuk anak didik mereka.

Hubungi beej@beej.us untuk informasi lebih lanjut.