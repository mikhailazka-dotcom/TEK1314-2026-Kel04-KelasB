# Checklist Hardening — Blue Team

**Target:** SRV-IOT-KEL04B — 192.168.4.5 (Ubuntu Server 22.04 LTS)
**PIC:** Radinal Avram Utama (Blue Team)

Status: ✅ Sudah · ⬜ Belum · ⚠️ Perlu verifikasi
Bukti before/after disimpan di folder [`evidence/`](evidence/).

## A. Access & Authentication

| No | Kontrol | Status | Bukti Before | Bukti After |
|---|---|---|---|---|
| A1 | Nonaktifkan SSH password auth (`PasswordAuthentication no`), wajibkan key auth | ✅ | `evidence/A1-before-sshd.txt` | `evidence/A1-after-sshd.txt` |
| A2 | Nonaktifkan `PermitRootLogin` | ✅ | `evidence/A2-before-root-login.txt` | `evidence/A2-after-root-login.txt` |

## B. Service & Attack Surface

| No | Kontrol | Status | Bukti Before | Bukti After |
|---|---|---|---|---|
| B1 | Firewall UFW whitelist: hanya port 22, 80, 443, 1883 | ✅ ⚠️ | `evidence/B1-before-ufw.txt` | `evidence/B1-after-ufw.txt` |
| B2 | Sembunyikan versi Nginx (`server_tokens off`) | ✅ | `evidence/B2-before-nginx-header.txt` | `evidence/B2-after-nginx-header.txt` |
| B3 | Rate limiting Nginx pada `/api/v1/` | ✅ | `evidence/B3-before-ratelimit.txt` | `evidence/B3-after-ratelimit.txt` |

## C. Data & Application

| No | Kontrol | Status | Bukti Before | Bukti After |
|---|---|---|---|---|
| C1 | Isolasi total port database (PostgreSQL 5432, internal only) | ✅ | `evidence/C1-before-ss-5432.txt` | `evidence/C1-after-ss-5432.txt` |
| C2 | Kredensial database di file `.env` (`chmod 600`) | ✅ | `evidence/C2-before-env-perm.txt` | `evidence/C2-after-env-perm.txt` |
| C3 | Redirect HTTP ke HTTPS (301) dan header HSTS | ✅ | `evidence/C3-before-http.txt` | `evidence/C3-after-http.txt` |

## Catatan

- ⚠️ **B1:** Konfirmasi bahwa broker MQTT benar-benar berjalan di port 1883. Jika tidak, hapus 1883 dari whitelist dan perbarui dokumen ini.
- Kredensial, token, dan isi `.env` tidak boleh masuk ke folder ini. Gunakan output yang sudah disamarkan.
