# Tugas-1-Jarcome_Adiwidya_012

**Mata Kuliah:** Komunikasi Data dan Jaringan Komputer

---

**Disusun oleh:**
* **Nama:** Adiwidya Budi Pratama
* **NRP:** 502724101

---

##  navigasi
* [Deskripsi Tugas](#-deskripsi-tugas)
* [Solusi & Implementasi](#-solusi--implementasi)
  * [Desain Topologi](#1-desain-topologi-cpt)
  * [Perhitungan Subnetting](#2-perhitungan-subnetting-vlsm--cidr)
  * [Konfigurasi Perangkat](#3-konfigurasi-perangkat)
* [Verifikasi & Pengujian](#-verifikasi--pengujian)

---

## 📝 Deskripsi Tugas
Studi kasus untuk tugas ini adalah:

> Yayasan Pendidikan ARA akan membangun jaringan untuk beberapa unit kerja. Sebagian unit berada di kantor pusat, sedangkan Bidang Pengawas Sekolah berada di kantor cabang. Rancanglah topologinya menggunakan Cisco Packet Tracer dan lakukan perhitungan subnetting dengan VLSM & CIDR.
>
**Base Network:** `10.52.0.0`

| Ruang (Departemen) | Kebutuhan | Prefix |
| :--- | :---: | :---: |
| Sekretariat | 380 | /23 |
| Bidang Kurikulum | 220 | /24 |
| Bidang Guru & Tendik | 95 | /25 |
| Bidang Sarana Prasarana | 45 | /26 |
| B. Pengawas Sekolah (Cabang) | 18 | /27 |
| Server & Admin | 6 | /29 |
| WAN Link (Pusat <-> Cabang) | 2 | /30 |
| **TOTAL** | **766** | |

---

## 💡 Solusi & Implementasi

### 1. Desain Topologi (CPT)
Desain topologi dibagi menjadi dua lokasi: Kantor Pusat dan Kantor Cabang, yang dihubungkan oleh link WAN Serial. Kantor Pusat menggunakan desain **"Router-on-a-Stick"** dengan VLAN untuk memisahkan setiap departemen secara logis.

<img width="1271" height="596" alt="Screenshot 2025-11-11 194707" src="https://github.com/user-attachments/assets/b2febb9d-5e4b-41ed-a1f6-f9bd7a6633f9" />

### 2. Perhitungan Subnetting (VLSM & CIDR)
<img width="1313" height="665" alt="Screenshot 2025-11-11 201655" src="https://github.com/user-attachments/assets/2be243e6-423c-44d5-a3f2-789eccbcde37" />
<img width="560" height="649" alt="image" src="https://github.com/user-attachments/assets/adf6f581-40b9-4976-97fc-8dc8a4714a5c" />

Perhitungan ini didasarkan pada **Base Network `10.52.0.0`**

**Tabel VLSM:**
| Nama Subnet (Ruang) | Kebutuhan Host | Prefix | Network ID | Mask | Range Host | Broadcast | Gateway |
| :--- | :---: | :---: | :--- | :--- | :--- | :--- | :--- |
| **Sekretariat** | 380 | /23 | `10.52.0.0` | `255.255.254.0` | `10.52.0.1` - `10.52.1.254` | `10.52.1.255` | `10.52.0.1` |
| **Bidang Kurikulum** | 220 | /24 | `10.52.2.0` | `255.255.255.0` | `10.52.2.1` - `10.52.2.254` | `10.52.2.255` | `10.52.2.1` |
| **Bidang Guru & Tendik** | 95 | /25 | `10.52.3.0` | `255.255.255.128`| `10.52.3.1` - `10.52.3.126` | `10.52.3.127` | `10.52.3.1` |
| **Bidang Sarana Prasarana**| 45 | /26 | `10.52.3.128` | `255.255.255.192`| `10.52.3.129` - `10.52.3.190` | `10.52.3.191` | `10.52.3.129` |
| **B. Pengawas (Cabang)** | 18 | /27 | `10.52.3.192` | `255.255.255.224`| `10.52.3.193` - `10.52.3.222` | `10.52.3.223` | `10.52.3.193` |
| **Server & Admin** | 6 | /29 | `10.52.3.224` | `255.255.255.248`| `10.52.3.225` - `10.52.3.230` | `10.52.3.231` | `10.52.3.225` |
| **WAN Link** | 2 | /30 | `10.52.3.232` | `255.255.255.252`| `10.52.3.233` - `10.52.3.234` | `10.52.3.235` | (N/A) |

- Visualisasi
  ```
  10.52.0.0/22 (Total Blok Alokasi Yayasan ARA)
  │
  ├── 10.52.0.0/23 (Blok 1: Dialokasikan untuk Sekretariat)
  │   │
  │   ├── Network: 10.52.0.0
  │   ├── Range: 10.52.0.1 - 10.52.1.254 (510 host)
  │   └── Broadcast: 10.52.1.255
  │
  ├── 10.52.2.0/24 (Blok 2: Dialokasikan untuk Bidang Kurikulum)
  │   │
  │   ├── Network: 10.52.2.0
  │   ├── Range: 10.52.2.1 - 10.52.2.254 (254 host)
  │   └── Broadcast: 10.52.2.255
  │
  └── 10.52.3.0/24 (Blok 3: Sisa blok untuk departemen yang lebih kecil)
      │
      ├── 10.52.3.0/25 (Blok 3a: Dialokasikan untuk Guru & Tendik)
      │   │
      │   ├── Network: 10.52.3.0
      │   ├── Range: 10.52.3.1 - 10.52.3.126 (126 host)
      │   └── Broadcast: 10.52.3.127
      │
      └── 10.52.3.128/25 (Blok 3b: Sisa dari Blok 3)
          │
          ├── 10.52.3.128/26 (Blok 3b-1: Dialokasikan untuk Sarana Prasarana)
          │   │
          │   ├── Network: 10.52.3.128
          │   ├── Range: 10.52.3.129 - 10.52.3.190 (62 host)
          │   └── Broadcast: 10.52.3.191
          │
          └── 10.52.3.192/26 (Blok 3b-2: Sisa dari Blok 3b)
              │
              ├── 10.52.3.192/27 (Blok 3b-2a: Dialokasikan untuk Pengawas Sekolah)
              │   │
              │   ├── Network: 10.52.3.192
              │   ├── Range: 10.52.3.193 - 10.52.3.222 (30 host)
              │   └── Broadcast: 10.52.3.223
              │
              └── 10.52.3.224/27 (Blok 3b-2b: Sisa dari Blok 3b-2)
                  │
                  ├── 10.52.3.224/29 (Blok 3b-2b-i: Dialokasikan untuk Server & Admin)
                  │   │
                  │   ├── Network: 10.52.3.224
                  │   ├── Range: 10.52.3.225 - 10.52.3.230 (6 host)
                  │   └── Broadcast: 10.52.3.231
                  │
                  ├── 10.52.3.232/30 (Blok 3b-2b-ii: Dialokasikan untuk WAN Link)
                  │   │
                  │   ├── Network: 10.52.3.232
                  │   ├── Range: 10.52.3.233 - 10.52.3.234 (2 host)
                  │   └── Broadcast: 10.52.3.235
                  │
                  └── (Sisa IP Address: 10.52.3.236 - 10.52.3.255 tidak terpakai)
  ```

**Tabel CIDR (Supernetting Total):**
- Seluruh alokasi jaringan Yayasan ARA (`10.52.0.0` hingga `10.52.3.255`) dapat diagregasi menjadi satu supernet:
<img width="1217" height="707" alt="Screenshot 2025-11-11 202456" src="https://github.com/user-attachments/assets/14dcbebc-09b5-49eb-8294-fbdb10c48cb2" />

<img width="768" height="328" alt="image" src="https://github.com/user-attachments/assets/0acf4870-4e49-4063-b79b-d0fa028bf5c6" />

| Network (Supernet) | Mask (Supernet) | Prefix (CIDR) |
| :--- | :--- | :---: |
| **`10.52.0.0`** | `255.255.252.0` | **/22** |

| Subnet (Ruang) | Network ID | Mask | Prefix | Range Host (IP Usable) | Broadcast | Gateway (Rekomendasi) |
| :--- | :--- | :--- | :---: | :--- | :--- | :--- |
| Sekretariat | `10.52.0.0` | `255.255.254.0` | /23 | `10.52.0.1` - `10.52.1.254` | `10.52.1.255` | `10.52.0.1` |
| Bidang Kurikulum | `10.52.2.0` | `255.255.255.0` | /24 | `10.52.2.1` - `10.52.2.254` | `10.52.2.255` | `10.52.2.1` |
| Bidang Guru & Tendik | `10.52.3.0` | `255.255.255.128`| /25 | `10.52.3.1` - `10.52.3.126` | `10.52.3.127` | `10.52.3.1` |
| Bidang Sarana Prasarana | `10.52.3.128` | `255.255.255.192`| /26 | `10.52.3.129` - `10.52.3.190` | `10.52.3.191` | `10.52.3.129` |
| Server & Admin | `10.52.3.224` | `255.255.255.248`| /29 | `10.52.3.225` - `10.52.3.230` | `10.52.3.231` | `10.52.3.225` |
| B. Pengawas Sekolah | `10.52.3.192` | `255.255.255.224`| /27 | `10.52.3.193` - `10.52.3.222` | `10.52.3.223` | `10.52.3.193` |
| WAN Link | `10.52.3.232` | `255.255.255.252`| /30 | `10.52.3.233` - `10.52.3.234` | `10.52.3.235` | (N/A) |

- Visualisasi
  ```
  10.52.0.0/22 (Total Alokasi Yayasan ARA)
  ├── 10.52.0.0/23 (Sekretariat)
  │   ├── Network: 10.52.0.0
  │   └── Broadcast: 10.52.1.255
  │
  ├── 10.52.2.0/24 (Bidang Kurikulum)
  │   ├── Network: 10.52.2.0
  │   └── Broadcast: 10.52.2.255
  │
  └── 10.52.3.0/24 (Agregasi dari subnet-subnet kecil)
      ├── Network: 10.52.3.0
      ├── Broadcast: 10.52.3.255
      │
      ├── 10.52.3.0/25    (Guru & Tendik)
      ├── 10.52.3.128/26  (Sarana Prasarana)
      ├── 10.52.3.192/27  (B. Pengawas Sekolah)
      ├── 10.52.3.224/29  (Server & Admin)
      └── 10.52.3.232/30  (WAN Link)
  ```
---

### 3. Konfigurasi Perangkat
Konfigurasi lengkap dapat dilihat langsung di file `.pkt`. Berikut adalah poin-poin konfigurasi utama:

**Router9 (Kantor Pusat - Inter-VLAN Routing):**
```cisco
! Konfigurasi Inter-VLAN (contoh VLAN 10 & 20)
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.52.0.1 255.255.254.0
!
interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.52.2.1 255.255.255.0
! (dan seterusnya untuk VLAN 30, 40, 50)
!
! Konfigurasi WAN
interface Serial0/1/0
 ip address 10.52.3.233 255.255.255.252
!
! Static Route ke Cabang
ip route 10.52.3.192 255.255.255.224 10.52.3.234
