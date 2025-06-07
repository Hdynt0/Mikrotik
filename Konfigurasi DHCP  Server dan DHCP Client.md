🧰 Topologi Sederhana
<p align="center">
  <img src="https://drive.google.com/uc?export=view&id=1FMHp7jITZCj3q44OIzAjWgVY7Q2fAgSk" alt="Topologi Jaringan" width="600"/>
</p>

⚙️ Konfigurasi Dasar Router A (Sebagai DHCP Server)

Setting IP Address
<pre>/ip address add address="ip/24" interface="ke isp" </pre>
<pre>/ip address add address="ip/24" interface=ether2 </pre>
<pre>/ip address add address="ip/24" interface=ether3</pre>

Setting Default Route (Gateway)
<pre>/ip route add dst-address=0.0.0.0/0 gateway="dari isp"</pre>

Setting DNS
<pre>/ip dns set servers=8.8.8.8,8.8.4.4 allow-remote-requests=yes</pre>

4. Setting Firewall NAT
<pre>/ip firewall nat add chain=srcnat out-interface="ke isp" action=masquerade</pre>

5. Tes Koneksi Internet
<pre>ping google.com</pre>

📡 Konfigurasi DHCP Server di Router A
GUI: IP -> DHCP Server -> DHCP Setup

Langkah-langkah:
Klik tombol "DHCP Setup".
Pilih interface: ether3.
DHCP Address Space: 192.168.2.0/24 (otomatis).
Gateway: 192.168.2.1 (otomatis).
Address Pool: 192.168.2.2 - 192.168.2.254.
DNS Server: 8.8.8.8.
Lease Time: 1d.

📥 Konfigurasi DHCP Client di Router B
1. Ubah Nama Router (Opsional)
<pre>/system identity set name="MikroTik B"</pre>
2. Tambahkan DHCP Client
<pre>/ip dhcp-client add interface=ether1 use-peer-dns=yes use-peer-ntp=yes add-default-route=yes</pre>
Gantilah ether1 jika nama interface berbeda.

3. Verifikasi IP Address
<pre>/ip address print</pre>
Cari IP dengan flag D (Dynamic) untuk memastikan DHCP Client sudah aktif dan mendapatkan IP.

✅ Hasil Akhir
Router A:
Mendapat IP dari ISP secara statis.
Menyediakan IP otomatis ke client melalui DHCP.
Menyediakan akses internet dan DNS.

Router B:
Mendapat IP, DNS, dan gateway secara otomatis dari Router A via DHCP Client.
