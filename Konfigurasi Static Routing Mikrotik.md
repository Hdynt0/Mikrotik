🌐 Konfigurasi Static Routing Mikrotik
Konfigurasi static routing memungkinkan dua jaringan berbeda yang terhubung melalui dua router saling berkomunikasi dan mengakses internet dengan rute manual.

🗺️ Topologi Jaringan
<center><img src="https://drive.google.com/uc?export=view&id=1hlQprqRxytHN4sc1D3HawD0Ml5-GlmJE" width="600"></center>

🧱 Konfigurasi IP Address pada Masing-masing Router
🖥️ Router 1 (via Terminal/CLI)
<pre> interface print ip address print /ip address add address=192.168.10.1/24 interface=ether2 /ip address add address=10.10.10.2/24 interface=ether3 </pre>
🖥️ Router 2 (via Winbox)
<pre> /ip address add address=10.10.10.3/24 interface=ether1 /ip address add address=192.168.20.1/24 interface=ether2 </pre>
🧪 Pengujian Koneksi Awal (Sebelum Routing)
Ping Router 2 → Router 1 (10.10.10.2): ✅ Berhasil

Ping Router 2 → 192.168.10.1: ❌ Gagal ("no route to host")

Ping Router 1 → Router 2 (10.10.10.3): ✅ Berhasil

Ping Router 1 → 192.168.20.1: ❌ Gagal

🛣️ Konfigurasi Static Routing
🔹 Router 1 (via Terminal/CLI)
<pre> /ip route add dst-address=192.168.20.0/24 gateway=10.10.10.3 </pre>
Ping ke 192.168.20.1 → ✅ Berhasil

🔹 Router 2 (via Winbox)
<pre> IP > Routes > Klik + Dst. Address: 192.168.10.0/24 Gateway: 10.10.10.2 </pre>
Ping ke 192.168.10.1 → ✅ Berhasil

💻 Konfigurasi IP Address pada Client
🧑‍💻 Client 2:
<pre> IP Address: 192.168.20.100 Subnet Mask: 255.255.255.0 Default Gateway: 192.168.20.1 DNS Server: 192.168.20.1 </pre>
✅ Semua ping (ke gateway, Router 1 & 2) → Berhasil

🧑‍💻 Client 1:
<pre> IP Address: 192.168.10.100 Subnet Mask: 255.255.255.0 Default Gateway: 192.168.10.1 DNS Server: 192.168.10.1 </pre>
✅ Semua ping (ke gateway, Router 1 & 2) → Berhasil

🌍 Konfigurasi Akses Internet
🔹 Router 1 (via Terminal/CLI)
<pre> /ip address add address=192.168.137.100/24 interface=ether1 /ip route add gateway=192.168.137.1 /ip firewall nat add chain=srcnat out-interface=ether1 action=masquerade /ip dns set servers=192.168.137.1,8.8.8.8 allow-remote-requests=yes </pre>
Ping ke 8.8.8.8 dan google.com: ✅ Berhasil

Ping dari Client 1 ke internet: ✅ Berhasil

🔹 Router 2 (via Winbox)
<pre> IP > Routes > Klik + Dst. Address: 0.0.0.0/0 Gateway: 10.10.10.2 IP > DNS Servers: 10.10.10.2 Allow Remote Requests: [✓] </pre>
Ping ke google.com dari Router 2: ✅

Ping dari Client 2 ke internet: ✅

Tidak perlu NAT di Router 2, karena sudah ditangani oleh Router 1.

✅ Kesimpulan
Dengan konfigurasi ini:

Dua jaringan lokal di Router 1 dan Router 2 berhasil saling terhubung.

Klien dari kedua sisi bisa berkomunikasi dan mengakses internet.

Routing manual berhasil dilakukan menggunakan static routing.
