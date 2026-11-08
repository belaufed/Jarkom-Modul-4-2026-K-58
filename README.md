# Laporan Praktikum Modul 4 — CIDR (Cisco Packet Tracer)

**Mata Praktikum:** Komunikasi Data dan Jaringan Komputer 2026  
**Materi:** Classless Inter-Domain Routing (CIDR), subnetting, supernetting, dan static routing  
**Kelompok:** *K-58*  
**Anggota:** *M. Salman Rifki (5027251095) Nadia Iqlima (5027251108)*  
**Bagian yang dikerjakan:** CIDR menggunakan Cisco Packet Tracer (CPT)

## 1. Deskripsi Praktikum

Praktikum ini mengimplementasikan topologi jaringan dengan beberapa router, switch, PC, laptop, dan server di Cisco Packet Tracer. Setiap segmen mendapat alamat IP dari blok dasar **`192.240.0.0/16`** melalui perhitungan subnetting CIDR. Sesudah konfigurasi alamat IP, dilakukan pengaktifan interface, pengaturan link serial, penerapan **static routing**, serta pengujian komunikasi antarsubnet.

Hasil pembagian IP dicatat pada spreadsheet, sedangkan proses penggabungan subnet (*supernetting*) ditunjukkan melalui gambar bertahap dari Canva.

## 2. CIDR

Topologi terdiri atas router **Cyberia, Wired, Psyche, Copland, Schumann, Accela, Navi, Tachibana, Deus, Protocol7, Knights, Phantoma,** dan **Shibuya**. Cyberia terhubung dengan Cloud-PT; koneksi Cloud tersebut ditampilkan sebagai bagian topologi eksternal, bukan koneksi internet yang telah terbukti aktif.

## 3. Pembagian Alamat IP

Blok IP dasar: **`192.240.0.0/16`**. Alokasi berikut mengikuti spreadsheet CIDR yang digunakan saat konfigurasi CPT.

| Subnet | Segmen / koneksi | Network ID | Prefix | Netmask |
|---|---|---|---|---|
| A1 | Karl + Lin | `192.240.0.0` | `/22` | `255.255.252.0` |
| A2 | Omnipresence + Denpa | `192.240.4.0` | `/22` | `255.255.252.0` |
| A3 | Masayuki | `192.240.8.0` | `/23` | `255.255.254.0` |
| A4 | Juri | `192.240.10.0` | `/26` | `255.255.255.192` |
| A5 | Akashic | `192.240.10.64` | `/29` | `255.255.255.248` |
| A6 | Schumann–Accela | `192.240.10.72` | `/30` | `255.255.255.252` |
| A7 | Alice + Reika | `192.240.16.0` | `/24` | `255.255.255.0` |
| A8 | Yasuo + Miho + Kuma | `192.240.17.0` | `/24` | `255.255.255.0` |
| A9 | Taro | `192.240.18.0` | `/26` | `255.255.255.192` |
| A10 | Collective + Ghost | `192.240.20.0` | `/23` | `255.255.254.0` |
| A11 | Lilith + Specter | `192.240.22.0` | `/25` | `255.255.255.128` |
| A12 | Chisa + Hodgeson | `192.240.22.128` | `/26` | `255.255.255.192` |
| A13 | Lain + Mika | `192.240.22.192` | `/26` | `255.255.255.192` |
| A14 | Semi | `192.240.23.0` | `/27` | `255.255.255.224` |
| A15 | Eiri | `192.240.23.32` | `/29` | `255.255.255.248` |
| A16 | JJ + MyuMyu | `192.240.24.0` | `/25` | `255.255.255.128` |
| A17 | Switch2 (Wired, Psyche, Copland, Schumann) | `192.240.24.128` | `/29` | `255.255.255.248` |
| A18 | Cyberia–Wired | `192.240.24.136` | `/30` | `255.255.255.252` |
| A19 | Wired–Navi | `192.240.24.140` | `/30` | `255.255.255.252` |
| A20 | Wired–Tachibana | `192.240.24.144` | `/30` | `255.255.255.252` |
| A21 | Tachibana–Deus | `192.240.24.148` | `/30` | `255.255.255.252` |
| A22 | Deus–Protocol7 | `192.240.24.152` | `/30` | `255.255.255.252` |
| A23 | Protocol7–Knights | `192.240.24.156` | `/30` | `255.255.255.252` |
| A24 | Knights–Phantoma | `192.240.24.160` | `/30` | `255.255.255.252` |
| A25 | Phantoma–Shibuya | `192.240.24.164` | `/30` | `255.255.255.252` |

