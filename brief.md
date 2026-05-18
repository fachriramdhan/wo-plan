# 📄 DETAILED PROJECT BRIEF & SYSTEM SPECIFICATION
**Project Name:** DIJ Manufacturing Execution System (MES) & Digital Andon
**Location:** PT Mattel West Plant - Area Direct Ink Jet (DIJ)
**Platform:** Web Application (Optimized for PC Line & Tablet)
**Document Version:** 1.1.0
**Date:** 19 Mei 2026

---

## 1. PENDAHULUAN
### 1.1. Latar Belakang
Area produksi Direct Ink Jet (DIJ) untuk produk Hot Wheels membutuhkan sistem pemantauan terpusat yang *real-time* untuk mengukur efisiensi mesin, mengelola antrean pekerjaan (*Work Order*), dan merespons kerusakan mesin secara instan. 

### 1.2. Tujuan Sistem
Membangun aplikasi *pure software-based* (tanpa integrasi IoT) yang mengandalkan input taktis manusia (Operator/Supervisor) untuk menghasilkan kalkulasi metrik manufaktur yang presisi, mengurangi *downtime*, dan mendigitalisasi pelaporan.

### 1.3. Tech Stack
* **Framework:** ASP.NET Core MVC / Web API (.NET 8 atau terbaru)
* **Database:** Microsoft SQL Server
* **Frontend:** HTML5, CSS3, Bootstrap 5, JavaScript (AJAX, jQuery UI untuk Drag-and-Drop)
* **Data Visualization:** Chart.js atau ApexCharts (untuk Dashboard & Timeline)

---

## 2. HAK AKSES PENGGUNA (USER ROLES)
Sistem memiliki kontrol akses berbasis peran (*Role-Based Access Control / RBAC*) untuk menjaga integritas data:

| Peran (Role) | Hak Akses & Kemampuan Utama |
| :--- | :--- |
| **Operator Lini** | Akses ke *DIJ Layout*. Bisa memicu *Downtime*, input *Reject*, dan klik *Finish* pada WO aktif. |
| **Supervisor / Planner** | Akses penuh ke *DIJ Schedule* (Kanban Board), *Report*, dan *Timeline*. Bisa memindahkan urutan mesin dan menambah WO baru. |
| **Maintenance / Teknisi**| Menerima notifikasi Andon, mengubah status *Call Mechanic* menjadi *Running* setelah perbaikan selesai. |
| **Plant Manager / Admin** | Akses semua modul termasuk *DIJ Dashboard* (OEE) dan manajemen *New Hire*. |

---

## 3. SPESIFIKASI FUNGSIONAL SISTEM (MODUL)

### 3.1. Modul DIJ Dashboard (Eksekutif)
Halaman ini menampilkan analitik kumulatif dari 20 mesin.
* **Kalkulasi OEE:** Dihitung secara dinamis dengan rumus: $OEE = Availability \times Performance \times Quality$
* **Availability Panel:** Menampilkan agregasi *Uptime*, *Runtime*, dan *Downtime* (dalam menit).
* **Performance Panel:** Menampilkan *Std Cycle Time* (dari Master Data) berbanding *Average Cycle* harian.
* **Quality Panel:** Visualisasi Donut Chart untuk persentase *Good Parts* vs *Reject Parts*.
* **Top 5 Losses:** Bar chart Pareto yang menarik data dari tabel `log_MachineStatus` dan `log_Rejects`.

### 3.2. Modul DIJ Layout (Digital Andon)
Papan antarmuka utama di lantai produksi yang menampilkan 20 Kartu Mesin (DIJ001 - DIJ020).
* **Real-time Color Coded:** Warna latar belakang kartu berubah secara asinkron (menggunakan AJAX/SignalR) sesuai status operasional.
    * Hijau: *Running*
    * Kuning: *Operational Downtime* / *Out of Cycle* (Teks Merah)
    * Merah: *Call Mechanic*
    * Oranye: *Changeover* / *External Constraints* (Oranye Muda)
    * Biru: *Connectivity* / *Plan Downtime* (Biru Muda)
    * Abu-abu: *Break* / *Planned Maintenance* (Abu Tua)
    * Hitam: *Machine Off / No Schedule*
* **On-Click Action:** Menekan kartu mesin akan membuka Halaman Detail Mesin.

