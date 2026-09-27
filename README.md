# TEK1314-2026-Kel04-KelasB
Repository Mata Kuliah Keamanan Siber Kelas B – Kelompok 4 | TEK1314 2026
- Azaria Nur Febriasari (J0404241041)
- Zahra Fadhail Amalia (J0404241046)
- Radinal Avram Utama (J0404241161)
- Mikhail Rafi Azka (J0404241168)

# PBL Keamanan Siber - Kelompok 4
## Skenario Proyek
Proyek PBL Kelompok 4 mengangkat skenario **Pengujian Keamanan Sistem Monitoring Tanah Berbasis IoT**. Sistem yang menjadi objek pengujian merupakan sistem monitoring yang digunakan untuk memantau kondisi tanah berdasarkan beberapa parameter, yaitu suhu, pH, dan kelembapan tanah.

Pada sistem tersebut, data hasil pembacaan sensor dikirimkan menuju server monitoring untuk diproses dan digunakan sebagai sumber informasi kondisi tanah. Dalam proyek keamanan ini, server monitoring direpresentasikan oleh **IoT Monitoring Server** yang digunakan sebagai lingkungan simulasi pengujian keamanan.

Lingkungan proyek terdiri atas tiga node utama, yaitu:
- Kali Linux sebagai Attacker Node untuk melakukan pengujian keamanan
- IoT Monitoring Server sebagai server yang merepresentasikan backend sistem monitoring IoT
- Security Onion sebagai Monitoring Node untuk memantau aktivitas jaringan selama proses pengujian

## Network
Topologi jaringan dirancang menggunakan segmen:
- Network: `192.168.4.0/24`
- IoT Monitoring Server: `192.168.4.5`
- Attacker Node: `192.168.4.100`
- Monitoring Node: `192.168.4.200`

## Port yang Menjadi Fokus Pengujian

Red Team melakukan identifikasi awal terhadap beberapa service
yang berpotensi menjadi fokus pengujian keamanan pada Metasploitable.

| Port | Service |
|------|---------|
|  21  |   FTP   |
|  22  |   SSH   |
|  23  |  Telnet |
|  80  |   HTTP  |
| 3306 |   MySQL |

Port dan service lain pada Metasploitable dapat diidentifikasi dan diverifikasi kembali pada tahap implementasi.

## Peran Red Team dan Blue Team
**Red Team** melakukan pengujian keamanan terhadap service yang tersedia pada IoT Monitoring Server.
**Blue Team** menggunakan Security Onion untuk memonitor aktivitas jaringan dan mengamati indikasi aktivitas pengujian dari Red Team.

## Struktur Design
- `docs/design/topology.png` — diagram topologi jaringan.
- `docs/design/ip_plan.md` — perencanaan IP Address dan OS setiap node.