## 4. Langkah Implementasi di Cisco Packet Tracer

### 4.1 Menyusun perangkat dan menghubungkan kabel

1. Menempatkan router Cisco 2811 sesuai topologi, bersama switch 2960, PC, laptop, server, dan Cloud-PT.
2. Menghubungkan perangkat akhir ke switch dengan **Copper Straight-Through**. Perangkat seperti Juri, Akashic, dan Eiri terhubung langsung ke router.
3. Menghubungkan router ke switch dan menyusun jalur router antarwilayah sesuai topologi.
4. Memasang modul **WIC-2T** pada router yang memerlukan koneksi serial, kemudian menghubungkan router menggunakan kabel **Serial DCE**.
5. Memastikan port router sesuai label fisik pada topologi; nomor interface bergantung pada posisi modul.

### 4.2 Konfigurasi IP interface router

IP interface router diisi menggunakan CLI. Contoh yang digunakan pada **Cyberia**:

```cisco
enable
configure terminal
no ip domain-lookup
interface FastEthernet0/0
 ip address 192.240.24.1 255.255.255.128
 no shutdown
exit
interface Serial0/3/0
 ip address 192.240.24.137 255.255.255.252
 no shutdown
exit
end
write memory
show ip interface brief
```

Pada **Wired**, port ke jaringan bersama Switch2 menggunakan alamat berikut:

```cisco
enable
configure terminal
interface FastEthernet0/0
 ip address 192.240.24.129 255.255.255.248
 no shutdown
exit
end
write memory
```

Satu link serial yang juga dikonfigurasi adalah **Protocol7–Knights (A23)**. Setelah modul serial Knights dipasang ulang, port Knights berubah menjadi `Serial0/0/1`, sehingga konfigurasinya disesuaikan:

**Protocol7:**

```cisco
enable
configure terminal
interface Serial0/3/1
 ip address 192.240.24.157 255.255.255.252
 no shutdown
exit
end
write memory
```

**Knights:**

```cisco
enable
configure terminal
interface Serial0/0/1
 ip address 192.240.24.158 255.255.255.252
 clock rate 64000
 no shutdown
exit
end
write memory
```

Perintah `clock rate` diterapkan pada interface yang menjadi sisi **DCE**. Setelah koneksi diperbaiki, pengujian ping link A23 berhasil.

### 4.3 Konfigurasi IP perangkat akhir

Untuk PC, laptop, dan server: **Desktop → IP Configuration → Static**. IP, netmask, dan default gateway diisi sesuai subnet masing-masing.

Beberapa contoh alamat yang diterapkan:

| Perangkat | IP Address | Netmask | Gateway |
|---|---|---|---|
| JJ | `192.240.24.2` | `255.255.255.128` | `192.240.24.1` |
| MyuMyu | `192.240.24.3` | `255.255.255.128` | `192.240.24.1` |
| Alice | `192.240.16.2` | `255.255.255.0` | `192.240.16.1` |
| Reika | `192.240.16.3` | `255.255.255.0` | `192.240.16.1` |
| Collective | `192.240.20.2` | `255.255.254.0` | `192.240.20.1` |
| Lilith | `192.240.22.2` | `255.255.255.128` | `192.240.22.1` |
| Chisa | `192.240.22.130` | `255.255.255.192` | `192.240.22.129` |
| Eiri | `192.240.23.34` | `255.255.255.248` | `192.240.23.33` |
| Semi | `192.240.23.2` | `255.255.255.224` | `192.240.23.1` |
| Karl | `192.240.0.2` | `255.255.252.0` | `192.240.0.1` |
| Lain | `192.240.22.194` | `255.255.255.192` | `192.240.22.193` |

### 4.4 Konfigurasi static routing

Agar jaringan antarsubnet dapat saling berkomunikasi, digunakan **static routing**. Router cabang umumnya memakai default route menuju router yang lebih dekat ke Wired, sedangkan router Wired dan router perantara memiliki rute balik menuju subnet tujuan.

Contoh default route di **Cyberia**:

```cisco
enable
configure terminal
ip route 0.0.0.0 0.0.0.0 192.240.24.138
end
write memory
```

Contoh rute di **Wired** yang digunakan untuk menjangkau jaringan Navi dan Cyberia:

```cisco
enable
configure terminal
ip route 192.240.16.0 255.255.254.0 192.240.24.142
ip route 192.240.18.0 255.255.255.192 192.240.24.142
ip route 192.240.24.0 255.255.255.128 192.240.24.137
end
write memory
```

