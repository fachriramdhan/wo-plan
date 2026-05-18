# 🏭 DIJ Manufacturing Execution System (MES) & Digital Andon

## Master Project Brief, Technical Blueprint & Development Guideline

**Project Name:** DIJ Manufacturing Execution System (MES) & Digital Andon Board
**Plant Area:** PT Mattel West Plant – Direct Ink Jet (DIJ) Production Area
**System Type:** Human-in-the-loop Manufacturing Execution System (MES)
**Platform:** Web Application (Desktop Line PC & Industrial Tablet Optimized)
**Document Version:** 1.0 Master Blueprint
**Prepared Date:** 19 Mei 2026
**Framework Standard:** ASP.NET Core MVC / Web API (.NET 8)
**Database:** Microsoft SQL Server

---

# 📑 TABLE OF CONTENTS

1. Executive Summary
2. Business Background
3. Manufacturing Theory & MES Concept
4. Project Objectives
5. Scope of System
6. Core Business Process
7. Production Flow Concept
8. User Roles & Access Control
9. System Modules Overview
10. Detailed Functional Specifications
11. Machine Status & Andon Standard
12. OEE Theory & Calculation Logic
13. Availability Logic
14. Performance Logic
15. Quality Logic
16. Work Order Lifecycle
17. Downtime Management Concept
18. Reject Management Concept
19. Scheduling & Queueing Logic
20. Timeline & Historical Logging
21. System Architecture
22. Technical Stack & Dependencies
23. Frontend Technical Architecture
24. Backend Technical Architecture
25. Database Architecture
26. Detailed Database Design
27. Entity Relationship Design (ERD)
28. API Design Standard
29. Naming Convention Standard
30. UI/UX Design Guideline
31. Responsive Tablet Standard
32. Real-Time Update Strategy
33. Security Standard
34. Audit Trail & Logging
35. Error Handling Standard
36. Performance Optimization Strategy
37. Reporting Architecture
38. Excel Export Architecture
39. Deployment Architecture
40. Infrastructure Recommendation
41. Testing Strategy
42. Development Sprint Roadmap
43. Coding Standard Recommendation
44. Folder Structure Recommendation
45. Future Enhancement Plan
46. Implementation Notes
47. Conclusion

---

# 1. EXECUTIVE SUMMARY

DIJ MES (Manufacturing Execution System) adalah sistem digital berbasis web yang dirancang untuk memonitor, mengontrol, dan mendokumentasikan aktivitas produksi pada area Direct Ink Jet (DIJ) di PT Mattel West Plant.

Sistem ini berfungsi sebagai:

* Digital Andon Board
* Work Order Management System
* Machine Monitoring System
* Downtime Logging System
* Production Performance Analytics System
* OEE Calculation Engine
* Production Timeline Monitoring System
* Shift Reporting System

Berbeda dengan MES berbasis IoT penuh, sistem ini menggunakan pendekatan:

> Human-in-the-loop manufacturing execution.

Artinya:

* Status mesin dipicu operator.
* Downtime dicatat supervisor/operator.
* Reject diinput manual.
* Finish WO dilakukan operator.
* Sistem berfungsi sebagai pusat validasi, kalkulasi, dan historisasi data.

Tujuan utama sistem:

* Mengurangi downtime.
* Mempercepat respon maintenance.
* Menyediakan data OEE real-time.
* Menghilangkan pencatatan manual berbasis kertas.
* Memudahkan analisa loss produksi.
* Membantu supervisor melakukan scheduling produksi.
* Menjadi pondasi digitalisasi area DIJ.

---

# 2. BUSINESS BACKGROUND

Area DIJ merupakan area pencetakan Direct Ink Jet untuk produk Hot Wheels.

Karakteristik area:

