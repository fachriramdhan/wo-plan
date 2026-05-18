Berdasarkan analisis *Project Brief* dan spesifikasi teknis MES yang sudah kita matangkan, aplikasi ini secara total akan memiliki **6 Halaman Utama** (berdasarkan menu *sidebar*) dan **1 Halaman Detail**.

Berikut adalah rincian daftar halaman beserta rancangan *Layout*, navigasi, dan **Wireframe (ASCII/Teks)** lengkap untuk seluruh halaman agar siap Anda implementasikan menggunakan Bootstrap 5.

---

## 1. Daftar Halaman & Struktur Navigasi (*Sitemap*)

Semua halaman akan menggunakan struktur **Fixed Sidebar** di sebelah kiri dan **Top Navbar** untuk informasi pengguna.

* **Main Navigation (Sidebar):**
1. `DIJ Dashboard` (Halaman Analitik & OEE)
2. `DIJ Layout` (Halaman Utama Andon 20 Mesin)
3. `DIJ Timeline` (Visualisasi Produktivitas Waktu)
4. `DIJ Schedule` (Papan Kanban Penjadwalan WO)
5. `DIJ Report` (Halaman Filter & Ekspor Data)
6. `DIJ New Hire Dashboard` (Input Karyawan Sementara)


* **Sub-Page (Akses via Klik):**
7.  `Machine Detail Control Panel` (Diakses ketika salah satu mesin di *DIJ Layout* diklik)

---

## 2. Wireframe & Desain Antarmuka (UI/UX Plan)

### 📄 Layout Dasar Aplikasi (Template Bootstrap 5)

Semua halaman akan dibungkus oleh *layout standard* ini:

```
+-----------------------------------------------------------------------+
|  [Sidebar]       | [Top Navbar]  PT Mattel West Plant - Area DIJ      |
|  DIJ Dashboard   | User: Admin (Shift 1)             Date: 19-05-2026 |
|  DIJ Layout      +----------------------------------------------------+
|  DIJ Timeline    |                                                    |
|  DIJ Schedule    |               [ KONTEN HALAMAN UTAMA ]             |
|  DIJ Report      |                                                    |
|  DIJ New Hire    |                                                    |
+-----------------------------------------------------------------------+

```

---

### PAGE 1: DIJ Dashboard (OEE Analitik)

* **UI/UX Concept:** Menggunakan *Big Numbers* untuk KPI utama dan grafik untuk tren.

```
+-----------------------------------------------------------------------+
| [OEE: 85%] (Card Hijau) | [Availability: 90%] | [Performance: 95%]    |
| Total OEE Kumulatif     | Run: 432m | Down: 48m | Avg Cycle: 4.2s     |
+-----------------------------------------------------------------------+
| [Quality: 98.5%]        | [Top 5 Losses (Pareto Bar Chart)]           |
| Good: 12,500 | Rej: 180 | 1. Call Mechanic (DIJ002) : 120 Menit       |
| Target: 13,000          | 2. Changeover (DIJ005)    : 85 Menit        |
|                         | 3. Tinta Mampet           : 40 Menit        |
+-----------------------------------------------------------------------+

```

---

### PAGE 2: DIJ Layout (Andon Board 20 Mesin)

* **UI/UX Concept:** *Grid System* (4x5 atau 3x7). Setiap kotak adalah tombol besar yang responsif. Warna latar belakang berubah dinamis sesuai status.

```
+-----------------------------------------------------------------------+
| [ DIJ001 ] (Hijau)   | [ DIJ002 ] (Merah)   | [ DIJ003 ] (Oranye)  |
| Running              | Call Mechanic        | Changeover           |
| Toy: HW-001          | Toy: HW-002          | Toy: HW-003          |
+----------------------+----------------------+----------------------+
| [ DIJ004 ] (Kuning)  | [ DIJ005 ] (Biru Mda)| [ DIJ006 ] (Hitam)   |
| Out of Cycle         | Plan Downtime        | Machine Off          |
| Cycle: 6.5s (Std 4s) | Briefing Shift       | No Schedule          |
+----------------------+----------------------+----------------------+
| (... Dan seterusnya sampai DIJ020 ...)                              |
+-----------------------------------------------------------------------+

```

---

### PAGE 3: Machine Detail Control Panel (Sub-Page dari Layout)

* **UI/UX Concept:** Fokus pada operasi taktis. Tombol aksi dibuat berukuran minimal 44x44 piksel agar ramah layar sentuh (*touch-target*).

```
+-----------------------------------------------------------------------+
| KEMBALI KE LAYOUT | MESIN: DIJ001 [Status: RUNNING (Hijau)]           |
+-----------------------------------------------------------------------+
| [ ONGOING WORK ORDER ]                      | [ 3 TOMBOL AKSI UTAMA ] |
| WO ID       : WO-20260519-01                |                         |
| Toy No      : HW-Boneshaker (V2)            | +---------------------+ |
| Part Number : P-98271                       | |    [ DOWNTIME ]     | |
| Target      : 5,000 / Realisasi: 3,200      | +---------------------+ |
| Panel Code  : JIG-DIJ-A                     | |     [ REJECT ]      | |
|                                             | +---------------------+ |
| [ BUTTON: FINISH WORK ORDER ] (Warna Biru)  | |   [ WORK ORDER ]    | |
+---------------------------------------------+ +---------------------+ |
| [ UPCOMING QUEUE (ANTREAN JADWAL) ]                                   |
| Seq | WO ID            | Toy No       | Target | Part No   | Panel    |
| 2   | WO-20260519-05   | HW-Camaro67  | 4,000  | P-98272   | JIG-DIJ-A|
| 3   | WO-20260519-09   | HW-TwinMill  | 3,500  | P-98275   | JIG-DIJ-B|
+-----------------------------------------------------------------------+

```

