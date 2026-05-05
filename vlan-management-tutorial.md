# Tutorial Implementasi VLAN Management pada MikroTik (CCR & CRS)

Dokumen ini berisi panduan teknis langkah-demi-langkah untuk melakukan konfigurasi VLAN Filtering dengan sistem **Management VLAN** pada perangkat MikroTik. Tujuannya adalah agar switch tetap dapat diremote via IP meskipun fitur VLAN Filtering telah diaktifkan.

---

## 🛠️ Topologi Ringkas
* **Router Core (CCR):** Sebagai sumber internet, VLAN, dan DHCP Server.
* **Switch (CRS326):** Sebagai perangkat distribusi yang membagi traffic ke user (VLAN 10 & 20) dan memiliki jalur manajemen (VLAN 99).
* **Uplink:** Menghubungkan `ether2` Router ke `ether1` Switch.

---

## 📶 Fase 1: Konfigurasi pada Router Core (CCR)
Langkah ini bertujuan menyiapkan "sumber" VLAN dan jalur manajemen di router utama.

### 1. Membuat Interface VLAN
Buka **Interfaces** > Tab **VLAN** > Klik **(+)**.
| Nama VLAN | VLAN ID | Interface |
| :--- | :--- | :--- |
| `vlan-10-siswa` | 10 | ether2 |
| `vlan-20-guru` | 20 | ether2 |
| `vlan-mgmt` | 99 | ether2 |

### 2. Memberi IP Address
Buka **IP** > **Addresses** > Klik **(+)**.
* **VLAN 10:** `192.168.10.1/24`
* **VLAN 20:** `192.168.20.1/24`
* **VLAN 99:** `192.168.99.1/24`

### 3. Menyiapkan DHCP Server
Buka **IP** > **DHCP Server** > Klik **DHCP Setup**.
Jalankan setup untuk ketiga interface VLAN tersebut (`vlan-10-siswa`, `vlan-20-guru`, dan `vlan-mgmt`) agar perangkat di bawahnya mendapatkan IP otomatis.

---

## 🔌 Fase 2: Konfigurasi pada Switch (CRS326)
> **⚠️ Catatan Penting:** Hubungkan laptop Anda ke port yang belum dikonfigurasi (misal port terakhir/ether24) atau gunakan akses MAC Address agar tidak terputus saat konfigurasi berlangsung.

### 1. Membuat Bridge
1.  Buka **Bridge** > Tab **Bridge** > Klik **(+)** beri nama `bridge1`.
2.  Buka **Bridge** > Tab **Ports** > Klik **(+)** masukkan port `ether1` sampai `ether24` ke dalam `bridge1`.

### 2. Menentukan Jalur VLAN (VLAN Table)
Buka **Bridge** > Tab **VLANs** > Klik **(+)**.
* **VLAN 10:**
    * Tagged: `ether1` (uplink)
    * Untagged: `ether3`, `ether5`, dst (semua port ganjil).
* **VLAN 20:**
    * Tagged: `ether1`
    * Untagged: `ether2`, `ether4`, dst (semua port genap).
* **VLAN 99 (KUNCI MANAJEMEN):**
    * Tagged: `ether1` **DAN** `bridge1`.
    * *Penjelasan: `bridge1` harus di-tagged agar CPU Switch bisa "melihat" paket VLAN 99 untuk keperluan remote Winbox via IP.*

### 3. Setting PVID (Access Port)
Buka **Bridge** > Tab **Ports**. Lakukan pada setiap port akses (kecuali ether1/uplink):
1.  Klik dua kali pada port (misal `ether2`).
2.  Pilih tab **VLAN**.
3.  **PVID:** Isi sesuai peruntukan (Contoh: `20` untuk guru).
4.  **Frame Types:** Pilih `admit only untagged and priority tagged`.
5.  **Ingress Filtering:** Centang.
6.  Klik **Apply/OK**.

### 4. Membuat Interface VLAN Management di Switch
Agar Switch memiliki "identitas" di VLAN 99:
1.  Buka **Interfaces** > Tab **VLAN** > Klik **(+)**.
2.  **Name:** `vlan-mgmt-switch`
3.  **VLAN ID:** `99`
4.  **Interface:** `bridge1`

### 5. Meminta IP Management (DHCP Client)
1.  Buka **IP** > **DHCP Client** > Klik **(+)**.
2.  **Interface:** `vlan-mgmt-switch`.
3.  Tunggu sampai status **Bound**. Sekarang switch memiliki IP manajemen (misal: `192.168.99.254`).

### 6. Aktivasi VLAN Filtering (Langkah Terakhir)
1.  Buka **Bridge** > Tab **Bridge**.
2.  Klik dua kali pada `bridge1`.
3.  Buka tab **VLAN**.
4.  Centang **VLAN Filtering**.
5.  Klik **Apply**.

---

## 📝 Catatan Penting
* **Lockout Protection:** Pastikan langkah 4 dan 5 (VLAN Management) sudah selesai dan IP sudah didapat sebelum mengaktifkan **VLAN Filtering**.
* **Akses Remote:** Setelah VLAN Filtering aktif, Anda bisa meremote switch ini menggunakan IP Address dari VLAN 99 meskipun Anda terhubung ke port akses VLAN lain (selama routing di Router Core diizinkan).

---
*Dibuat untuk keperluan dokumentasi teknis IT Teacher SMK El-Husna.*