* Terdapat 20 mesin DIJ.
* Produksi berjalan berdasarkan Work Order.
* Setiap mesin dapat memiliki antrean pekerjaan.
* Perubahan model membutuhkan changeover.
* Banyak downtime bersifat operasional.
* Dibutuhkan respon cepat terhadap breakdown.
* Produksi berjalan berdasarkan shift.

Masalah utama sebelum sistem:

| Masalah                         | Dampak               |
| ------------------------------- | -------------------- |
| Pencatatan manual downtime      | Data tidak akurat    |
| Tidak ada histori real-time     | Sulit analisa loss   |
| Monitoring mesin manual         | Respon lambat        |
| OEE dihitung manual             | Tidak realtime       |
| Antrean WO tidak terstruktur    | Scheduling kacau     |
| Tidak ada timeline status mesin | Sulit audit produksi |

Sistem DIJ MES dibuat untuk menyelesaikan seluruh permasalahan tersebut.

---

# 3. MANUFACTURING THEORY & MES CONCEPT

## 3.1 Apa Itu MES

MES (Manufacturing Execution System) adalah sistem yang berada di antara:

* ERP (Enterprise Resource Planning)
* Shopfloor Production

MES bertugas:

* Mengontrol produksi.
* Mengumpulkan data produksi.
* Memantau mesin.
* Mengelola work order.
* Mengukur efisiensi.

## 3.2 Human-in-the-loop MES

Sistem ini tidak menggunakan sensor otomatis.

Semua event dipicu manusia:

| Aktivitas          | Pelaku              |
| ------------------ | ------------------- |
| Start WO           | Operator            |
| Finish WO          | Operator            |
| Input downtime     | Operator/Supervisor |
| Input reject       | Operator            |
| Schedule WO        | Supervisor          |
| Recovery breakdown | Maintenance         |

## 3.3 Andon Concept

Andon adalah sistem visual management.

Tujuan:

* Memberi status mesin secara cepat.
* Memudahkan identifikasi masalah.
* Mempercepat respon.

Implementasi:

* Kartu mesin berwarna.
* Warna berubah berdasarkan status.
* Status update real-time.

---

# 4. PROJECT OBJECTIVES

## 4.1 Operational Objectives

* Mengurangi downtime mesin.
* Meningkatkan OEE.
* Mempercepat respon maintenance.
* Menstandarisasi pencatatan produksi.
* Mempercepat pengambilan keputusan supervisor.

## 4.2 Technical Objectives

* Membangun sistem scalable.
* Mendukung update real-time.
* Mendukung tablet industrial.
* Memastikan data timestamp valid.
* Menyimpan histori produksi lengkap.

## 4.3 Long-Term Objectives

* Menjadi fondasi Smart Factory.
* Menjadi basis integrasi IoT di masa depan.
* Menjadi pusat data manufaktur DIJ.

---

# 5. SCOPE OF SYSTEM

## Included Scope

* Digital Andon
* Work Order Scheduling
* Downtime Logging
* Reject Logging
* OEE Calculation
* Timeline Monitoring
* Shift Reporting
* Excel/PDF Export
* New Hire Temporary Repository

## Excluded Scope

* PLC Integration
* Direct Machine Sensor
* SAP Integration
* Barcode Automation
* AI Prediction
* Automatic Cycle Detection

---

# 6. CORE BUSINESS PROCESS

## 6.1 Production Flow

1. Supervisor membuat WO.
2. Supervisor menjadwalkan WO.
3. WO masuk antrean mesin.
4. Mesin menjalankan WO.
5. Operator input downtime bila terjadi masalah.
6. Operator input reject bila ada defect.
7. Operator klik Finish saat selesai.
8. Sistem otomatis menarik queue berikutnya.
9. Data masuk dashboard analytics.
10. Report dapat diexport.

---

# 7. PRODUCTION FLOW CONCEPT

## Shift-Based Production

Sistem berbasis shift:

| Shift   | Jam           |
| ------- | ------------- |
| Shift 1 | 07:00 - 15:00 |
| Shift 2 | 15:00 - 23:00 |
| Shift 3 | 23:00 - 07:00 |

