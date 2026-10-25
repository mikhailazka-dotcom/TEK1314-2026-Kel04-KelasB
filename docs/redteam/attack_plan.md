# Attack Plan — Fase 2 (Red Team)

**Kelompok:** 4 (Kelas B)
**Skenario proyek:** Pengujian Keamanan Sistem Monitoring Tanah Berbasis IoT
**Target node:** `192.168.4.5` — `SRV-IOT-KEL04B` (Ubuntu Server 22.04 LTS)
**Attacker node:** `192.168.4.100` — Kali Linux
**Monitoring node:** `192.168.4.200` — Security Onion
**Disusun oleh:** Red Team (Zahra Fadhail Amalia, Mikhail Rafi Azka)
**Status:** Rencana. Eksekusi Minggu 10, pelaporan Minggu 11.

---

## 1. Ringkasan Skenario

| Kode | Kategori | Teknik | Tools | Target | Hasil yang diharapkan |
|---|---|---|---|---|---|
| S1 | Reconnaissance | Scanning port dan versi layanan, OS fingerprinting, enumerasi banner | Nmap, Wireshark | Seluruh port TCP `192.168.4.5` | Daftar port terbuka dan versi layanan |
| S2 | Akses tidak sah | Brute-force SSH, uji kredensial lemah | Hydra, ssh | TCP 22 (SSH) | Sesi shell jika kredensial lemah ditemukan; jika tidak, percobaan gagal tercatat |
| S3 | Web / API | Validasi X-API-KEY (API key palsu/tidak ada) dan uji rate limiting | Burp Suite, cURL | TCP 80/443, endpoint `/api/v1/` | Request tanpa key valid ditolak; request berlebih dibatasi |
| S4 (opsional) | Jaringan lokal | ARP spoofing dan penyadapan HTTP | Ettercap, Wireshark | Segmen `192.168.4.0/24` | Kredensial terlihat dalam teks biasa, jika HTTP tidak terenkripsi |

## 2. Dasar Pemilihan Skenario

1. Arsitektur IoT asli menerima data sensor melalui REST API dengan autentikasi X-API-KEY, sehingga API dan kunci API menjadi jalur masuk utama.
2. SSH menjadi akses administrasi server dan menjadi target brute-force yang paling umum.
3. Target sudah diperkuat (hardening Fase 1), sehingga setiap serangan menguji apakah kontrol bekerja.

## 3. Detail Skenario

### S1 — Reconnaissance

- **Tujuan:** memetakan permukaan serangan.
- **Langkah:**
  ```bash
  ping -c 4 192.168.4.5
  sudo nmap -sS -sV -O -p- -T4 192.168.4.5 -oN S1-recon-full-tcp.txt
  sudo nmap -sU --top-ports 20 192.168.4.5 -oN S1-recon-udp.txt
  ```
- **Bukti:** file output nmap, screenshot terminal, screenshot alert di Security Onion.
- **Deteksi Blue Team:** Suricata `ET SCAN`, Zeek `conn.log` (banyak koneksi `S0`/`REJ` dari satu sumber).
- **Mitigasi yang diharapkan:** whitelist port per layanan, rate limiting koneksi.

### S2 — Brute-force SSH

- **Tujuan:** menguji ketahanan autentikasi SSH setelah hardening.
- **Langkah:**
  ```bash
  hydra -l <user-uji> -P <wordlist-kecil.txt> -t 4 -f ssh://192.168.4.5
  ```
  Wordlist kecil (10–20 kandidat) dibuat manual untuk menghindari beban berlebih.
- **Bukti:** screenshot Hydra, screenshot `auth.log` target.
- **Deteksi Blue Team:** `/var/log/auth.log` berisi `Failed password` berulang dari 192.168.4.100; Suricata `ET SCAN Potential SSH Brute Force`.
- **Ambang wajar:** lebih dari 5 kegagalan dalam 60 detik dari satu IP.
- **Mitigasi yang diharapkan:** fail2ban, larangan login root (sudah), key-only auth (sudah).