### 3.3. Modul Detail Mesin (Machine Panel)
Panel kontrol operasional untuk operator di depan mesin.
* **Ongoing WO Card:** Menampilkan instruksi saat ini (WO ID, Toy No, Target, Part Number, Panel).
* **Tombol Finish:** Mengakhiri *Ongoing WO*. Sistem otomatis mengambil WO antrean ke-1 untuk dinaikkan menjadi *Ongoing*.
* **Queue Table:** Tabel *read-only* (bagi operator) yang menampilkan daftar tunggu WO untuk mesin tersebut.
* **Tombol Downtime:** Membuka *Modal Window*. Operator memilih alasan henti mesin $\rightarrow$ Sistem mencatat `StartTime` dan mengubah warna kartu mesin.
* **Tombol Reject:** Membuka *Modal Window*. Operator memasukkan kuantitas (Qty) dan kategori cacat $\rightarrow$ Data ditambahkan ke *Quality Panel* tanpa menghentikan status *Running*.
* **Tombol Work Order:** Membuka *Modal* pop-up berisi *Standard Operating Procedure* (SOP) atau catatan khusus untuk Toy No yang sedang dikerjakan.

### 3.4. Modul DIJ Schedule (Production Planning)
Papan Kanban interaktif untuk Supervisor.
* **Kolom Mesin:** Terdapat 20 kolom vertikal merepresentasikan mesin.
* **Kartu Pekerjaan (Job Cards):** Representasi *Work Order*.
* **Drag-and-Drop:** Supervisor dapat menarik Job Card antar kolom mesin atau mengubah urutan atas-bawah dalam satu kolom.
* **Auto-Sequence Update:** Saat Job Card dipindahkan, *backend* langsung memperbarui kolom `SequenceNo` dan `MachineID` pada tabel database `trn_WorkOrders`.

### 3.5. Modul DIJ Timeline & Report
* **Timeline:** Menampilkan *Gantt Chart* horizontal per mesin dalam siklus 24 jam. Warna balok disesuaikan dengan kode warna Andon (misal: balok hijau jam 08.00-10.00, balok merah jam 10.00-10.15).
* **Report:** Filter tanggal dan *Shift*. Tombol ekspor ke `.xlsx` (Excel) menggunakan *library* seperti EPPlus.

### 3.6. Modul DIJ New Hire Dashboard
* Form input sederhana (ID Sementara, Nama, Shift).
* Data produksi yang diinput oleh operator dari modul ini akan disimpan dengan label `[New Hire]` hingga Admin melakukan sinkronisasi ID definitif dari HRMS Pusat.

---

## 4. STRUKTUR DATABASE RELASIONAL (ERD PLAN)

Berikut adalah entitas utama dan atribut esensialnya:

**1. `mst_Machines` (Master Mesin)**
* `MachineID` (PK, INT)
* `MachineCode` (VARCHAR, ex: DIJ001)
* `CurrentStatus` (VARCHAR, ex: Running, Breakdown)

**2. `trn_WorkOrders` (Transaksi Pekerjaan)**
* `WOID` (PK, INT)
* `MachineID` (FK, INT) - Mesin yang ditugaskan
* `ToyNo` (VARCHAR) - Kode model Hot Wheels
* `TargetQty` (INT) - Target panel/unit
* `SequenceNo` (INT) - Urutan antrean di mesin
* `StatusWO` (VARCHAR) - Scheduled, Ongoing, Finished

**3. `log_MachineStatus` (Pencatat Waktu Andon)**
* `LogID` (PK, BIGINT)
* `MachineID` (FK, INT)
* `StatusCode` (VARCHAR) - Sesuai daftar Andon
* `StartTime` (DATETIME) - Dicatat otomatis oleh server
* `EndTime` (DATETIME) - Boleh NULL jika masih berlangsung
* `DurationMinutes` (INT) - Kalkulasi EndTime - StartTime

**4. `log_Rejects` (Pencatat Kualitas)**
* `RejectID` (PK, BIGINT)
* `WOID` (FK, INT)
* `RejectQty` (INT)
* `ReasonCode` (VARCHAR)

---

## 5. NON-FUNCTIONAL REQUIREMENTS (NFR)
* **Kinerja (*Performance*):** Waktu muat halaman *DIJ Layout* tidak boleh lebih dari 2 detik. Pembaruan status kartu mesin via AJAX maksimal setiap 5 detik.
* **Usability:** Tombol pada modal *Downtime* dan *Reject* harus berukuran sentuh minimal $44 \times 44$ pixel (standar *touch-target*) agar mudah ditekan operator yang memakai sarung tangan.
* **Keamanan:** Mencegah manipulasi waktu (*timestamp*); jam mulai dan selesai harus diambil dari waktu *Server* (GETDATE() SQL Server), bukan perangkat klien.