Contoh routing ke jaringan wilayah kiri melalui Switch2:

```cisco
! Dijalankan pada router Wired
enable
configure terminal
ip route 192.240.0.0 255.255.252.0 192.240.24.130
ip route 192.240.4.0 255.255.252.0 192.240.24.132
ip route 192.240.8.0 255.255.254.0 192.240.24.131
ip route 192.240.10.0 255.255.255.192 192.240.24.131
ip route 192.240.10.64 255.255.255.248 192.240.24.132
end
write memory
```

Contoh routing di jalur wilayah kanan:

```cisco
! Dijalankan pada router Tachibana
enable
configure terminal
ip route 0.0.0.0 0.0.0.0 192.240.24.145
ip route 192.240.22.0 255.255.254.0 192.240.24.150
end
write memory
```

> Baris-baris di atas merupakan **contoh perintah yang digunakan dalam pengerjaan**, bukan berkas ekspor konfigurasi lengkap setiap router. Untuk melihat rute dan konfigurasi aktual pada perangkat di file `.pkt`, gunakan `show ip route` atau `show running-config`.

### 4.5 Verifikasi dan pengujian

Kondisi interface diperiksa menggunakan:

```cisco
show ip interface brief
show ip route
```

Komunikasi diuji menggunakan `ping` dari perangkat akhir maupun router, misalnya:

```text
# Di Command Prompt JJ
ping 192.240.24.1
ping 192.240.16.2

# Di Command Prompt Collective
ping 192.240.22.2
ping 192.240.22.130
ping 192.240.23.34
ping 192.240.23.2

# Di Command Prompt Lain
ping 192.240.0.2
ping 192.240.16.2
ping 192.240.24.2
```

Dalam proses pengujian, koneksi **Protocol7–Knights** sempat `down/down` dan diselesaikan setelah pemeriksaan modul WIC-2T, port serial, serta konfigurasi interface. Koneksi **Semi–Phantoma** juga sempat gagal karena interface LAN Phantoma belum diberi IP dan masih `administratively down`. Setelah IP gateway `192.240.23.1/27` diaktifkan pada `FastEthernet0/0`, pengujian dapat dilanjutkan dan dilaporkan berhasil.

## 5. Proses Penggabungan CIDR (*Supernetting*)

Supernetting didokumentasikan melalui beberapa tahap penggabungan pada spreadsheet dan Canva:

| Tahap | Hasil | Gabungan | Prefix akhir |
|---|---|---|---|
| B | B1 | A1 + A2 | `/21` |
| B | B2 | A7 + A8 | `/23` |
| B | B3 | A12 + A13 | `/25` |
| B | B4 | A18 + A19 | `/29` |
| B | B5 | A20 + A21 | `/29` |
| B | B6 | A22 + A23 | `/29` |
| B | B7 | A24 + A25 | `/29` |
| C | C1 | A11 + B3 | `/24` |
| C | C2 | A17 + B4 | `/28` |
| C | C3 | B5 + B6 | `/28` |
| D | D1 | C2 + C3 | `/27` |

Untuk memperlihatkan satu blok yang **mencakup seluruh rentang subnet**, diperoleh **`192.240.0.0/19`** dengan netmask **`255.255.224.0`**. Rentangnya adalah `192.240.0.0–192.240.31.255`.

**Perhatian:** `/19` merupakan *covering supernet* yang juga mencakup alamat yang tidak dialokasikan pada A1–A25. Pengelompokan pada Canva bersifat dokumentasi peringkasan alamat, **bukan perintah untuk memasang satu rute `/19` pada seluruh router**. Rute aktual tetap harus sesuai arah jaringan/next-hop.


### Dokumentasi Subnetting dan Supernetting CIDR

#### Tahap A — Pembagian Subnet Awal
![Tahap A](images/CIDR%28A%29.png)

#### Tahap B — Penggabungan Pertama
![Tahap B](images/CIDR%28B%29.png)

#### Tahap C — Penggabungan Kedua
![Tahap C](images/CIDR%28C%29.png)

#### Tahap D — Penggabungan Ketiga
![Tahap D](images/CIDR%28D%29.png)

#### Tahap E — Penggabungan Keempat
![Tahap E](images/CIDR%28E%29.png)

#### Tahap F — Supernetting Akhir
![Tahap F](images/CIDR%28F%29.png)


## 6. Kesimpulan

