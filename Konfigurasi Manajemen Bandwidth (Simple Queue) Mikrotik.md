 Konfigurasi Manajemen Bandwidth (Simple Queue) di Mikrotik
🧾 Tujuan
Membatasi kecepatan upload dan download untuk satu atau beberapa IP dalam jaringan menggunakan fitur Simple Queue pada Mikrotik, baik untuk single IP, network, atau beberapa IP sekaligus.

🔧 Konfigurasi 1: Simple Queue - Single IP
Langkah-langkah:
Masuk ke Winbox, login ke Mikrotik.

Buka menu Queues.

Pada tab Simple Queues, klik + untuk menambahkan rule.

Di tab General, isi sebagai berikut:

<pre> Name: Simple XP Target Address: 10.10.10.2 Max Limit: 512k/512k (Download/Upload) </pre>
Klik Apply dan OK.

Lakukan speed test pada klien dengan IP 10.10.10.2 → seharusnya kecepatan maksimal sekitar 0.49 Mbps.

⚠️ Catatan: Jika klien mengubah IP-nya secara manual, maka pembatasan bandwidth tidak berlaku.

🔧 Konfigurasi 2: Simple Queue - Network IP
Langkah-langkah:
Nonaktifkan atau hapus rule sebelumnya.

Tambah rule baru di Simple Queues.

Di tab General, isi sebagai berikut:

<pre> Name: Network Bandwidth Target Address: 10.10.10.0/24 Max Limit: 512k/512k </pre>
Klik Apply dan OK.

Lakukan speed test dari klien manapun dalam jaringan 10.10.10.0/24 (contoh: 10.10.10.10, 10.10.10.120) → semua akan terbatasi sesuai limit.

✅ Cocok untuk memastikan semua klien dalam subnet dibatasi, meskipun IP-nya berubah.

🔧 Konfigurasi 3: Simple Queue - Beberapa IP
Langkah-langkah:
Nonaktifkan atau hapus rule sebelumnya.

Tambah rule baru.

Di tab General, isi:

<pre> Name: Beberapa IP Target Address: - 10.10.10.2 - 10.10.10.100 (tambahkan via tombol panah ke bawah) Max Limit: 512k/512k </pre>
Klik Apply dan OK.

Lakukan speed test:

IP 10.10.10.2 dan 10.10.10.100 → dibatasi.

IP lain (contoh 10.10.10.10) → tidak dibatasi, mendapat kecepatan penuh.

📝 Catatan Tambahan
Max Limit menentukan batas maksimum bandwidth.

Jika ingin lebih fleksibel, pertimbangkan juga menggunakan fitur PCQ (Per Connection Queue) atau Queue Tree.

Simple Queue sangat cocok untuk kebutuhan dasar dan manajemen bandwidth ringan.
