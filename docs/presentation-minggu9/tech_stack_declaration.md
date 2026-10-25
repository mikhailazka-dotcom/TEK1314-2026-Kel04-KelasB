# Deklarasi Tech Stack dan Replikasi — Kelompok 4

**Mata Kuliah:** TEK1314 Keamanan Siber — Kelas B
**Skenario:** Pengujian Keamanan Sistem Monitoring Tanah Berbasis IoT
**Tanggal:** `[isi tanggal]`

## 1. Tech Stack Sistem Asli vs Replikasi

| Komponen | Teknologi pada Proyek IoT Asli | Direplikasi sebagai (Target Node) |
|---|---|---|
| Sistem Operasi Backend | Google Cloud (platform cloud) dan Supabase (layanan backend/database pendukung) | Ubuntu Server 22.04 LTS pada VM sebagai target pengujian lokal |
| Database / Penyimpanan Data | PostgreSQL melalui Supabase | PostgreSQL lokal pada Ubuntu Server (port 5432, hanya internal) |
| Protokol Komunikasi Sensor | HTTP REST API; ESP32-S3 mengirim data sensor ke backend melalui HTTP POST | HTTP REST API pada layanan backend lokal dengan data sensor simulasi |
| Dashboard / Monitoring | Aplikasi mobile berbasis Flutter | Monitoring layanan dan lalu lintas jaringan menggunakan log server; Security Onion |

## 2. Catatan Replikasi

Replikasi dilakukan pada level layanan (SSH, REST API, PostgreSQL), bukan salinan identik dari infrastruktur cloud. Lingkungan laboratorium tidak dapat mengakses Google Cloud dan Supabase, sehingga layanan tersebut direproduksi secara lokal. Pengujian keamanan dilakukan pada layanan yang sama jenisnya dengan sistem asli.

## 3. Skema IP Jaringan

Segmen jaringan: `192.168.4.0/24`

| Node Simulasi | Interface / IP Address | Role & Implementasi Teknis |
|---|---|---|
| Target Node (Ubuntu Server 22.04 LTS) | 192.168.4.5 | Host web server/API, PostgreSQL, endpoint API sensor, Elastic Agent |
| Attacker Node (Kali Linux) | 192.168.4.100 | Platform Red Team: reconnaissance, port scanning, pengujian API, serangan |
| Monitoring Node (Security Onion, management) | 192.168.4.200 | Dashboard SOC dan manajemen Security Onion untuk Blue Team |
| Monitoring Node (Security Onion, sniffer) | Promiscuous (tanpa IP) | Interface SPAN untuk penangkapan paket jaringan |
