# 📘 Dokumentasi Konfigurasi DHCP pada MikroTik

## 🧭 Pendahuluan: Apa Itu DHCP?

**DHCP** (Dynamic Host Configuration Protocol) adalah protokol jaringan yang digunakan untuk memberikan alamat IP dan informasi jaringan lainnya secara otomatis kepada perangkat yang terhubung ke jaringan.

---

## 🔄 Perbedaan DHCP Server dan DHCP Client

| Peran          | Fungsi                                                                 |
|----------------|------------------------------------------------------------------------|
| **DHCP Server** | Memberikan IP secara otomatis. Contoh: Router MikroTik memberikan IP ke komputer. |
| **DHCP Client** | Menerima IP dari server. Contoh: MikroTik menerima IP dari modem ISP. |

---

## 🧰 Contoh Kasus

### Topologi Sederhana

<p align="center">
  <img src="https://drive.google.com/uc?export=view&id=1FMHp7jITZCj3q44OIzAjWgVY7Q2fAgSk" alt="Topologi Jaringan" width="600"/>
</p>

---

## ⚙️ Konfigurasi Dasar **Router A** (Sebagai DHCP Server)

### 1. Setting IP Address
```shell
/ip address add address=<IP>/24 interface=ether1  # Interface ke ISP
/ip address add address=<IP>/24 interface=ether2  # Ke jaringan internal
/ip address add address=<IP>/24 interface=ether3  # Ke jaringan client
```
### 2. Setting Default Route (Gateway)
```
/ip route add dst-address=0.0.0.0/0 gateway=<IP dari ISP>
```
### 3. Setting DNS
```
/ip dns set servers=8.8.8.8 allow-remote-requests=yes
```
### 4. Setting Firewall NAT
```
/ip firewall nat add chain=srcnat out-interface=ether1 action=masquerade
```
### 5. Tes Koneksi Internet
```
ping google.com
```
## 📡 Konfigurasi DHCP Server di Router A
Melalui GUI:
IP -> DHCP Server -> DHCP Setup
Langkah-langkah:
- Klik tombol "DHCP Setup".
- Pilih interface: ether3.
- DHCP Address Space: contoh 192.168.2.0/24 (otomatis).
- Gateway: contoh 192.168.2.1 (otomatis).
- Address Pool: contoh 192.168.2.2 - 192.168.2.254.
- DNS Server: contoh 8.8.8.8.
- Lease Time: 1d.

## 📥 Konfigurasi DHCP Client di Router B
### 1. Ubah Nama Router (Opsional)
```
/system identity set name="MikroTik B"
```
### 2. Tambahkan DHCP Client
```
/ip dhcp-client add interface=ether1 use-peer-dns=yes use-peer-ntp=yes add-default-route=yes
```
Gantilah ether1 jika nama interfacenya berbeda.

### 3. Verifikasi IP Address
```
/ip address print
```
Cari IP yang memiliki flag D (Dynamic) untuk memastikan DHCP Client sudah aktif dan berhasil menerima IP.

## ✅ Hasil Akhir
Router A:
Mendapat IP dari ISP secara manual/statis.
Memberikan IP ke client secara otomatis via DHCP Server.
Menyediakan akses internet dan DNS ke jaringan lokal.

Router B:
Mendapatkan IP, DNS, dan gateway secara otomatis dari Router A melalui DHCP Client.