### S3 — Validasi API Key dan Rate Limiting

- **Tujuan:** menguji apakah endpoint `/api/v1/` menolak request tanpa API key yang valid dan membatasi request berlebih.
- **Langkah:**
  ```bash
  # Request tanpa key
  curl -i https://192.168.4.5/api/v1/
  # Request dengan key palsu
  curl -i -H "X-API-KEY: palsu-123" https://192.168.4.5/api/v1/
  # Flood ringan untuk menguji rate limit
  for i in $(seq 1 50); do curl -s -o /dev/null -w "%{http_code}\n" -H "X-API-KEY: palsu-123" https://192.168.4.5/api/v1/; done
  ```
  Uji juga melalui Burp Suite (Repeater) dengan memodifikasi header X-API-KEY.
- **Bukti:** output cURL, screenshot Burp Suite, log Nginx.
- **Deteksi Blue Team:** log Nginx berisi status 401/403/429 berulang dari satu IP; Zeek `http.log` untuk pola request berulang.
- **Mitigasi yang diharapkan:** validasi X-API-KEY di sisi server, rate limiting (sudah diterapkan), HTTPS dengan HSTS (sudah diterapkan).

### S4 — ARP Spoofing (opsional)

- **Tujuan:** membuktikan apakah traffic HTTP tanpa enkripsi bisa disadap di segmen lokal.
- **Catatan:** di lingkungan virtual, ARP spoofing bisa tidak stabil. Jika gagal, catat sebagai kendala dan jangan dipaksakan.
- **Deteksi Blue Team:** Zeek ARP log, arpwatch jika tersedia.
- **Mitigasi yang diharapkan:** HTTPS (sudah diterapkan), static ARP entry.

## 4. Aturan Pengujian

- Serangan hanya ditujukan ke `192.168.4.5`.
- Tidak boleh ada serangan ke jaringan kampus, Wi-Fi publik, atau infrastruktur lab lain.
- Gunakan data dummy. Samarkan data sensitif pada screenshot.
- Snapshot VM diambil sebelum eksekusi.

## 5. Rencana Eksekusi Minggu 10

| Urutan | Aktivitas | Estimasi | PIC |
|---|---|---|---|
| 0 | Snapshot VM target dan attacker | 5' | Mikhail |
| 1 | S1 Reconnaissance | 20' | Zahra |
| 2 | Cek alert di Security Onion | 10' | Zahra, Radinal |
| 3 | S2 Brute-force SSH | 25' | Mikhail |
| 4 | S3 Validasi API key dan rate limiting | 25' | Zahra |
| 5 | S4 ARP spoofing (opsional) | 15' | Mikhail |
| 6 | Kumpulkan bukti, beri nama sesuai konvensi | 15' | Keduanya |
| 7 | Serahkan temuan ke Blue Team | 10' | Keduanya |

**Prasyarat:**
- [ ] Snapshot target dan attacker sudah diambil
- [ ] Security Onion aktif dan menerima traffic
- [ ] Blue Team sudah menerima detection handoff
- [ ] Folder `evidence/` siap

## 6. Kriteria Keberhasilan

| Skenario | Berhasil jika | Gagal (tetap dicatat) jika |
|---|---|---|
| S1 | Daftar port dan versi layanan didapat | Target tidak merespons atau semua port filtered |
| S2 | Sesi SSH didapat | Semua kredensial ditolak |
| S3 | Request tanpa key valid berhasil diproses | Request ditolak dan rate limit aktif (ini hasil yang diharapkan dari hardening) |
| S4 | Kredensial terlihat di capture | Traffic terenkripsi atau ARP spoofing tidak bekerja |

> Kegagalan serangan karena hardening adalah bukti bahwa Blue Team bekerja. Dokumentasikan apa adanya.
