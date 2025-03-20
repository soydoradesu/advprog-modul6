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