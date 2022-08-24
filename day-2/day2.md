# Computer Network
**Computer Network** merupakan hubungan antara 2 device atau lebih menggunakan kabel atau wireless dimana semua device dapat berkomunikasi satu sama lain dan dapat bertukar data dan menggunakan resource bersama-sama (ex: printer/NAS) 

# Basic Shell Linux Commands

| Command   |      Fungsi  | Usage |
|:----------:|-------------|--------------------|
| **sudo** |  Digunakan untuk menjalankan sebuah command dengan access root. | sudo apt install apache2 |
| **mkdir** | Membuat directory baru (Folder) | mkdir <nama dir>
| **cd** |  Change Directory, digunakan untuk mengganti directory ke lokasi baru | cd <nama dir> / cd .. |
| **ls** |  List, melihat isi directory yang sedang ditempati | ls |
| **cp** |  Copy, mengcopy sebuah file dengan nama baru | cp <nama file> <file baru>|
| **mv** |  Move, bisa digunakan untuk rename sebuah file atau untuk memindahkan file ke directory lain | mv <file> <dir baru> / mv  <nama lama> <baru>|
| **touch** |  Membuat sebuah file baru | touch <nama dir>|
| **echo** |  Menaruh string kedalam file | echo "Hello" >> [file]|
| **cat** | Melihat isi sebuah file | cat <file>|
| **find** |  Mencari data didalam sebuah file | find -type f/d -name <file>|
| **grep** |  Mencari text didalam sebuah file | grep <text> <file> / grep -r <text>|
| **chmod 777** |  Merubah permission file | sudo chmod 777 <file>|
| **chown** | Merubah ownership file | sudo chown root:root <file>|
| **history** | Melihat sejarah command yang digunakan | history|
| **ping** | Bisa digunakan untuk cek koneksi ke alamat yang dituju | ping <address>|
| **wget** | Mendownload file dari sebuah address |wget https://wordpress.org/latest.zip|
| **unzip** | Untuk membuka/extract archive | unzip <archived file>|
| **zip** | Untuk mengarchive directory | zip -r <nama archive> <file>|
| **adduser** | Menambahkan user baru | sudo adduser <new user>|
| **usermod** | Memasukkan user ke dalam group sudo | sudo usermod -aG sudo <user>|
| **sudo su** | Bisa digunakan untuk masuk root atau pindah user | sudo su <user> / sudo su [masuk root]|
| **rm** | Remove, digunakan untuk menghapus directory/file |rm <file> / rmdir <dir> / rm -rf <dir>|

# IP Configuration
## 1. Memeriksa konfigurasi IP
Gunakan Command `ip a`, disini IP kita menggunakan `192.168.100.208`
 
![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-2/media/1.png?raw=true)

Lalu, kita coba akses file konfigurasi netplan dengan command `sudo nano /etc/netplan/00-installer-config.yaml`
  
![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-2/media/1-0.png?raw=true)
  
Dari sini kita akan masuk ke window nano 00-installer-config.yaml.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-2/media/1-1.png?raw=true)

Kita rubah IP lama (.200) menjadi IP baru (.131).

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-2/media/1-2.png?raw=true)
  
Jika sudah, jalankan command `sudo netplan apply` lalu cek kembali konfigurasi IP.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-2/media/1-3.png?raw=true)

Lalu coba kita tes apakah kita masih bisa terhubung internet.
  
![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-2/media/1-4.png?raw=true)

