# GAME_SERVER_FINAL_PROJECT_OS-SERVER-SYSTEM-ADMIN-23.83.1051

NAMA : ROVAL APRILIAN SANDI

NIM : 23.83.1051

# GAME SERVER | LEFT FOR DEAD 2

Spesifikasi yang saya gunakan, sebagai berikut:

- CPU: Minimal 2 core.

- RAM: 2 GB untuk server kecil (4-8 pemain).

- Penyimpanan: Sekitar 15 GB untuk file server.

- OS: Ubuntu Server 22.04.

# Layanan Server
1. openssh
2. Dedicated Server (Steam Command Line)
3. Systemd Service (Otomatisasi Server)
4. SourceMod & MetaMod (Modding dan Admin Tools)
5. Screen (Layanan Background Session)
6. Cron (Jadwal Otomatis Restart atau Backup)
7. Logrotate (Pengelolaan Log Server)
8. Fail2Ban (Keamanan Server)
## openssh

### install
```
sudo apt install openssh-server -y
sudo systemctl status ssh
sudo systemctl start ssh
sudo systemctl enable ssh
```
### konfigurasi
```
/etc/ssh/sshd_config
sudo nano /etc/ssh/sshd_config
```

```
Port 2222
PermitRootLogin no
PasswordAuthentication no
```

```
sudo systemctl restart ssh
```

```
sudo ufw allow ssh
sudo ufw allow 2222/tcp
```
## SteamCMD
### Cara install
```
sudo apt update
sudo apt install lib32gcc-s1 wget tar -y
mkdir ~/steamcmd
cd ~/steamcmd
wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
tar -xvzf steamcmd_linux.tar.gz
```
### Gunakan SteamCMD untuk mengunduh server:
```
./steamcmd.sh +login anonymous +force_install_dir ~/l4d2_server +app_update 222860 validate +quit
```

## Systemd Service
### buat layanan file
```
sudo nano /etc/systemd/system/l4d2.service
```
### tambahkan konfigurasi berikut:
```
[Unit]
Description=Left 4 Dead 2 Dedicated Server
After=network.target

[Service]
Type=simple
User=your_username
WorkingDirectory=/home/your_username/l4d2_server
ExecStart=/home/your_username/l4d2_server/srcds_run -game left4dead2 -console -port 27015 +map c1m1_hotel +maxplayers 8
Restart=always

[Install]
WantedBy=multi-user.target
```
### aktifkan layanan
```
sudo systemctl enable l4d2
sudo systemctl start l4d2
```

## SourceMod & MetaMod

### unduh MetaMod
```
wget https://mms.alliedmods.net/mmsdrop/1.11/mmsource-1.11.0-git1148-linux.tar.gz
tar -xvzf mmsource-1.11.0-git1148-linux.tar.gz -C ~/l4d2_server/left4dead2/addons
```
### unduh SourceMod
```
wget https://sm.alliedmods.net/smdrop/1.11/sourcemod-1.11.0-git6998-linux.tar.gz
tar -xvzf sourcemod-1.11.0-git6998-linux.tar.gz -C ~/l4d2_server/left4dead2/addons
```

## Screen
### cara install
```
sudo apt install screen
```
### mulai screen
```
screen -S l4d2
```
### menjalankan screen 
```
cd ~/l4d2_server
./srcds_run -game left4dead2 -console -port 27015 +map c1m1_hotel +maxplayers 8
```
## Cron
### cara install
```
sudo apt install cron
```
### konfigurasi
```
crontab -e
```
lalu
```
0 4 * * * systemctl restart l4d2
```
## Logrotate
### cara install
```
sudo apt install logrotate
```
### konfigurasi
```
sudo nano /etc/logrotate.d/l4d2
```
lalu
```
/home/your_username/l4d2_server/left4dead2/logs/*.log {
    daily
    missingok
    rotate 7
    compress
    notifempty
    create 640 your_username your_group
}
```
## Fail2ban
### cara install
```
sudo apt install fail2ban
```
### konfigurasi
```
sudo nano /etc/fail2ban/jail.local
```
lalu
```
[srcds]
enabled = true
port = 27015
protocol = udp
```
## Jalankan server
### Navigasi ke Direktori Server Pindah ke folder tempat server L4D2 diinstal:
```
cd /home/your_username/l4d2_server
```
### Jalankan Server Jalankan server dengan perintah berikut:
```
./srcds_run -game left4dead2 -console -port 27015 +map c1m1_hotel +maxplayers 8
```

## menjalankan server di latar belakang

### screen
```
screen -S l4d2 ./srcds_run -game left4dead2 -console -port 27015 +map c1m1_hotel +maxplayers 8
```
## Hubungkan server
A. Untuk Anda (Host)
- Buka Left 4 Dead 2 di komputer Anda.
- Tekan tombol ~ untuk membuka console.
- Masukkan perintah:
  ```
  connect 127.0.0.1
  ```
B. Untuk Pemain Lain
1. Dapatkan Alamat IP Public Server
- Gunakan perintah berikut di terminal server untuk mengetahui IP:
```
curl ifconfig.me
```
2. Bagikan IP dan Port ke Teman
- formatnya:
```
IP:Port
Contoh: 123.45.67.89:27015
```
3. Teman Anda Menggunakan Console
- Buka console di game dengan ~.
- Masukkan:
```
connect 123.45.67.89:27015
```

## Konfigurasi Tambahan
### Konfigurasi server.cfg
- Edit file konfigurasi server untuk mengatur nama, kata sandi admin, dan lainnya:
```
nano /home/your_username/l4d2_server/left4dead2/cfg/server.cfg
```
- Contoh konfigurasi:
```
hostname "Server Left 4 Dead 2 Anda"
rcon_password "password_admin"
sv_lan 0
sv_allow_lobby_connect_only 0
maxplayers 8
```