## Production Cycle

Setiap WO memiliki:

* Target Qty
* Standard Cycle Time
* Running Time
* Reject Count
* Downtime

---

# 8. USER ROLES & ACCESS CONTROL

## 8.1 Operator Lini

Hak akses:

* Melihat DIJ Layout
* Input downtime
* Input reject
* Finish WO
* Melihat queue mesin

Tidak dapat:

* Mengubah jadwal WO
* Menghapus histori
* Mengubah master data

## 8.2 Supervisor / Planner

Hak akses:

* Full schedule management
* Drag and drop WO
* Add/edit WO
* View reports
* View timeline
* Monitoring produksi

## 8.3 Maintenance

Hak akses:

* Melihat Call Mechanic
* Mengubah status recovery
* Melihat histori breakdown

## 8.4 Plant Manager / Admin

Hak akses:

* Full access
* User management
* Dashboard analytics
* OEE management
* Report access
* Master data access

---

# 9. SYSTEM MODULES OVERVIEW

| Modul          | Fungsi                        |
| -------------- | ----------------------------- |
| DIJ Dashboard  | OEE analytics                 |
| DIJ Layout     | Digital Andon                 |
| DIJ Timeline   | Timeline histori status       |
| DIJ Schedule   | Kanban scheduling             |
| DIJ Report     | Reporting & export            |
| DIJ New Hire   | Temporary employee repository |
| Machine Detail | Machine operational control   |

---

# 10. DETAILED FUNCTIONAL SPECIFICATIONS

# 10.1 DIJ Dashboard

## Fungsi

Pusat analytics produksi.

## KPI Utama

* OEE
* Availability
* Performance
* Quality
* Total Reject
* Downtime
* Runtime
* Top 5 Losses

## Visual Components

* KPI Cards
* Pareto Chart
* Donut Chart
* Bar Chart
* Trend Graph

## Refresh Interval

* Auto refresh setiap 5 detik.

---

# 10.2 DIJ Layout

## Fungsi

Digital Andon real-time.

## Jumlah Mesin

20 Mesin:

* DIJ001
* DIJ002
* DIJ003
* ...
* DIJ020

## Card Content

Setiap kartu menampilkan:

* Machine Code
* Current Status
* Current WO
* Toy No
* Current Cycle
* Status Color

## Interaction

Klik kartu membuka:

* Machine Detail Panel

---

# 10.3 Machine Detail Control Panel

## Fungsi

Operational tactical control.

## Area Utama

### Ongoing WO

Menampilkan:

* WO ID
* Toy No
* Part Number
* Target
* Actual Production
* Panel/Jig

### Queue Table

Menampilkan antrean berikutnya.

### Action Buttons

| Tombol     | Fungsi                |
| ---------- | --------------------- |
| Finish     | Menyelesaikan WO      |
| Downtime   | Input status downtime |
| Reject     | Input reject          |
| Work Order | SOP dan notes         |

---

# 10.4 DIJ Schedule

## Fungsi

Scheduling produksi.

## Konsep

Kanban board.

## Fitur

* Drag and drop WO
* Change sequence
* Change machine assignment
* Auto update sequence

## Rules

* Hanya 1 Ongoing WO per mesin.
* Sequence harus unik per mesin.
* Queue bersifat FIFO berdasarkan sequence.

---

# 10.5 DIJ Timeline

## Fungsi

Visualisasi histori status mesin.

## Data Source

`log_MachineStatus`

## Warna Timeline

Mengikuti Andon color.

## Time Scale

* 24 jam
* Per shift
* Per hari

---

# 10.6 DIJ Report

## Fungsi

Reporting produksi.

## Filter

* Date range
* Shift
* Machine
* WO

## Export

* XLSX
* PDF

---

# 10.7 DIJ New Hire Dashboard

## Fungsi

Repository operator baru.

