# Checklist Hardening — Blue Team

**Target:** SRV-IOT-KEL04B — 192.168.4.5 (Ubuntu Server 22.04 LTS)
**PIC:** Radinal Avram Utama (Blue Team)

Bukti before/after tersimpan di folder [`evidence/`](evidence/).

## A. Access & Authentication

| No | Kontrol | Status | Bukti Before | Bukti After |
|---|---|---|---|---|
| A1 | Nonaktifkan SSH password auth (`PasswordAuthentication no`), wajibkan key auth | ✅ | [docs/blueteam/evidence/SSH Auth Before.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/SSH%20Auth%20Before.jpeg) | [docs/blueteam/evidence/SSH Auth After.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/SSH%20Auth%20After.jpeg) |
| A2 | Nonaktifkan `PermitRootLogin` | ✅ | [docs/blueteam/evidence/Root Login Before.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Root%20Login%20Before.jpeg) | [docs/blueteam/evidence/Root Login After.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Root%20Login%20After.jpeg) |

## B. Service & Attack Surface

| No | Kontrol | Status | Bukti Before | Bukti After |
|---|---|---|---|---|
| B1 | Firewall UFW whitelist: hanya port 22, 80, 443, 1883 | ✅ | [docs/blueteam/evidence/Firewall Status Before.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Firewall%20Status%20Before.jpeg) | [docs/blueteam/evidence/Firewall Status After.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Firewall%20Status%20After.jpeg) |
| B2 | Sembunyikan versi Nginx (`server_tokens off`) | ✅ | [docs/blueteam/evidence/Sembunyikan versi nginx Before.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Sembunyikan%20versi%20nginx%20Before.jpeg) | [docs/blueteam/evidence/Sembunyikan versi nginx After.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Sembunyikan%20versi%20nginx%20After.jpeg) |
| B3 | Rate limiting Nginx pada `/api/v1/` | ✅ | [docs/blueteam/evidence/Nginx Rate Limiting Before.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Nginx%20Rate%20Limiting%20Before.jpeg) | [docs/blueteam/evidence/Nginx Rate Limiting After.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Nginx%20Rate%20Limiting%20After.jpeg) |

## C. Data & Application

| No | Kontrol | Status | Bukti Before | Bukti After |
|---|---|---|---|---|
| C1 | Isolasi total port database (PostgreSQL 5432, internal only) | ✅ | [docs/blueteam/evidence/Isolasi Port DB Before.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Isolasi%20Port%20DB%20Before.jpeg) | [docs/blueteam/evidence/Isolasi Port DB After.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Isolasi%20Port%20DB%20After.jpeg) |
| C2 | Kredensial database di file `.env` (`chmod 600`) | ✅ | [docs/blueteam/evidence/Mengamankan kredensial database via file.env Before.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Mengamankan%20kredensial%20database%20via%20file.env%20Before.jpeg) | [docs/blueteam/evidence/Mengamankan kredensial database via file.env After.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/Mengamankan%20kredensial%20database%20via%20file.env%20After.jpeg) |
| C3 | Redirect HTTP ke HTTPS (301) dan header HSTS | ✅ | ⚠️ `[belum ada file before, lihat catatan]` | [docs/blueteam/evidence/redirect http ke httpss After.jpeg](https://github.com/mikhailazka-dotcom/TEK1314-2026-Kel04-KelasB/blob/main/docs/blueteam/evidence/redirect%20http%20ke%20httpss%20After.jpeg) |

## Catatan

- ⚠️ **B1:** Konfirmasi bahwa broker MQTT benar-benar berjalan di port 1883. Jika tidak, hapus 1883 dari whitelist dan perbarui dokumen ini.
- ⚠️ **C3:** File `redirect http ke httpss After.jpeg` di GitHub memiliki kesalahan ketik ("httpss"). Sebaiknya diganti namanya menjadi `redirect http ke https After.jpeg`, lalu link di atas diperbarui. Bukti before untuk C3 belum ada di folder. File `Bukti Web.jpeg` kemungkinan bisa dipakai sebagai bukti before, tetapi perlu dicek dulu isinya.
- Kredensial, token, dan isi `.env` tidak boleh masuk ke folder ini. Pastikan screenshot file `.env` tidak menampilkan nilai kredensial.
