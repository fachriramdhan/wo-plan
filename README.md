# 🏭 DIJ Manufacturing Execution System (MES) & Work Order
**Location:** PT Mattel West Plant - Direct Ink Jet (DIJ) Area  
**Type:** Human-in-the-loop MES & Digital Andon Board  
**Version:** 1.0.0 (Master Plan)

## 📑 Daftar Isi
1. [Ringkasan Proyek](#1-ringkasan-proyek)
2. [Tech Stack](#2-tech-stack)
3. [Arsitektur Navigasi Utama](#3-arsitektur-navigasi-utama)
4. [Standar Kode Warna (Andon Status)](#4-standar-kode-warna-andon-status)
5. [Logika Bisnis & Alur Kerja](#5-logika-bisnis--alur-kerja)
6. [Struktur Database Inti](#6-struktur-database-inti)
7. [Fase Pengembangan](#7-fase-pengembangan)

---

## 1. Ringkasan Proyek
Proyek ini bertujuan membangun aplikasi *Manufacturing Execution System* (MES) dan manajemen *Work Order* (WO) berbasis web untuk memantau performa 20 mesin cetak Direct Ink Jet (DIJ) pencetak miniatur kendaraan (Hot Wheels) secara *real-time*. 

Sistem dirancang beroperasi penuh secara *software-based* (tanpa integrasi IoT langsung ke mesin) dan mengandalkan *input* taktis dari operator/supervisor di lantai produksi menggunakan perangkat PC lini atau tablet. Objektif utama sistem ini adalah menghitung metrik **OEE (Overall Equipment Effectiveness)** yang presisi dan meminimalkan *downtime* mesin.

---

## 2. Tech Stack
* **Backend:** ASP.NET Core (MVC / Web API)
* **Database:** Microsoft SQL Server
* **Frontend:** HTML5, CSS3, Bootstrap 5, JavaScript (jQuery/Ajax untuk fitur antarmuka dinamis seperti *Drag-and-Drop* dan *Modals*).

---

## 3. Arsitektur Navigasi Utama
Sistem menggunakan *Sidebar Navigation* dengan 6 modul utama:

1. **`DIJ Dashboard`**: Pusat analitik manajemen. Menampilkan metrik utama:
   * **OEE (%):** Kalkulasi efisiensi keseluruhan.
   * **Availability:** *Runtime*, *Uptime*, *Downtime*.
   * **Performance:** *Std Cycle Time*, *Average Cycle*, *Last Cycle*.
   * **Quality:** *Total Parts*, *Target*, *Good Parts*, *Reject Parts*.
   * **Top 5 Losses:** Pareto *downtime* tertinggi dan unit *reject* terbanyak.
2. **`DIJ Layout`**: *Andon Board* digital yang memetakan status 20 mesin secara *real-time* dalam bentuk kartu (Grid).
3. **`DIJ Timeline`**: Visualisasi *Gantt Chart* dari riwayat status setiap mesin selama 1 *shift* kerja.
4. **`DIJ Schedule`**: Papan antrean kerja (*Kanban Style*) untuk mengatur alokasi dan urutan *Work Order* ke setiap mesin.
5. **`DIJ Report`**: Modul pelaporan (*Export to Excel/PDF*) untuk rekap data produksi harian.
6. **`DIJ New Hire Dashboard`**: Data *repository* lokal untuk menampung *input* produksi dari karyawan baru (< 1 bulan) yang ID-nya belum tersinkronisasi penuh dengan sistem HRMS pusat.

---

## 4. Standar Kode Warna (Andon Status)
Pada halaman **DIJ Layout**, sistem menggunakan 20 kartu mesin (DIJ001 - DIJ020). Latar belakang kartu berubah secara dinamis berdasarkan status operasional yang dipicu (*triggered*) oleh operator:

| Status Mesin | Bootstrap Class / Hex Color | Indikator Visual | Deskripsi Kondisi |
| :--- | :--- | :--- | :--- |
| **Running** | `bg-success` | Hijau | Mesin beroperasi normal sesuai siklus produksi. |
| **Operational Downtime**| `bg-warning text-dark` | Kuning | Masalah operasional lini non-teknis. |
| **Out of Cycle** | `bg-warning text-danger`| Kuning (Teks Merah) | Mesin berjalan, namun waktu siklus melampaui standar. |
| **Call Mechanic** | `bg-danger text-white` | Merah | *Breakdown* mekanis/elektrik, butuh intervensi teknisi. |
| **Changeover** | `#fd7e14` | Oranye | Proses ganti jig/model (Set-up *Toy No* baru). |
| **External Constraints**| `#ffc107` (Opacity 75%)| Oranye Muda | Terhenti karena faktor pasokan dari departemen lain. |
| **Connectivity** | `bg-primary text-white` | Biru | Gangguan jaringan, *software* grafis, atau transfer data. |
| **Plan Downtime** | `bg-info text-dark` | Biru Muda | Berhenti terencana (misal: *briefing* awal *shift*, 5S). |
| **Break** | `bg-secondary text-white`| Abu-abu | Jam istirahat operator produksi. |
| **Planned Maintenance** | `bg-dark text-white` | Abu-abu Tua | Jadwal perawatan rutin (*Preventive Maintenance*). |
| **Machine Off / No Sched**| `bg-black text-white` | Hitam | Mesin dimatikan / tidak ada alokasi target dari *Planner*. |

---

## 5. Logika Bisnis & Alur Kerja

### 5.1. Manajemen Jadwal (DIJ Schedule)
* Menggunakan antarmuka papan kanban. Supervisor memindahkan (*drag-and-drop*) kartu *Work Order* ke dalam jalur/kolom spesifik mesin.
* Urutan (Sequence) WO dalam satu kolom mesin menentukan antrean mana yang berjalan lebih dulu.

### 5.2. Panel Kontrol Mesin (Machine Detail)
Saat satu kartu mesin di **DIJ Layout** diklik, antarmuka akan menampilkan panel kontrol dengan dua segmen data:
1. **Ongoing Work Order:** Data yang aktif dicetak (*WO ID, Toy No, Target, Part Number, Panel*).
2. **Queue (Antrean):** Daftar WO selanjutnya untuk mesin tersebut berdasarkan *DIJ Schedule*.

**Tombol Aksi Cepat (Quick Actions):**
* `[ Finish ]`: Menyelesaikan *Ongoing WO*. Sistem otomatis mengambil WO urutan teratas di *Queue* untuk dinaikkan statusnya menjadi *Ongoing*.
* `[ Downtime ]`: Membuka *modal* untuk mengganti status mesin menjadi salah satu dari kategori *downtime* (misal: memicu warna Merah/*Call Mechanic*). Mencatat *timestamp* detik itu sebagai awal kerugian waktu.
* `[ Reject ]`: Membuka *modal* input jumlah produk cacat (*scrap*) dan jenis kerusakannya tanpa harus menghentikan status *Running* mesin.
* `[ Work Order ]`: Menu navigasi untuk melihat detail instruksi teknis, SOP model, atau menata ulang prioritas antrean.

---

## 6. Struktur Database Inti
Desain Relasional SQL Server difokuskan pada integritas data sekuensial dan riwayat waktu (*Time-Series*).

* **`mst_Machines`:** Menyimpan 20 ID Mesin dan status terkininya.
* **`trn_WorkOrders`:** Data pekerjaan (ID, Toy No, Part No, Target, Status [Scheduled, Ongoing, Finished], SequenceNo).
* **`log_MachineStatus`:** Tabel paling krusial untuk kalkulasi *Availability*. Mencatat `MachineID`, `StatusCode`, `StartTime`, `EndTime`, dan `DurationInMinutes`.
* **`log_Rejects`:** Tabel historis input kualitas (WO ID, RejectQty, ReasonCode, Timestamp).
* **`tmp_NewHires`:** Tabel isolasi data produksi dari ID karyawan sementara.

---

## 7. Fase Pengembangan (Roadmap)

1. **Sprint 1: Fondasi & UI/UX**
   * *Setup* ASP.NET Core & SQL Server.
   * Pembuatan layout *Sidebar* dan implementasi skema warna *Andon* 20 Mesin menggunakan Bootstrap 5.
2. **Sprint 2: Core Engineering (Mesin & Jadwal)**
   * Pembuatan logika *DIJ Schedule* (Drag & Drop antrean).
   * Pembuatan panel *Machine Detail* dengan fungsionalitas auto-routing tombol `Finish`.
3. **Sprint 3: Aksi Lapangan & Logika Waktu**
   * Pengaktifan tombol *Downtime*, *Reject*, dan *Work Order*.
   * Membangun sistem *timestamp* otomatis di *backend* untuk mencegah manipulasi waktu oleh pengguna.
4. **Sprint 4: Analitik & Finalisasi**
   * Pembuatan kalkulasi OEE dinamis di *DIJ Dashboard*.
   * Pengembangan grafik *DIJ Timeline*.
   * Uji coba terintegrasi (*User Acceptance Test*) dengan perangkat *tablet* di lini DIJ West Plant.
