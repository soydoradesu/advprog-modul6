# Reflection

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