🌐 Konfigurasi Hotspot di MikroTik <br>

Dokumentasi ini menjelaskan langkah-langkah untuk mengaktifkan Hotspot, mengelola user login, dan mengatur limitasi sesi dan profil pengguna pada MikroTik RouterOS.

📶 1. Setup Hotspot
GUI: IP -> Hotspot -> Hotspot Setup

🔹 Langkah-langkah:
Klik tombol "Hotspot Setup".

Hotspot Interface: Pilih interface yang mengarah ke user (misal: ether3).

<pre>Hotspot Interface: ether3</pre>
Local Address of Network: Terisi otomatis, misal 192.168.88.1/24. Klik Next.

Address Pool of Network: Biarkan default (otomatis membuat DHCP server).

Select Certificate: Pilih none (karena tidak menggunakan HTTPS).

IP Address of SMTP Server: Kosongkan.

DNS Servers: Masukkan DNS Google jika perlu:

<pre>8.8.8.8, 8.8.4.4</pre>
DNS Name: Masukkan nama domain lokal untuk halaman login, misal:

<pre>hotspot.local</pre>
Setelah selesai, Hotspot aktif dan siap digunakan.

🍪 2. Manajemen Cookie & Sesi Login
Mencegah user tetap login otomatis saat reconnect.

🔹 Langkah-langkah:
Masuk ke IP -> Hotspot -> Server Profiles.

Double-klik pada profile aktif (biasanya default).

Pergi ke tab Login.

Hilangkan centang "Cookie" agar tidak menyimpan sesi login.

Atau, atur Cookie Lifetime, misalnya:

<pre>00:01:00</pre>
Klik Apply dan OK.

👤 3. Menambahkan User Hotspot Baru
Membuat akun login manual dengan waktu terbatas.

🔹 Langkah-langkah:
Masuk ke IP -> Hotspot -> Users.

Klik tombol + (Add).

Isi detail berikut:

<pre>
Server: hotspot1
Name: Jordi
Password: 123
Profile: default
Limit Uptime: 00:01:00
</pre>
4. Klik Apply lalu OK.

🧪 Uji coba: Hubungkan klien ke jaringan, login pakai user "Jordi". Koneksi akan otomatis logout setelah 1 menit.

🧩 4. Membuat User Profile & Shared Users
Digunakan untuk membuat paket akun Hotspot yang dapat digunakan oleh banyak perangkat.

🔹 Langkah-langkah:
Masuk ke IP -> Hotspot -> User Profiles.

Klik tombol + (Add).

Isi detail:

<pre>
Name: paket1jam
Shared Users: 10
Rate Limit: 2M/2M (opsional untuk batas bandwidth)
</pre>
4. Klik Apply lalu OK.

Saat membuat user baru, pilih profile ini agar:

User bisa dipakai maksimal 10 perangkat secara bersamaan.

Bisa digunakan untuk paket berbasis waktu (misal 1 jam).

✅ Hasil Akhir
Hotspot aktif pada interface LAN.

User dapat login via halaman redirect Hotspot.

Limitasi waktu, sesi login, dan jumlah perangkat dapat diatur.

📝 Tips Tambahan
Gunakan DNS name seperti wifi.local agar halaman login mudah diakses.

Tambahkan branding atau custom login page di menu:
Files -> hotspot -> edit login.html

Gunakan ip hotspot active print untuk melihat user yang sedang terhubung.