Topologi jaringan berhasil disusun di Cisco Packet Tracer dengan alokasi 25 subnet (A1–A25) dari blok `192.240.0.0/16`. Router, perangkat akhir, dan koneksi serial dikonfigurasi menggunakan IP dari spreadsheet. Static routing diterapkan sehingga pengujian komunikasi antarsubnet yang dilakukan selama pengerjaan berhasil setelah beberapa perbaikan interface.

Penggabungan CIDR ditampilkan bertahap pada spreadsheet dan Canva. Blok peringkas terkecil yang mencakup seluruh subnet adalah `192.240.0.0/19`, dengan catatan bahwa blok ini juga mencakup alamat tambahan di luar subnet yang digunakan.


# Laporan Praktikum Jaringan Komputer: Topologi "The Wired"

Repositori ini berisi dokumentasi dan langkah pengerjaan praktikum jaringan komputer menggunakan simulator **GNS3**. Proyek ini mencakup perancangan arsitektur *routing*, perhitungan subnetting (VLSM), konfigurasi otomatis menggunakan skrip, dan pengujian *end-to-end*.

---

## Tahap 1: Perhitungan Subnetting (VLSM)

Blok IP dasar yang digunakan adalah `192.240.0.0/16`. Subnetting dilakukan menggunakan metode **Route-Based Subnetting (Hierarchical Subnetting)** untuk mengelompokkan IP berdasarkan jalur *router*, yang berguna meringkas tabel *routing* (Supernetting).

Berikut adalah tabel hasil pembagian IP VLSM yang diterapkan pada topologi:

| Subnet | Network ID | Netmask | Broadcast | Range IP | Prefix |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **A1** | 192.240.0.0 | 255.255.252.0 | 192.240.3.255 | 192.240.0.1 - 192.240.3.254 | /22 |
| **A2** | 192.240.4.0 | 255.255.252.0 | 192.240.7.255 | 192.240.4.1 - 192.240.7.254 | /22 |
| **A3** | 192.240.8.0 | 255.255.254.0 | 192.240.9.255 | 192.240.8.1 - 192.240.9.254 | /23 |
| **A4** | 192.240.10.0 | 255.255.254.0 | 192.240.11.255 | 192.240.10.1 - 192.240.11.254 | /23 |
| **A5** | 192.240.12.0 | 255.255.255.0 | 192.240.12.255 | 192.240.12.1 - 192.240.12.254 | /24 |
| **A6** | 192.240.13.0 | 255.255.255.0 | 192.240.13.255 | 192.240.13.1 - 192.240.13.254 | /24 |
| **A7** | 192.240.14.0 | 255.255.255.128 | 192.240.14.127 | 192.240.14.1 - 192.240.14.126 | /25 |
| **A8** | 192.240.14.128 | 255.255.255.128 | 192.240.14.255 | 192.240.14.129 - 192.240.14.254 | /25 |
| **A9** | 192.240.15.0 | 255.255.255.192 | 192.240.15.63 | 192.240.15.1 - 192.240.15.62 | /26 |
| **A10** | 192.240.15.64 | 255.255.255.192 | 192.240.15.127 | 192.240.15.65 - 192.240.15.126 | /26 |
| **A11** | 192.240.15.128 | 255.255.255.192 | 192.240.15.191 | 192.240.15.129 - 192.240.15.190 | /26 |
| **A12** | 192.240.15.192 | 255.255.255.192 | 192.240.15.255 | 192.240.15.193 - 192.240.15.254 | /26 |
| **A13** | 192.240.16.0 | 255.255.255.224 | 192.240.16.31 | 192.240.16.1 - 192.240.16.30 | /27 |
| **A14** | 192.240.16.32 | 255.255.255.248 | 192.240.16.39 | 192.240.16.33 - 192.240.16.38 | /29 |
| **A15** | 192.240.16.40 | 255.255.255.248 | 192.240.16.47 | 192.240.16.41 - 192.240.16.46 | /29 |
| **A16** | 192.240.16.48 | 255.255.255.248 | 192.240.16.55 | 192.240.16.49 - 192.240.16.54 | /29 |
| **A17** | 192.240.16.56 | 255.255.255.252 | 192.240.16.59 | 192.240.16.57 - 192.240.16.58 | /30 |
| **A18** | 192.240.16.60 | 255.255.255.252 | 192.240.16.63 | 192.240.16.61 - 192.240.16.62 | /30 |
| **A19** | 192.240.16.64 | 255.255.255.252 | 192.240.16.67 | 192.240.16.65 - 192.240.16.66 | /30 |
| **A20** | 192.240.16.68 | 255.255.255.252 | 192.240.16.71 | 192.240.16.69 - 192.240.16.70 | /30 |
| **A21** | 192.240.16.72 | 255.255.255.252 | 192.240.16.75 | 192.240.16.73 - 192.240.16.74 | /30 |
| **A22** | 192.240.16.76 | 255.255.255.252 | 192.240.16.79 | 192.240.16.77 - 192.240.16.78 | /30 |
| **A23** | 192.240.16.80 | 255.255.255.252 | 192.240.16.83 | 192.240.16.81 - 192.240.16.82 | /30 |
| **A24** | 192.240.16.84 | 255.255.255.252 | 192.240.16.87 | 192.240.16.85 - 192.240.16.86 | /30 |
| **A25** | 192.240.16.88 | 255.255.255.252 | 192.240.16.91 | 192.240.16.89 - 192.240.16.90 | /30 |

