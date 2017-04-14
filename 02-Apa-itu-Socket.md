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

## 2.1. Dua Jenis dari Internet Socket

Apa ini? Ada dua jenis dari Internet Socket? Ya. Hmm, tidak. Saya bohong.
Terdapat lebih banyak, tapi saya tidak ingin menakuti kalian. Saya hanya
akan membicarakan tentang dua jenis di sini. Kecuali untuk kalimat ini, dimana
saya akan memberitahhukan kepada kamu bahwa "Raw Socket" juga sangat berguna
dan kamu sebaiknya melihatnya.

Baiklah. Apa dua jenis _socket_ tersebut? Satu adalah "Stream Sockets"; lainnya
adalah "Datagram Sockets", yang setelah ini dirujuk sebagai "SOCK_STREAM" dan
"SOCK_DGRAM". _Socket_ datagram kadang disebut sebagai "_socket_ yang tidak
terkoneksi" (Baca: _connectionless_). (Tetapi mereka juga bisa
di-`connect()`-kan jika kamu benar-benar mau. Lihat `connect()`, di bawah.)

_Stream Socket_ merupakan aliran komunikasi dua arah yang handal. Jika kamu
mengeluarkan dua item dalam socket dengan urutan "1, 2", mereka akan tiba
dengan urutan "1, 2" pada sisi penerima. Mereka juga akan bebas dari _error_.
Saya sangat yakin, bahwa, mereka akan bebas dari _error_, tapi saya hanya
akan meletakkan jari saya ditelinga dan bernyanyi la la la la la jika ada
orang yang mencoba mengklaim sebaliknya.

Apa yang menggunakan _stream socket_? Baiklah, kamu mungkin pernah mendengar
aplikasi telnet, pernah? Itu menggunakan _stream socket_. Semua karakter yang
kamu ketikkan harus tiba dengan urutan yang sama ketika kamu mengetik, benar?
Juga, _web browser_ menggunakan protokol HTTP dimana itu menggunakan _stream
socket_ untuk mendapatkan halaman. Bahkan, jika kamu _telnet_ pada sebuah
website pada port 80, dan mengetikkan "GET / HTTP/1.0" dan menekan ENTER dua
kali, itu akan mengeluarkan semua HTML kembali kepada kamu.

Bagaimana _stream socket_ dapat mencapai kualitas data transisi yang begitu
tinggi? Mereka menggunakan sebuah protokol yang disebut "The Transmission
Control Protocol", atau yang dikenal dengan sebutan "TCP" (lihat
[RFC 793](http://tools.ietf.org/html/rfc793) untuk keterangan yang lebih
detail tentang TCP.) TCP membuat data kamu datang secara berurutan dan bebas
_error_. Kamu mungkin pernah mendengar "TCP" sebelumnya sebagai bagian juga
dari "TCP/IP" dimana "IP" singkatan dari "Internet Protocol" (lihat
[RFC 791](http://tools.ietf.org/html/rfc791).) Tugas utama IP berhubungan
dengan _routing_ Internet dan secara umum tidak bertanggung jawab untuk
integritas data.

Keren. Bagaimana tentang Datagram _socket_? Kenapa mereka disebut
_connectionless_? Apa yang menyebabkannya? Kenapa mereka tidak handal? Baiklah,
ini adalah beberapa faktanya: jika kamu mengirim sebuah datagram, paketnya
mungkin sampai. Paketnya mungkin sampai tidak berurutan. Jika sampai, data di
dalam paket akan bebas dari _error_.

_Datagram socket_ juga menggunakan IP untuk _routing_, tapi mereka tidak
menggunakan TCP; mereka menggunakan "User Datagram Protocol", atau "UDP" (lihat
[RFC 768](http://tools.ietf.org/html/rfc768).)

Kenapa mereka tidak terkoneksi (_connectionless_? Baiklah, secara sederhana,
itu karena kamu tidak perlu menjaga koneksi terbuka seperti yang kamu lakukan
dengan _stream socket_. Kamu hanya perlu membuat sebuah paket, menambahkan
sebuah _header_ IP didalamnya dengan informasi tujuan, dan mengirimkannya.
Koneksi tidak diperlukan. Mereka digunakan ketika sebuah susunan TCP tidak
tersedia atau ketika beberapa paket hilang di sini dan di situ tidak berarti
kiamat untuk alamat semesta. Contoh aplikasi: tftp (trivial file transfer
protocol, saudara dekat dari FTP), dhcpd (sebuah klien DHCP), game multiplayer,
audio _streaming_, konferensi video, dan lainnya.

"Tunggu dulu! tftp dan dhcpd digunakan untuk mengirimkan binari aplikasi dari
satu _host_ ke lainnya! Data tidak boleh hilang ketika sampai jika kamu berharap
aplikasi untuk bekerja! Ilmu hitam macam apa ini?"

Baiklah, teman manusiaku, tftp dan program sejenis memiliki protokol mereka
sendiri di atas UDP. Sebagai contoh, protokol tftp mengatakan untuk setiap
paket yang dikirim, penerima harus mengirim balik sebuah paket yang mengatakan,
"Saya menerimanya!" (sebuah paket "ACK".) Jika pengirim dari paket asli tidak
mendapat balasan, katakanlah, lima detik, dia akan memgirim ulang paketnya
sampai dia mendapat balasan ACK. Prosedur pemberitahuan ini sangat peting
ketika mengimplementasikan aplikasi SOCK_DGRAM yang handal.

Untuk aplikasi yang tidak memerlukan kehandalan seperti game, audio atau
video, kamu dapat mengabaikan paket-paket yang hilang, atau mungkin mencoba
untuk mengkompensasikannya. (Pemain Quake akan mengetahui manifestasi efek
ini dengan istilah teknis: _lag_ terkutuk). Kata "terkutuk", dalam hal ini,
mewakili semua kata-kata kotor yang terucap.)

Kenapa kamu menggunakan protokol yang tidak handal? Dua alasan: kecepatan dan
kecepatan. Lebih sangat cepat untuk kirim-dan-lupakan daripada harus mengawasi
apakah paket datang dengan aman dan memastikan bahwa paketnya berurutan dan
sebagainya. Jika kamu mengirimkan sebuah pesan _chat_, TCP sangat bagus; jika
mengirimkan 40 update posisi per detik dari semua pemain di seluruh dunia,
mungkin tidak begitu masalah jika satu atau dua paket hilang, dan UDP merupakan
pilihan yang bagus.