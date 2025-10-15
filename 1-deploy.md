## Versi Ubuntu
22

## Install script

Download install script
```
https://raw.githubusercontent.com/Falestio/odoo-deploy/refs/heads/main/install_odoo_ubuntu.sh
```

`sudo chmod +x install_odoo_ubuntu.sh`

`sudo ./install_odoo_ubuntu.sh`

## Multi-db config
problem: odoo punya jenis user logged in, dan user non-logged in (portal, website visitor) Jika odoo menjalankan multi database, odoo harus tau database yang mana yang non logged in user akan pakai

Ini tidak masalah untuk logged in user karena mereka bisa memilih database saat log in

Ini tidak masalah jika odoo hanya menggunakan satu database

Untuk multiple database kasus ini ditangani oleh `dbfilter`

menggunakan regex

possibly including the dynamically injected hostname (%h) or the first subdomain (%d)

- Show only databases with names beginning with 'mycompany'

```
[options]
dbfilter = ^mycompany.*$
```

- Show only databases matching the first subdomain after www: for example the database “mycompany” will be shown if the incoming request was sent to www.mycompany.com or mycompany.co.uk, but not for www2.mycompany.com or helpdesk.mycompany.com.

```
[options]
dbfilter = ^%d$
```

Referensi: https://www.odoo.com/documentation/18.0/administration/on_premise/deploy.html#dbfilter

## Postgre sql
Terdapat 2 opsi untuk deploy database
- dalam satu hardware yang sama dengan odoo
- hardware yang terpisah dengan odoo

problem: postgre by default hanya support koneksi melalui UNIX soccet

Jika postgre diinstall pada mesin yang berbeda maka ada 2 pilihan

- gunakan koneksi loopback dan hubungkan kedua hardware melalui SSH tunnel
- Konfigurasi koneksi melalui SSL

Referensi: https://www.odoo.com/documentation/18.0/administration/on_premise/deploy.html#postgresql