*(Catatan: Rute A17 hingga A25 adalah blok /30 yang digunakan khusus untuk jaringan point-to-point antar-router).*

---

## Tahap 2: Setup Klien (VPCS)

Setiap PC virtual dikonfigurasi dengan IP Address dari *Range IP* dan dihubungkan ke *Gateway* (IP Router yang terhubung langsung).

1. Buka *console* VPCS.
2. Tambahkan IP address dan IP Gateway menggunakan sintaks: `ip [IP_PC]/[Prefix] [IP_Gateway]`.
3. Simpan konfigurasi menggunakan perintah `save` agar tidak hilang saat direstart.

**Contoh (Konfigurasi PC Karl):**
```bash
Karl> ip 192.240.0.2/22 192.240.0.1
Karl> save
```

---

## Tahap 3: Setup Router (Alpine Linux)

Konfigurasi antarmuka jaringan dan tabel *routing* pada router dilakukan menggunakan *script* agar otomatis memuat konfigurasi.

### 1. Membuat dan Mengedit Script
Buka teks editor `vi` untuk membuat *script* konfigurasi IP di direktori root.
```bash
Router:~# vi /root/config-ip.sh
```

### 2. Isi Script Konfigurasi
Tekan `i` untuk masuk ke *Insert Mode*, lalu tulis konfigurasi. Script ini bertugas untuk:
* Mengatur IP pada port fisik (`ip addr add`).
* Mengaktifkan IP antarmuka (`ip link set up`).
* Menghidupkan fitur *forwarding* antar-subnet.
* Menambahkan rute spesifik (*Static Route*) atau NAT (khusus router perbatasan).

**Contoh isi script `/root/config-ip.sh` (Router Cyberia / Edge):**
```bash
#!/bin/ash
# Konfigurasi IP Interface
ip addr add 192.240.0.1/22 dev eth0
ip link set eth0 up

# Mengaktifkan IP Forwarding
sysctl -w net.ipv4.ip_forward=1

# Konfigurasi Static Route (Supernetting ke cabang bawah)
ip route add 192.240.0.0/17 via 192.240.16.62

# Konfigurasi NAT (Masquerade) ke Internet
iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE
```
*(Tekan `Esc`, lalu ketik `:wq` dan tekan `Enter` untuk menyimpan dan keluar).*

### 3. Eksekusi Script
Setelah file dibuat, jalankan *script* tersebut (lakukan ini pada **setiap router** setelah GNS3 dinyalakan):
```bash
Router:~# sh /root/config-ip.sh
```

---

## Tahap 4: Pengujian Keterhubungan (Testing)

Setelah semua klien disematkan IP dan *script* router dieksekusi, verifikasi dilakukan melalui *console* VPCS.

### 1. Pengecekan Routing Antar-Subnet (Ping & Trace)
Digunakan untuk melihat apakah paket data berhasil menyeberangi berbagai router dari satu ujung ke ujung lain (End-to-End).

```bash
# Mengetes ping dari ujung ke ujung
Karl> ping 192.240.15.3

# Melacak jalur (hop) yang dilewati paket
Karl> trace 192.240.15.3
```
*Hasil sukses akan menampilkan `Reply` secara konsisten pada PING, dan daftar IP router yang dilalui pada TRACE tanpa adanya pesan `Destination Host Unreachable`.*

### 2. Pengecekan Koneksi Eksternal (NAT / Internet)
Digunakan untuk menguji apakah jaringan *The Wired* sudah bisa mengakses internet berkat translasi NAT di Router Cyberia.

```bash
Collective> ping 8.8.8.8
```
*Hasil yang sukses akan memunculkan balasan dari server DNS Google (8.8.8.8).*

### Dokumentasi Subnetting
#### Pembagian Subnet
![Subnetting VLSM](images/SubnettingVLSM.png)
