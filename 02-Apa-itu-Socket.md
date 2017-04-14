# 2. Apa itu Socket?

Kamu mendengar pembicaraan tentang "_socket_" sepanjang waktu, dan kamu mungkin
berpikir apakah sebenarnya itu. Jadi, _socket_ itu: adalah sebuah cara untuk
berbicara dengan program-program yang lain menggunakan standar _file descriptor_
Unix.

Apa?

Baiklah-kamu mungkin pernah mendengar beberapa _hacker_ Unix bilang, "Astaga,
semua di Unix adalah sebuah file!" Apa yang orang tersebut bicarakan adalah
dalam kenyataannya ketika sebuah program Unix melakukan kegiatan I/O (baca:
Input Output), mereka melakukannya dengan membaca dan menulis ke sebuah _file
descriptor_. Sebuah _file descriptor_ secara sederhana adalah sebuah angka yang
diasosiasikan pada sebuah _file_ yang terbuka. Tetapi (dan inilah kuncinya),
bahwa _file_ dapat juga berupa sebuah koneksi jaringan, sebuah FIFO, sebuah
_pipe_, sebuah _terminal_, sebuah _file_ nyata pada sebuah media penyimpanan,
atau bisa juga sesuatu yang lain. Segalanya di Unix adalah sebuah _file_! Jadi
ketika kamu ingin berkomunikasi dengan program lainnya melalui Internet kamu
akan melakukannya melalui sebuah _file descriptor_, kamu sebaiknya percaya itu.

"Dimana saya bisa mendapat _file descriptor_ ini untuk komunikasi jaringan,
Tuan Pintar?" mungkin adalah pertanyaan terakhir yang terlintas dipikiran
kamu saat ini, tapi bagaimanapun juga saya akan tetap menjawabnya: Kamu harus
membuat panggilan ke rutin sistem `socket()`. Rutin tersebut mengembalikan
_socket descriptor_, dan kamu dapat berkomunikasi melaluinya menggunakan
panggilan _socket_ khusus `send()` dan `recv()`
([man send](http://beej.us/guide/bgnet/output/html/multipage/sendman.html),
[man recv](http://beej.us/guide/bgnet/output/html/multipage/recvman.html)).

"Hei, tapi!" kamu mungkin sudah mulai protes sekarang. "Jika itu adalah sebuah
_file descriptor_, atas nama Neptune kenapa kita tidak menggunakan panggilan
normal `read()` dan `write()` untuk berkomunikasi melalui _socket_?" Jawaban
singkatnya adalah, "Kamu bisa!", Jawaban panjangnya adalah, "Kamu bisa, tetapi
`send()` dan `recv()` menawarkan lebih banyak fleksibilitas untuk mengontrol
data transmisi kamu."

Apa selanjutnya? Bagaimana jika ini: ada berbagai macam jenis _socket_. Ada
alamat Internet DARPA (Internet Sockets), nama _path_ pada sebuah lokasi lokal
(Unix Sockets), alamat CCITT X.25 (X.25 Sockets yang bisa kamu abaikan), dan
mungkin juga masih banyak lainnya tergantung jenis Unix yang kamu jalankan.
Dokumen ini hanya membahas yang pertama: Internet Sockets.