* *UX Note untuk Pop-up Modals:* * Jika **[ Downtime ]** diklik, muncul *popup* pilihan: Operational, Call Mechanic, dll.
* Jika **[ Reject ]** diklik, muncul *popup* kalkulator angka cepat (+1, +5, +10) dan pilihan alasan (Mampet, Buram, Geser).



---

### PAGE 4: DIJ Schedule (Kanban Planning)

* **UI/UX Concept:** Kolom-kolom horisontal yang mewakili mesin dengan fitur *Scroll* ke kanan. Kartu WO dapat digeser (*drag*) antar mesin.

```
+-----------------------------------------------------------------------+
| [ TAMBAH WO BARU ]                                                    |
+-----------------------------------------------------------------------+
| [ KOLOM MESIN DIJ001 ]   | [ KOLOM MESIN DIJ002 ]   | [ KOLOM... ]    |
| +----------------------+ | +----------------------+ |                 |
| | WO-01: Ongoing       | | | WO-03: Ongoing       | |                 |
| | Toy: Boneshaker      | | | Toy: TwinMill        | |                 |
| +----------------------+ | +----------------------+ |                 |
| | WO-02: Queue (Seq 2) | | | (Kosong / No Queue)  | |                 |
| | Toy: '67 Camaro      | | |                      | |                 |
| +----------------------+ | +----------------------+ |                 |
+-----------------------------------------------------------------------+

```

---

### PAGE 5: DIJ Timeline

* **UI/UX Concept:** Visualisasi horizontal berbasis waktu (*Time-series bar*) untuk memantau efisiensi waktu kerja per mesin dalam 1 *shift*.

```
+-----------------------------------------------------------------------+
| Shift: 1 (07:00 - 15:00) | Filter Tanggal: [ 19-05-2026 ]             |
+-----------------------------------------------------------------------+
| DIJ001 | [HHH][HHH][HHH][MMM][HHH][HHH][AAA][HHH]                     |
| DIJ002 | [HHH][KKK][KKK][HHH][HHH][HHH][AAA][HHH]                     |
| DIJ003 | [BBB][BBB][BBB][BBB][BBB][BBB][BBB][BBB]                     |
+-----------------------------------------------------------------------+
| Keterangan: [H] Hijau-Run | [M] Merah-Breakdown | [K] Kuning-Downtime |
|             [A] Abu-Break | [B] Biru-No Schedule                      |
+-----------------------------------------------------------------------+

```

---

### PAGE 6: DIJ Report

* **UI/UX Concept:** Form filter yang bersih disandingkan dengan tombol ekspor dokumen yang kontras.

```
+-----------------------------------------------------------------------+
| Rentang Tanggal: [ Mulai ] s/d [ Selesai ]      Shift: [ Semua / 1/2/3 ]|
| Mesin: [ Semua Mesin / DIJ001-020 ]             [ FILTER DATA ]       |
+-----------------------------------------------------------------------+
| PILIHAN EKSPOR:  [ DOWNLOAD EXCEL (.XLSX) ]   [ DOWNLOAD PDF ]        |
+-----------------------------------------------------------------------+
| REKAP PREVIEW DATA:                                                   |
| Tanggal    | Mesin  | WO ID  | Total Cetak | Total Reject | OEE (%)   |
| 19-05-2026 | DIJ001 | WO-01  | 5,000       | 45           | 88.5%     |
| 19-05-2026 | DIJ002 | WO-03  | 3,200       | 120          | 72.1%     |
+-----------------------------------------------------------------------+

```

---

### PAGE 7: DIJ New Hire Dashboard (Data Karyawan Baru)

* **UI/UX Concept:** Menggunakan layout *Split-screen*. Sisi kiri untuk pendaftaran ID sementara, sisi kanan untuk melihat daftar nama yang aktif.

```
+-----------------------------------------------------------------------+
| [ INPUT ID KARYAWAN SEMENTARA ]   | [ DAFTAR KARYAWAN BARU AKTIF ]    |
|                                   |                                   |
| Scan/Ketik ID:                    | ID Sementara | Nama       | Shift |
| [ Mattel-NH-001             ]     | NH-001       | Ahmad J.   | 1     |
|                                   | NH-002       | Rian Kurnia| 2     |
| Nama Lengkap:                     |                                   |
| [ Ahmad Junaedi             ]     |                                   |
|                                   |                                   |
| Tanggal Mulai Kerja:              | [ SINKRONISASI KE HRMS PUSAT ]    |
| [ 19-05-2026                ]     | (Tekan jika ID asli sudah terbit) |
|                                   |                                   |
| [ SIMPAN DATA ]                   |                                   |
+-----------------------------------------------------------------------+

```

---

## 3. Strategi Transisi Desain ke Bootstrap 5

1. **Gunakan Utility Classes:** Optimalkan kelas bawaan seperti `bg-success` (Hijau), `bg-danger` (Merah), `bg-warning` (Kuning) untuk manajemen status Andon secara instan.
2. **Flexbox & Grid (`row`, `col-md-*`):** Pastikan halaman *DIJ Layout* menggunakan struktur grid yang fleksibel agar ketika dibuka di tablet operator (posisi *landscape*), susunan 20 mesin otomatis menyesuaikan lebar layar tanpa merusak estetika informasi.
3. **Persiapan AJAX Component:** Untuk tombol **[ Finish ]**, **[ Downtime ]**, dan **[ Reject ]**, antarmukanya harus mengeksekusi perintah di latar belakang melalui JavaScript tanpa memicu *force reload* satu halaman penuh, sehingga data performa mesin tidak mengalami jeda pembaruan data (*latency*).
