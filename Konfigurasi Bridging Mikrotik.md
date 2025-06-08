🔗 Konfigurasi Bridging MikroTik
Bridging memungkinkan beberapa interface pada router MikroTik bekerja seolah-olah berada dalam jaringan yang sama. Dengan konfigurasi ini, klien yang terhubung melalui router berbeda dapat saling berkomunikasi dan mengakses internet dari satu sumber (Router 1).

🎯 Tujuan Akhir
Klien 1 dan Klien 2 berada dalam satu segmen IP (10.10.10.0/24).

Keduanya dapat ping satu sama lain.

Klien 2 dapat mengakses internet melalui MikroTik 1.

🖼️ Topologi Jaringan
<center><img src="https://drive.google.com/uc?export=view&id=1S6o_MD0Pv0Gh_D-Iv92KkOj277sDp3cI" width="700"></center>

🛠️ Konfigurasi MikroTik 1 (Router 1)
🔹 Langkah-langkah:
<pre> /interface set ether3 name=Bridge-Router2 /ip address add address=10.10.10.2/24 interface=Bridge-Router2 </pre>
🖥️ Konfigurasi MikroTik 2 (Router 2)
🔸 Setting Manual IP di Klien 2 (XP2)
<pre> IP: 10.10.10.200 Gateway: 10.10.10.4 DNS: 10.10.10.4 </pre>
🔹 Login MikroTik via MAC Address
Karena Router 2 belum punya IP, login via Winbox menggunakan MAC.

🔹 Ubah Nama Interface
<pre> /interface set ether1 name=Bridge-ke-R1 /interface set ether2 name=Lokal2 </pre>
🔹 Tambah IP Address
<pre> /ip address add address=10.10.10.3/24 interface=Bridge-ke-R1 /ip address add address=10.10.10.4/24 interface=Lokal2 </pre>
🔹 Tes Ping
Ping dari Router 2 ke Router 1 (10.10.10.2): ✅ berhasil

Ping dari Router 2 ke Klien 1 (10.10.10.254): ❌ gagal (belum bridging)

🔗 Konfigurasi Bridging di MikroTik 2
Masuk ke menu Bridge

Klik tombol "+", beri nama Bridge2, lalu Apply & OK

Masuk ke tab Ports:

Tambahkan Bridge-ke-R1 ke Bridge2

Tambahkan Lokal2 ke Bridge2

🔹 Status "R" (Running) akan muncul
Tes ping dari Klien 2 ke MikroTik 1 atau internet: ✅ berhasil

🔗 Konfigurasi Bridging di MikroTik 1
Masuk ke menu Bridge

Klik tombol "+", beri nama bridge1, lalu Apply & OK

Masuk ke tab Ports:

Tambahkan Bridge-Router2 ke bridge1

Tambahkan interface lokal (ke Klien 1) ke bridge1

🔹 Tes Ping:
Klien 1 ke Klien 2: ✅ berhasil

Klien 2 ke Klien 1: ✅ berhasil

🌐 Konfigurasi Akses Internet MikroTik 2 & Klien 2
🔹 Tambah NAT di MikroTik 2
<pre> /ip firewall nat add chain=srcnat out-interface=Bridge2 action=masquerade </pre>
🔹 Setting DNS
<pre> /ip dns set servers=8.8.8.8,8.8.4.4 allow-remote-requests=yes </pre>
🔹 Tambah Default Route
<pre> /ip route add dst-address=0.0.0.0/0 gateway=10.10.10.2 </pre>
🔹 Uji Coba:
Ping dari MikroTik 2 ke google.com: ✅ berhasil

Ping dari Klien 2 ke google.com: ✅ berhasil

Akses YouTube dari Klien 2: ✅ berhasil (jika tidak diblokir di MikroTik 1)

📌 Catatan
Jika YouTube tidak bisa dibuka, kemungkinan ada rule firewall di MikroTik 1.

Disable sementara rule tersebut untuk menguji koneksi sepenuhnya.

✅ Kesimpulan
Bridging berhasil membuat jaringan seolah-olah satu switch.

Klien 1 & Klien 2 dapat saling terhubung tanpa routing.

Internet sharing melalui satu router (Router 1) dapat berjalan.

Setup cocok untuk lab jaringan, warnet, atau skenario LAN bridging.