## Tujuan

Mengisolasi data operator yang belum sinkron HRMS.

---

# 11. MACHINE STATUS & ANDON STANDARD

| Status               | Color          | Kategori         |
| -------------------- | -------------- | ---------------- |
| Running              | Hijau          | Production       |
| Operational Downtime | Kuning         | Loss             |
| Out of Cycle         | Kuning + Merah | Performance Loss |
| Call Mechanic        | Merah          | Breakdown        |
| Changeover           | Oranye         | Setup            |
| External Constraints | Oranye Muda    | External         |
| Connectivity         | Biru           | IT Issue         |
| Plan Downtime        | Biru Muda      | Planned          |
| Break                | Abu-Abu        | Planned          |
| Planned Maintenance  | Abu Gelap      | Maintenance      |
| Machine Off          | Hitam          | No Production    |

---

# 12. OEE THEORY & CALCULATION LOGIC

OEE dihitung menggunakan formula:

genui{"math_block_widget_always_prefetch_v2":{"content":"OEE = Availability \times Performance \times Quality"}}

## OEE Purpose

Mengukur efektivitas total mesin.

## Ideal OEE

| OEE    | Kategori         |
| ------ | ---------------- |
| >85%   | World Class      |
| 60-85% | Good             |
| <60%   | Need Improvement |

---

# 13. AVAILABILITY LOGIC

Availability mengukur seberapa lama mesin benar-benar berjalan.

Formula:

genui{"math_block_widget_always_prefetch_v2":{"content":"Availability = \frac{Runtime}{Planned\ Production\ Time}"}}

## Runtime

Runtime = Planned Production Time - Downtime

## Downtime Included

* Call Mechanic
* Operational Downtime
* Connectivity
* External Constraints

## Downtime Excluded

* Break
* Planned Maintenance
* Planned Downtime

---

# 14. PERFORMANCE LOGIC

Performance mengukur kecepatan produksi dibanding ideal.

Formula:

genui{"math_block_widget_always_prefetch_v2":{"content":"Performance = \frac{Ideal\ Cycle\ Time \times Total\ Count}{Runtime}"}}

## Out of Cycle

Mesin dianggap Out of Cycle bila:

Actual Cycle > Standard Cycle

---

# 15. QUALITY LOGIC

Quality mengukur persentase produk bagus.

Formula:

genui{"math_block_widget_always_prefetch_v2":{"content":"Quality = \frac{Good\ Parts}{Total\ Parts}"}}

## Reject Categories

* Blur Print
* Misalignment
* Ink Smudge
* Missing Print
* Double Print
* Color Issue

---

# 16. WORK ORDER LIFECYCLE

## WO Status

| Status    | Meaning           |
| --------- | ----------------- |
| Scheduled | Menunggu antrean  |
| Ongoing   | Sedang diproduksi |
| Finished  | Selesai           |
| Cancelled | Dibatalkan        |

## WO Flow

Scheduled → Ongoing → Finished

## Finish Logic

Saat Finish:

1. WO aktif menjadi Finished.
2. Timestamp finish dicatat.
3. Queue sequence #1 menjadi Ongoing.
4. Machine status kembali Running.

---

# 17. DOWNTIME MANAGEMENT CONCEPT

## Tujuan

* Mengukur loss.
* Mengukur breakdown.
* Menjadi basis Pareto.

## Downtime Rules

### Start Downtime

Saat operator klik downtime:

* Status mesin berubah.
* Log baru dibuat.
* StartTime dicatat server.

### End Downtime

Saat recovery:

* EndTime dicatat.
* Duration dihitung otomatis.

### Timestamp Security

Timestamp wajib dari:

* SQL GETDATE()
* Server DateTime

Bukan:

* Browser time
* Client time

---

# 18. REJECT MANAGEMENT CONCEPT

Reject tidak menghentikan mesin.

## Reject Input

Operator mengisi:

* Qty reject
* Reason
* Notes optional

