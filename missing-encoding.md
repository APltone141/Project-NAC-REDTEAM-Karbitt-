🚩 Challenge: Missing Encoding (Juice Shop)
📝 Description
Retrieve the photo of Bjoern's cat in "melee combat-mode" from the Photo Wall.

🔍 Analysis
Masalah utama terletak pada interpretasi karakter khusus dalam URL.

Temuan: Pada halaman Photo Wall, terdapat satu gambar yang gagal dimuat (broken image).

Identifikasi Masalah: Hasil inspeksi elemen menunjukkan URL gambar mengandung karakter # (contoh: assets/public/images/uploads/###.jpg).

Penyebab Root Cause: Dalam standar RFC 3986, karakter # digunakan untuk menandai fragment identifier. Browser memotong URL tepat di karakter tersebut sebelum mengirimkan request ke server. Akibatnya, server menerima path yang tidak lengkap dan mengembalikan error 404.

🛠️ Solution (Step-by-Step)
Identifikasi URL Asli: Temukan URL gambar yang rusak melalui Inspect Element (F12) pada Photo Wall.

URL Encoding: Ubah karakter # menjadi format Percent-Encoding. Karakter # memiliki nilai heksadesimal 23, sehingga kodenya adalah %23.

Substitusi Path: Ganti setiap kemunculan # pada URL tersebut dengan %23.

Sebelum: .../uploads/bjoern_cat#melee.jpg

Sesudah: .../uploads/bjoern_cat%23melee.jpg

Eksekusi: Masukkan URL yang sudah diperbaiki ke address bar browser atau ganti atribut src pada elemen HTML secara langsung.

Hasil: Gambar kucing Bjoern dalam mode tempur akan muncul, dan tantangan terselesaikan.