# Reflection
Nama: Valentino Kim Fernando
NPM : 2306275771

## Milestone 1: Inside `handle_connection`
1. Menerima parameter TcpStream
Method `handle_connection` menerima parameter `TcpStream`, yang digunakan untuk mengelola komunikasi melalui koneksi TCP, baik untuk membaca maupun menulis data.
2. Membuat objek BufReader
Selanjutnya, method ini membuat objek `BufReader` yang membungkus `TcpStream`, yang dilakukan untuk membaca data dari stream dengan cara yang lebih efisien, terutama ketika memproses data yang dikirim secara bertahap.
3. Membaca HTTP Request baris per baris
Method `lines()` digunakan untuk membaca stream satu baris pada satu waktu. `map()` digunakan untuk men-convert hasil pembacaan menjadi sebuah `String`. `take_while()` digunakan sehingga pembacaan dilanjutkan hingga menemukan baris kosong. Seluruh baris kemudian dikumpulkan pada sebuah `Vec<String>` yang dinamakan `http_request`.
4. Menampilkan HTTP Request
Setekah mendapatkan seluruh data HTTP Request, method `handle_conncetion` mencetak isi dari variable `http_request` untuk melihat detail request yang diterima

## Milestone 2: Updated `handle_connection`
1. Menentukan status HTTP Response
Pada tahap awal, method ini mendefinisikan status response dengan pesan "HTTP/1.1 200 OK". Status ini menandakan bahwa permintaan dari klien telah diproses dengan sukses.
Gambar response di web browser:
2. Membaca file HTML ke dalam string
Selanjutnya, method membaca isi file HTML yang bernama "hello.html" ke dalam sebuah string. Untuk memastikan file berhasil dibaca, fungsi `unwrap()` digunakan.
3. Menghitung panjang konten HTML
Setelah itu, panjang konten HTML yang telah dibaca dihitung menggunakan method `len()`. Ini penting karena kita perlu memberitahukan ukuran konten dalam HTTP Response.
4. Menyusun Response Lengkap
Response yang lengkap disusun dengan memformat status line, header `Content-Length`, dan isi konten HTML yang telah dibaca. Format ini mengikuti struktur standar HTTP Response.
5. Mengirim Response ke Client
Akhirnya, respons yang telah disusun dikirim ke klien. Data dikirimkan dalam bentuk byte array setelah dikonversi menggunakan `as_bytes()`, dan respons tersebut diteruskan melalui stream menggunakan `write_all()`.
![Commit 2 screen capture](/assets/images/commit2.png)

## Milestone 3: Updated `handle_connection`
1. Memeriksa request line
Method sekarang memeriksa baris pertama dari HTTP request (request line). Server akan membandingkan apakah request line tersebut adalah GET / HTTP/1.1. Jika cocok, server akan merespons dengan konten dari hello.html. Jika tidak, server akan mengembalikan status 404 dengan halaman 404.html.
2. Refactor untuk pemisahan logic
Sebelumnya server mengirimkan halaman yang sama untuk semua request. Kini, dengan memisahkan logic response berdasarkan jenis request, server mampu memberikan respons yang sesuai. Hal ini meningkatkan kemampuan server untuk mengkomunikasikan status yang lebih jelas kepada klien.
![Commit 3 screen capture](/assets/images/commit3.png)

## Milestone 4: Simulation response lambat
1. Mengamati dampaknya pada server satu thread
Ketika URL `/sleep` diakses, browser akan membutuhkan waktu lebih lama untuk memuat halaman tersebut. Sementara itu, saat saya membuka window kedua dan mengakses /, response tersebut harus menunggu hingga request pertama selesai diproses sebelum dapat meresponse.
2. Pentingnya mempertimbangkan concurrency dalam server
Saya menyadari pentingnya design server yang dapat menangani beberapa request secara bersamaan (concurrent requests). Server single thread hanya dapat menangani satu request pada satu waktu, yang dapat menyebabkan penundaan jika ada request yang memakan waktu lama.

## Milestone 5: Implementasi Threadpool
1. Perbandingan dengan server single thread
Dengan adanya thread pool, server kini dapat menangani request secara sekaligus, yang sangat mengurangi masalah bottleneck yang terjadi pada pendekatan single thread. Perbedaan ini sangat terasa ketika mengakses /sleep dan / secara bersamaan, di mana server dapat merespons lebih cepat dan efisien.

## (Bonus) Function Improvement
1. Memisahkan logic pembuatan ThreadPool ke dalam function terpisah
Perubahan ini memisahkan logic pembuatan ThreadPool ke dalam sebuah function khusus. Meskipun perubahan ini terlihat kecil, hal ini meningkatkan fleksibilitas kode. Dengan memisahkan logic ini, kamu dapat dengan mudah mengubah ukuran atau konfigurasi thread pool hanya di satu tempat, tanpa perlu memodifikasi kode di beberapa bagian, yang sejalan dengan prinsip penulisan kode yang bersih dan modular.