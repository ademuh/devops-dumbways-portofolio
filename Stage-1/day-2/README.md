# Computer Network
**Computer Network** merupakan hubungan antara 2 device atau lebih menggunakan kabel atau wireless dimana semua device dapat berkomunikasi satu sama lain dan dapat bertukar data dan menggunakan resource bersama-sama (ex: printer/NAS) 

# Basic Shell Linux Commands

| Command   |      Fungsi  | Usage |
|:----------:|-------------|--------------------|
| **sudo** |  Digunakan untuk menjalankan sebuah command dengan access root. | `sudo apt install apache2` |
| **mkdir** | Membuat directory baru (Folder) | `mkdir <nama dir>`
| **cd** |  Change Directory, digunakan untuk mengganti directory ke lokasi baru | `cd <nama dir> / cd ..` |
| **ls** |  List, melihat isi directory yang sedang ditempati | `ls` |
| **cp** |  Copy, mengcopy sebuah file dengan nama baru | `cp <nama file> <file baru>`|
| **mv** |  Move, bisa digunakan untuk rename sebuah file atau untuk memindahkan file ke directory lain | `mv <file> <dir baru> / mv  <nama lama> <baru>`|
| **touch** |  Membuat sebuah file baru | `touch <nama dir>`|
| **echo** |  Menaruh string kedalam file | `echo "Hello" >> [file]`|
| **cat** | Melihat isi sebuah file | `cat <file>`|
| **find** |  Mencari data didalam sebuah file | `find -type f/d -name <file>`|
| **grep** |  Mencari text didalam sebuah file | `grep <text> <file> / grep -r <text>`|
| **chmod 777** |  Merubah permission file | `sudo chmod 777 <file>`|
| **chown** | Merubah ownership file | `sudo chown root:root <file>`|
| **history** | Melihat sejarah command yang digunakan | `history`|
| **ping** | Bisa digunakan untuk cek koneksi ke alamat yang dituju | `ping <address>`|
| **wget** | Mendownload file dari sebuah address |`wget https://wordpress.org/latest.zip`|
| **unzip** | Untuk membuka/extract archive | `unzip <archived file>`|
| **zip** | Untuk mengarchive directory | `zip -r <nama archive> <file>`|
| **adduser** | Menambahkan user baru | `sudo adduser <new user>`|
| **usermod** | Memasukkan user ke dalam group sudo | `sudo usermod -aG sudo <user>`|
| **sudo su** | Bisa digunakan untuk masuk root atau pindah user | `sudo su <user> / sudo su [masuk root]`|
| **rm** | Remove, digunakan untuk menghapus directory/file |`rm <file> / rmdir <dir> / rm -rf <dir>`|

# Merubah IP Address

Gunakan Command `ip a`, disini IP kita menggunakan `192.168.100.208`
 
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/1.png?raw=true)

Lalu, kita coba akses file konfigurasi netplan dengan command `sudo nano /etc/netplan/00-installer-config.yaml`
  
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/1-0.png?raw=true)
  
Dari sini kita akan masuk ke window nano 00-installer-config.yaml.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/1-1.png?raw=true)

Kita rubah IP lama (.200) menjadi IP baru (.131).

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/1-2.png?raw=true)
  
Jika sudah, jalankan command `sudo netplan apply` lalu cek kembali konfigurasi IP.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/1-3.png?raw=true)

Lalu coba kita tes apakah kita masih bisa terhubung internet.
  
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/1-4.png?raw=true)

# Login melalui SSH

Disini, kita akan menggunakan cmd bawaan Windows untuk login kedalam server melalui openSSH.
Gunakan command `ssh <username>@<IP Address>` untuk masuk kedalam server dan masukkan password.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/2.png?raw=true)

> Jika diminta fingerprint access, ketik 'Yes' dan masukkan password.

Jika berhasil, akan keluar seperti ini :

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/2-1.png?raw=true)

Dari sini, kita jalankan command `sudo apt update` dan `sudo apt upgrade` untuk mengambil list package dari repo server dan mengupgradenya jika dibutuhkan.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/2-2.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/2-3.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/2-4.png?raw=true)

# Install Apache2 dan Localtunnel
## Installing Apache2

Gunakan command `sudo apt-get install apache2 `/` sudo apt install apache 2` untuk mengambil package web server apache2.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/3.png?raw=true)

Agar kita dapat mem port-forward supaya bisa diakses publik, kita harus menyalakan firewall agar traffic yang lewat port 80 bisa mengakses web server.

Disini, kita menggunakan `sudo ufw enable` untuk menjalankan firewall, dan menggunakan `sudo ufw app list` untuk melihat list web server yang bisa menggunakan firewall tersebut.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/3-1.png?raw=true)

Karena kita ingin memberikan apache akses, maka kita jalankan command `sudo ufw allow 'Apache Full'`
Gunakan `sudo ufw status` untuk melihat app apa saja yang diberikan akses melalui firewall.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/3-2.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/3-3.png?raw=true)

Setelah apache terinstal dengan baik, lakukan check apakah apache berjalan normal
Gunakan command `systemctl status apache2`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/3-4.png?raw=true)

Jika dibuka melalui Direct IP (192.168.100.131) maka akan keluar :

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/3-5.png?raw=true)

## Install Localtunnel
Agar web server kita bisa diakses secara publik, kita harus menggunakan Localtunnel.
Sebelumnya, kita harus coba install `node.js` melalui command `nvm`.

Pertama, kita harus install fitur `curl` agar dapat transfer data melalui command-line via URL.
Gunakan command `sudo apt install curl`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/4.png?raw=true)

Lalu, kita install library `nvm` melalui command :
`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/4-1.png?raw=true)

Jalankan `exec bash` dan `nvm install 14` untuk menginstall nvm versi 14

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/4-2.png?raw=true)

Lalu, kita pastikan apakah `node.js` dan `nvm` sudah terinstall

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/4-3.png?raw=true)

Sekarang, karena `nvm` sudah terinstall, kita juga bisa menggunakan command `npm` sehingga kita bisa install localtunnel dari package yang sudah ada.
Gunakan command `npm install -g localtunnel`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/4-4.png?raw=true)

Jika sudah, command `lt` akan kita gunakan untuk membuat URL untuk di akses melalui port 80 (HTTP)
Jalankan command 'lt --port 80`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/4-5.png?raw=true)

URL Localtunnel akan dibuat dan bisa diakses melalui device apapun yang memperbolehkan port 80.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/4-6.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/4-7.png?raw=true)

Foto di Smartphone :

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-2/media/5.jpeg?raw=true)
