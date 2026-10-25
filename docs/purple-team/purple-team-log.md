# Purple Team Log — Kelompok 4

Log ini mencatat setiap siklus Purple Team Loop untuk tiap skenario serangan. Setiap skenario harus melewati lima tahap, dan bukti setiap tahap disimpan di `docs/redteam/evidence/` atau `docs/blueteam/evidence/`.

## Alur kerja

Red menjalankan baseline lalu Blue mencatat, Blue melakukan mitigasi, dan Red mengulangi serangan yang sama untuk verifikasi. Blue tidak boleh melakukan mitigasi sebelum baseline terekam.

> **Catatan urutan:** hardening sudah diterapkan sebelum fase serangan. Tahap 1 (baseline) harus dijalankan terhadap kondisi sebelum hardening, misalnya snapshot pra-hardening, agar bukti "before" bisa dibandingkan. Jika baseline dijalankan terhadap sistem yang sudah diperkuat, catat di kolom catatan tahap 1.

---

## S1 — Reconnaissance

| Tahap | Aktivitas | PIC | Waktu (UTC) | Hasil | Bukti | Catatan |
|---|---|---|---|---|---|---|
| 1. Serangan baseline | Nmap full TCP scan `192.168.4.5` | Zahra | `[isi]` | `[isi jumlah port terbuka]` | `redteam/evidence/S1-1-...` | |
| 2. Deteksi | Alert Suricata/Zeek di Security Onion | Radinal | `[isi]` | `[terdeteksi / tidak]` | `blueteam/evidence/S1-2-...` | `[isi signature]` |
| 3. Mitigasi | Firewall whitelist per port, rate limiting | Radinal | `[isi]` | `[isi]` | `blueteam/evidence/S1-3-...` | |
| 4. Serangan ulang | Nmap scan yang sama setelah mitigasi | Zahra | `[isi]` | `[isi jumlah port terbuka]` | `redteam/evidence/S1-4-...` | |
| 5. Verifikasi & evaluasi | Bandingkan hasil 1 dan 4, tentukan status kontrol | Keduanya | `[isi]` | `[berhasil / sebagian / gagal]` | `[link]` | `[pelajaran]` |

## S2 — Brute-force SSH

| Tahap | Aktivitas | PIC | Waktu (UTC) | Hasil | Bukti | Catatan |
|---|---|---|---|---|---|---|
| 1. Serangan baseline | Hydra SSH dengan wordlist kecil | Mikhail | `[isi]` | `[isi]` | `redteam/evidence/S2-1-...` | |
| 2. Deteksi | Cek `auth.log` dan alert Suricata | Radinal | `[isi]` | `[isi]` | `blueteam/evidence/S2-2-...` | `[isi]` |
| 3. Mitigasi | fail2ban, ganti kredensial lemah | Radinal | `[isi]` | `[isi]` | `blueteam/evidence/S2-3-...` | |
| 4. Serangan ulang | Hydra dengan konfigurasi yang sama | Mikhail | `[isi]` | `[isi]` | `redteam/evidence/S2-4-...` | |
| 5. Verifikasi & evaluasi | Bandingkan hasil 1 dan 4 | Keduanya | `[isi]` | `[isi]` | `[link]` | `[pelajaran]` |

## S3 — Validasi API Key dan Rate Limiting

| Tahap | Aktivitas | PIC | Waktu (UTC) | Hasil | Bukti | Catatan |
|---|---|---|---|---|---|---|
| 1. Serangan baseline | cURL dengan key palsu, flood ringan ke `/api/v1/` | Zahra | `[isi]` | `[isi kode HTTP]` | `redteam/evidence/S3-1-...` | |
| 2. Deteksi | Log Nginx (status 401/403/429), Zeek `http.log` | Radinal | `[isi]` | `[isi]` | `blueteam/evidence/S3-2-...` | `[isi]` |
| 3. Mitigasi | Validasi X-API-KEY, tuning rate limit | Radinal | `[isi]` | `[isi]` | `blueteam/evidence/S3-3-...` | |
| 4. Serangan ulang | cURL dan flood yang sama | Zahra | `[isi]` | `[isi]` | `redteam/evidence/S3-4-...` | |
| 5. Verifikasi & evaluasi | Bandingkan hasil 1 dan 4 | Keduanya | `[isi]` | `[isi]` | `[link]` | `[pelajaran]` |

## S4 — ARP Spoofing (opsional)

Isi bagian ini jika S4 dijalankan. Jika gagal karena keterbatasan VMware, catat sebagai kendala pada tahap 1 dan tahap 5.

| Tahap | Aktivitas | PIC | Waktu (UTC) | Hasil | Bukti | Catatan |
|---|---|---|---|---|---|---|
| 1–5 | `[isi]` | `[isi]` | `[isi]` | `[isi]` | `[isi]` | `[isi]` |

---

## Ringkasan CVE dan Temuan

| Skenario | Kerentanan / celah | CVE (jika ada) | Status setelah mitigasi |
|---|---|---|---|
| S1 | `[isi]` | — | `[isi]` |
| S2 | Kredensial lemah (jika ditemukan) | — | `[isi]` |
| S3 | Validasi X-API-KEY | — | `[isi]` |

Terakhir diperbarui: `[isi tanggal]` (Minggu ke-11)
