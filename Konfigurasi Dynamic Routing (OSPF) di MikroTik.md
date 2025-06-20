🌐 Konfigurasi Dynamic Routing (OSPF) Mikrotik <br>

🔍 Pengertian
Dynamic Routing: Rute ditentukan otomatis oleh router, cocok untuk jaringan besar dan kompleks. OSPF (Open Shortest Path First): Protokol routing dinamis berbasis link-state yang menyebarkan dan menjaga tabel routing antar router. Autonomous System (AS): Sekumpulan jaringan dengan kebijakan routing tunggal. OSPF bekerja di dalam satu AS.
⚙️ Konfigurasi Network Adapter (VirtualBox)
Jika menggunakan VirtualBox:

<pre> Router1 (Mikrotik1): - Adapter 1: Host-only Adapter (akses Winbox dari laptop) - Adapter 2: Internal Network (misal: inet1) → ke Router2 - Adapter 3: Internal Network (misal: inet3) → ke Router3 Router2 (Mikrotik2): - Adapter 1: Host-only Adapter - Adapter 2: Internal Network: inet1 - Adapter 3: Internal Network: inet2 (ke Router3) Router3 (Mikrotik3): - Adapter 1: Host-only Adapter - Adapter 2: Internal Network: inet2 - Adapter 3: Internal Network: inet3 </pre>
Catatan: Pastikan nama Internal Network konsisten antar router yang terhubung.

🛠️ Konfigurasi IP Address (di Winbox)
Masuk ke Winbox menggunakan MAC Address, lalu atur IP di setiap router.

Contoh Router1:

<pre> IP > Addresses > + - ether1: 192.168.1.1/24 - ether2: 20.20.20.1/24 - ether3: 10.10.10.1/24 </pre>
Router2:

<pre> - ether1: 192.168.2.1/24 - ether2: 20.20.20.2/24 - ether3: 30.30.30.1/24 </pre>
Router3:

<pre> - ether1: 192.168.3.1/24 - ether2: 30.30.30.2/24 - ether3: 10.10.10.2/24 </pre>
🧪 Pengujian Konektivitas Awal
Lakukan ping dari setiap router ke IP tetangga yang langsung terhubung → Berhasil

Ping ke IP router yang tidak langsung terhubung → Gagal, karena belum ada rute.

🔁 Konfigurasi OSPF
Di setiap router:
untuk router versi lama
Masuk ke Routing > OSPF

Pilih tab Networks

Klik tombol + dan tambahkan network ID berikut dengan Area: backbone


untuk versi baru

Buka tab Areas:
Klik tab Areas di menu OSPF (lihat bar atas: Instances | Interface Templates | Interfaces | Areas ...)

Klik tombol + (add)

2. Isi sebagai berikut:
Name: backbone

Area ID: 0.0.0.0 (ini penting karena backbone = area 0)

Instance: Pilih instance yang sudah kamu buat (biasanya default)

3. Klik OK

Masuk ke tab Interface Templates, lalu:

Tambahkan interface yang punya IP 192.168.1.x

Tambahkan interface yang punya IP 20.20.20.x

Tambahkan interface yang punya IP 10.10.10.x

Pilih Area: backbone, dan Instance sesuai yang kamu buat

Router1:

<pre> 192.168.1.0/24 20.20.20.0/24 10.10.10.0/24 </pre>
Router2:

<pre> 192.168.2.0/24 20.20.20.0/24 30.30.30.0/24 </pre>
Router3:

<pre> 192.168.3.0/24 10.10.10.0/24 30.30.30.0/24 </pre>
⚠️ Gunakan Network ID, bukan IP interface. Area backbone = Area 0 (default OSPF).

🔍 Verifikasi OSPF
Masuk ke tab Interfaces pada OSPF → Pastikan status interface menunjukkan DR, BDR, atau DROTHER

Masuk ke menu IP > Routes → Periksa apakah rute OSPF muncul dengan flag D A O (Dynamic, Active, OSPF)

🧪 Pengujian Konektivitas Lanjut
Setelah OSPF aktif, ping dari setiap router ke IP router lain (yang tidak langsung terhubung) → Harus berhasil

Contoh dari Router1:

<pre> ping 192.168.2.1 ping 192.168.3.1 </pre>
🔄 Simulasi Failover OSPF
Disable salah satu interface antar-router (misal: ether2 Router1)

Cek tabel routing → rute via link tersebut akan hilang

OSPF akan memilih jalur alternatif secara otomatis

Re-enable interface → rute akan kembali muncul

✅ Kesimpulan
OSPF menyederhanakan distribusi rute pada jaringan besar

Otomatisasi rute → meminimalkan kesalahan manual

Mampu mendeteksi dan memperbaiki jalur yang gagal secara dinamis