## Impact

Reject mempengaruhi:

* Quality
* OEE
* Pareto defect

---

# 19. SCHEDULING & QUEUEING LOGIC

## Queue Principle

FIFO by Sequence.

## Drag and Drop Rules

Saat WO dipindah:

* MachineID berubah.
* SequenceNo direorder.
* Queue refresh.

## Constraint

Tidak boleh:

* Duplicate sequence.
* Multiple ongoing WO.

---

# 20. TIMELINE & HISTORICAL LOGGING

Timeline dibangun dari:

`log_MachineStatus`

## Timeline Segments

* StartTime
* EndTime
* Status
* Duration

## Visualization

Horizontal Gantt-style timeline.

---

# 21. SYSTEM ARCHITECTURE

## Architecture Type

Three-layer architecture.

## Layers

### Presentation Layer

* Razor Views
* Bootstrap
* JavaScript

### Business Layer

* ASP.NET Core Services
* OEE Engine
* Scheduling Logic

### Data Layer

* SQL Server
* Entity Framework Core

---

# 22. TECHNICAL STACK & DEPENDENCIES

## Backend

* ASP.NET Core MVC
* ASP.NET Core Web API
* Entity Framework Core
* SignalR

## Frontend

* HTML5
* CSS3
* Bootstrap 5
* jQuery
* jQuery UI
* AJAX

## Charts

* ApexCharts
* Chart.js

## Database

* Microsoft SQL Server 2022

## Export Libraries

* EPPlus
* QuestPDF

---

# 23. FRONTEND TECHNICAL ARCHITECTURE

## UI Framework

Bootstrap 5.

## Frontend Philosophy

* Fast rendering
* Minimal reload
* Touch optimized
* High readability

## Layout Standard

* Fixed sidebar
* Top navbar
* Responsive content

---

# 24. BACKEND TECHNICAL ARCHITECTURE

## Pattern

Recommended:

* Clean Architecture
* Repository Pattern
* Service Layer Pattern

## Main Components

| Component    | Function          |
| ------------ | ----------------- |
| Controllers  | HTTP endpoints    |
| Services     | Business logic    |
| Repositories | Database access   |
| DTOs         | Data transfer     |
| Middleware   | Pipeline handling |

---

# 25. DATABASE ARCHITECTURE

## Database Type

Relational SQL Server.

## Database Goals

* Strong consistency
* Fast query
* Historical integrity
* Time-series capability

---

# 26. DETAILED DATABASE DESIGN

# 26.1 mst_Machines

| Column        | Type         |
| ------------- | ------------ |
| MachineID     | INT PK       |
| MachineCode   | VARCHAR(20)  |
| MachineName   | VARCHAR(100) |
| CurrentStatus | VARCHAR(50)  |
| CurrentWOID   | INT NULL     |
| IsActive      | BIT          |

---

# 26.2 trn_WorkOrders

| Column       | Type          |
| ------------ | ------------- |
| WOID         | INT PK        |
| WONumber     | VARCHAR(50)   |
| MachineID    | INT FK        |
| ToyNo        | VARCHAR(50)   |
| PartNumber   | VARCHAR(50)   |
| TargetQty    | INT           |
| ActualQty    | INT           |
| RejectQty    | INT           |
| SequenceNo   | INT           |
| StatusWO     | VARCHAR(30)   |
| StdCycleTime | DECIMAL(10,2) |
| CreatedDate  | DATETIME      |
| StartDate    | DATETIME      |
| FinishDate   | DATETIME      |

---

# 26.3 log_MachineStatus

| Column          | Type          |
| --------------- | ------------- |
| LogID           | BIGINT PK     |
| MachineID       | INT FK        |
| WOID            | INT FK        |
| StatusCode      | VARCHAR(50)   |
| StartTime       | DATETIME      |
| EndTime         | DATETIME NULL |
| DurationMinutes | INT           |
| Remarks         | VARCHAR(255)  |
| CreatedBy       | VARCHAR(100)  |

