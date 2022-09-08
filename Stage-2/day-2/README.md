# Managed Database & Backend Setup
## Login SSH Keys
Disini, kita akan mencoba setup agar semua server bisa diakses menggunakan **Private Key** dari keygen SSH yang kita miliki.
Kita menggunakan SSH Key yang sudah dibuat di dalam appserver (Private key = yeet)

1. `ssh-keygen`
2. `cd .ssh`
3. `nano authorized_keys;
    #didalam nano
    ^R from id_rsa.pub`
    
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/8.png?raw=true)

4. `cp id_rsa yeet`
5. `scp -r .ssh ads@[IP Server tujuan]:~/ads/`
6. `ssh -i .ssh/yeet ads@ads@[IP Server tujuan]`

Dititik ini, kita dapat login ke server tanpa menggunakan password.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/2-3.png?raw=true)

Namun, karena server kita masih bisa diakses menggunakan password, disini kita bisa mematikan akses melalui password dengan akses ke `sshd_config` di folder `/etc/ssh` pada server yang ingin dimatikan aksesnya.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/2-2.png?raw=true)

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/2-1.png?raw=true)

## Menyiapkan Database MySQL
Jalankan `sudo apt install mysql-server` pada **Server Database** untuk menjalankan MySQL, dan `sudo apt install mysql-client` pada Server yang digunakan untuk menerima I/O daru database nanti.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/3-1.png?raw=true)

Setelah terinstal, disini kita akses terlebih MySQL melalui _root_ karena belum ada user di MySQL.
`sudo mysql -u root`

Untuk memberikan password kepada root, kita harus jalankan command ini didalam mysql :
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'passwordnya';`

Dengan password ini, kita dapat menjalankan command `sudo mysql_secure_installation` tanpa error karena akun _root_ sudah di secure dengan password tersebut.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/3-3.png?raw=true)

Sekarang, kita bisa gunakan parameter `-p` agar login di MySQL meggunakan password yang sudah ditentukan.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/3-4.png?raw=true)

Disini, baru kita mencoba untuk membuat user baru.

1. `mysql> CREATE USER 'ads'@'%' IDENTIFED BY 'password';`
2. `mysql> GRANT ALL PRIVILEGES ON *.* TO 'ads'@'%';` 
3. `sudo mysql -u ads -p`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/3-5.png?raw=true)

Setelah membuat user baru, kita dapat melakukan ujicoba apakah MySQL berjalan dengan baik.

1. `CREATE DATABASE dumbflix;`
2. `SHOW DATABASES;`
3. `USE dumbflix;`
4. `SHOW TABLES;`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/3-6.png?raw=true)

## Setup Backend

Kondisi : nginx terinstall, setup di gateway; repo dan nodejs terinstall di appserver
1. Nginx di _gateway_
2. Repository Backend
3. Nodejs v10.x di appserver
4. Sudah ada DNS Cloudflare (aimingds; api.aimingds)

**Reverse Proxy untuk Backend :**

1. `cd /etc/nginx/dumbways`
2. `sudo nano rproxyb.conf`
3. Source code :

        server { 
            server_name api.aimingds.studentdumbways.my.id; 
    
        location / { 
             proxy_pass http://<ip server>:5000;
            }
        }


4. `sudo nginx -t && sudo systemctl restart nginx`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/4.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/4-1.png?raw=true)

**Setup Backend + Frontend untuk appserver**
1. `git clone <Repo URL>`
2. `npm install`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/2.png?raw=true)

3. `cd <Repo>`
4. `npm i -g sequelize-cli`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/4-2.png?raw=true)

5. `cd <Repo>/config`
6. `cp .example.env .env`
7. `nano config.json`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/4-6.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/4-5.png?raw=true)

8. `sequelize-cli db:create`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/4-9.png?raw=true)

9. `nano ~/<Front-end>/src/config/api.js`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/5.png?raw=true)

## Test Run #1

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/6-2.png?raw=true)

**Setup Backend + Database**

1. `ss -natup`
2. `sudo nano /etc/mysql/mysql.conf.d/my.cnf`
3. `
bind-address = 0.0.0.0
mysqlx-bind-address = 0.0.0.0
`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/4-8.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/7.png?raw=true)

## Test Run #2

Dengan input yang sama di _Test Run #1_, kita cek di server **Database** apakah ada input data yang berhasil dimasukkan.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/7-1.png?raw=true)


## Instalasi Certbot

Agar koneksi kita dapat berubah ke `https://` maka kita harus memiliki sebuah _SSL Certificate_ yang akan digunakan sebagai bukti kalau server kita sudah _secure_.

Mengikuti manual yang ada di https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal, berikut hasil dari installasi certbot :

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/6.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/7-2.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/7-3.png?raw=true)

## Test Run #3

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/7-4.png?raw=true)








