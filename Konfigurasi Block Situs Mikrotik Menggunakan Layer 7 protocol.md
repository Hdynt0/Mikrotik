🔧 Konfigurasi Layer7 Protocol
Menambahkan rule Layer7 untuk mendeteksi trafik yang mengarah ke YouTube atau Google Video.

🔹 Langkah-langkah:
Masuk ke menu: IP > Firewall > tab Layer7 Protocols

Klik tombol "+" (Add)

Isi parameter berikut:

<pre> Name: blok_youtube Regexp: ^.+(youtube.com|googlevideo.com).*$ </pre>
📌 Ekspresi ini mendeteksi URL YouTube dan Google Video dari HTTP/HTTPS request.

🚫 Membuat Filter Rule untuk Memblokir
Menggunakan rule forward untuk mencegah trafik YouTube melewati router.

🔹 Langkah-langkah:
Masuk ke menu: IP > Firewall > tab Filter Rules

Klik tombol "+" (Add)

Tab General:

<pre>
Chain: forward
Protocol: 6 (tcp)
Dst. Port: 80,443
</pre>
4. Tab Advanced:

<pre> Layer7 Protocol: blok_youtube </pre>
Tab Action:

<pre>Action: drop</pre>
6. Klik Apply lalu OK

✅ Hasil Akhir
Trafik menuju situs youtube.com dan googlevideo.com akan diblokir.

Klien yang mencoba mengakses YouTube akan mengalami koneksi gagal.

🧪 Uji Coba
Hubungkan perangkat ke jaringan MikroTik.

Akses https://youtube.com melalui browser.

Jika berhasil diblokir, maka halaman tidak akan muncul atau akan terus loading.