---

# 26.4 log_Rejects

| Column     | Type         |
| ---------- | ------------ |
| RejectID   | BIGINT PK    |
| WOID       | INT FK       |
| RejectQty  | INT          |
| ReasonCode | VARCHAR(100) |
| Notes      | VARCHAR(255) |
| Timestamp  | DATETIME     |
| CreatedBy  | VARCHAR(100) |

---

# 26.5 mst_Users

| Column       | Type         |
| ------------ | ------------ |
| UserID       | INT PK       |
| Username     | VARCHAR(100) |
| PasswordHash | VARCHAR(MAX) |
| RoleCode     | VARCHAR(50)  |
| FullName     | VARCHAR(150) |
| IsActive     | BIT          |

---

# 26.6 tmp_NewHires

| Column         | Type         |
| -------------- | ------------ |
| TempID         | INT PK       |
| TempEmployeeID | VARCHAR(50)  |
| FullName       | VARCHAR(150) |
| ShiftCode      | VARCHAR(10)  |
| JoinDate       | DATETIME     |
| IsSynced       | BIT          |

---

# 27. ENTITY RELATIONSHIP DESIGN (ERD)

## Main Relationships

* One Machine → Many Work Orders
* One Work Order → Many Reject Logs
* One Machine → Many Machine Status Logs
* One User → Many Activity Logs

---

# 28. API DESIGN STANDARD

## REST API Convention

### Example

GET:

/api/machines

POST:

/api/workorders

PUT:

/api/machines/{id}/status

DELETE:

/api/workorders/{id}

## Response Standard

```json
{
  "success": true,
  "message": "Data updated successfully",
  "data": {}
}
```

---

# 29. NAMING CONVENTION STANDARD

## SQL Tables

| Prefix | Meaning        |
| ------ | -------------- |
| mst_   | Master         |
| trn_   | Transaction    |
| log_   | Historical log |
| tmp_   | Temporary      |

## C# Naming

| Type          | Convention |
| ------------- | ---------- |
| Class         | PascalCase |
| Method        | PascalCase |
| Variable      | camelCase  |
| Private field | _camelCase |

---

# 30. UI/UX DESIGN GUIDELINE

## Design Principles

* High readability
* Minimal click
* Large touch buttons
* Color clarity
* Fast navigation

## Touch Standard

Minimum touch target:

44x44 px.

## Color Priority

* Red = Critical
* Yellow = Warning
* Green = Normal

---

# 31. RESPONSIVE TABLET STANDARD

## Target Devices

* Industrial Tablet
* 10 inch tablet
* Line PC
* Desktop supervisor

## Orientation

Landscape priority.

---

# 32. REAL-TIME UPDATE STRATEGY

## Recommended Technology

SignalR.

## Alternative

AJAX polling setiap 5 detik.

## Real-Time Components

* Machine status
* Dashboard KPI
* Queue update
* Timeline update

---

# 33. SECURITY STANDARD

## Authentication

ASP.NET Identity.

## Authorization

RBAC.

## Security Rules

* No client timestamp.
* No direct DB access.
* Validate all input.
* Use anti-forgery token.
* Use password hashing.

---

# 34. AUDIT TRAIL & LOGGING

## Audit Requirements

Semua critical action wajib dicatat.

## Logged Activities

* WO Finish
* Downtime change
* Reject input
* Schedule move
* Login/logout

---

# 35. ERROR HANDLING STANDARD

## User Error

Friendly notification.

## System Error

Logged internally.

## Recommended

Use middleware global exception handler.

---

# 36. PERFORMANCE OPTIMIZATION STRATEGY

## Target

DIJ Layout load:

< 2 detik.

## Optimization

* Lazy loading
* SQL indexing
* Minified assets
* Asynchronous AJAX
* Query optimization

---

# 37. REPORTING ARCHITECTURE

## Reports

* Daily production
* Shift report
* Downtime report
* OEE report
* Reject report

---

# 38. EXCEL EXPORT ARCHITECTURE

## Library

EPPlus.

## Export Features

* Auto filter
* Styling
* Summary section
* Multi-sheet report

---

# 39. DEPLOYMENT ARCHITECTURE

## Hosting Recommendation

* Windows Server
* IIS
* SQL Server

## Environment

| Environment | Purpose         |
| ----------- | --------------- |
| Development | Developer local |
| Staging     | UAT             |
| Production  | Live plant      |

---

# 40. INFRASTRUCTURE RECOMMENDATION

## Minimum Server Spec

| Component | Spec                |
| --------- | ------------------- |
| CPU       | 8 Core              |
| RAM       | 16 GB               |
| Storage   | SSD 512 GB          |
| OS        | Windows Server 2022 |

---

# 41. TESTING STRATEGY

## Testing Types

| Test             | Purpose               |
| ---------------- | --------------------- |
| Unit Test        | Logic validation      |
| Integration Test | API validation        |
| UAT              | Production simulation |
| Load Test        | Multi-user testing    |

---

# 42. DEVELOPMENT SPRINT ROADMAP

# Sprint 1 – Foundation

* Setup project
* Authentication
* Sidebar layout
* Bootstrap UI
* Master machine

# Sprint 2 – Scheduling Engine

* Work order module
* Kanban drag-drop
* Queue logic

# Sprint 3 – Machine Operations

* Finish WO
* Downtime modal
* Reject modal
* Logging engine

# Sprint 4 – Analytics

* Dashboard
* OEE engine
* Timeline
* Reporting

# Sprint 5 – Stabilization

* UAT
* Bug fixing
* Performance tuning
* Production deployment

---

# 43. CODING STANDARD RECOMMENDATION

## Principles

* Clean code
* SOLID principles
* Reusable services
* Thin controller
* Strong DTO validation

## Recommended Practices

* Avoid business logic in controller.
* Use async/await.
* Separate view models.
* Centralize constants.

---

# 44. FOLDER STRUCTURE RECOMMENDATION

```text
src/
 ├── Controllers/
 ├── Services/
 ├── Repositories/
 ├── Models/
 ├── DTOs/
 ├── Data/
 ├── Views/
 ├── wwwroot/
 ├── Middleware/
 ├── Helpers/
 └── SignalR/
```

---

# 45. FUTURE ENHANCEMENT PLAN

## Phase 2 Possibilities

* PLC integration
* Barcode scanning
* QR work order
* Real IoT cycle detection
* Predictive maintenance
* AI downtime prediction
* Mobile app
* SAP integration
* Power BI integration

---

# 46. IMPLEMENTATION NOTES

## Important Priority

Prioritaskan:

1. Data integrity
2. Timestamp accuracy
3. Real-time status
4. Simplicity for operator
5. Fast UI response

## Avoid

* Complex operator workflow
* Too many clicks
* Heavy page reload
* Manual timestamp input

---

# 47. CONCLUSION

DIJ MES & Digital Andon merupakan sistem manufaktur berbasis software yang dirancang untuk menjadi pusat kontrol operasional area Direct Ink Jet PT Mattel West Plant.

Sistem ini bukan hanya dashboard visual, tetapi:

* Platform monitoring produksi
* Engine kalkulasi OEE
* Sistem pencatat downtime
* Sistem manajemen work order
* Sistem histori produksi
* Fondasi transformasi digital manufaktur

Blueprint ini dirancang agar dapat menjadi:

* Technical reference
* Development guideline
* Architecture reference
* Documentation standard
* Long-term manufacturing system foundation

Dengan mengikuti project brief ini secara konsisten, sistem dapat dikembangkan secara:

* Scalable
* Maintainable
* Production-ready
* Real-time capable
* Industrial-friendly
* Future-proof
