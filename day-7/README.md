# Web Server
**Web Server** merupakan software yang memberikan suatu layanan untuk user dengan menerima HTTP/HTTPS dari Web Browser, Lalu **Web Server** akan mengembalikan permintaan tersebut dengan menampilkan halaman web.

# Reverse Proxy dan Load Balancing
## Membuat 3 instance VM
Disini, saya akan menggunakan server VMware sebagai _gateway_, lalu menggunakan **multipass** untuk 2 server _aplikasi_.

Sebelumnya, kita harus install web server **nginx** dengan menjalankan `sudo apt install nginx`.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/1.png?raw=true)

Jalankan `nginx -v` dan `sudo systemctl status nginx` untuk memastikan apakah sudah terinstal.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/2.png?raw=true)

> Note : disini, nginx masih disabled namun sudah terinstal dengan baik.

Sekarang kita install **multipass** dengan menggunakan `sudo snap install multipass`.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/3.png?raw=true)

Lalu, kita membuat 2 instance VM didalam **multipass** dengan commmand `multipass launch --name [VM name]`.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/3-1.png?raw=true)

Setelah kita membuat 2 instance VM, kita check _IP Address_ dari kedua instance tersebut agar kita dapat melakukan **Reverse Proxy** dan **Load Balancing** nanti.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/3-4.png?raw=true)

Karena kita akan menjalankan aplikasi front-end _React.JS_, maka kita harus melakukan installasi _Node.JS_ di kedua instance VM terlebih dahulu.
Kita masuk kedalam instance VM menggunakan command `multipass shell [VM name]`

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/3-3.png?raw=true)

Lalu, kita lakukan instalasi `nvm` dan mengambil repo `wayshub` untuk front-end yang kita gunakan.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/4.png?raw=true)

Kita jalankan `nvm install 16` dan menginstal express JS, lalu `npm install` didalam repo yang sudah kita pull, lalu kita pastikan versinya sudah sesuai atau tidak.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/4-1.png?raw=true)

Karena kita akan menggunakan PM2 untuk mengatur proses server, maka kita lakukan instalasi PM2 terlebih dahulu.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/4-2.png?raw=true)

Lakukan hal yang sama pada VM ke-2. (Instalasi nodejs dan pull repo)
Sampai disini, harusnya kita sudah memiliki :
- _Server Gateway_ dengan **nginx** terinstal.
- _Server Aplikasi_ dengan nodejs dengan front-end yang siap digunakan
- Process Manager dengan mengunakan PM2.

## Reverse Proxy
Disini, kita masuk ke folder **nginx** dengan command `cd /etc/nginx` lalu membuat directory baru dengan command `sudo mkdir dumbways` dan membuat file baru dengan `sudo nano my.reverse-proxy.conf`

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/5-1.png?raw=true)

Karena kita ingin menggunakan Server Aplikasi ke-1 untuk **Reverse Proxy**, maka kita isi dengan source code berikut :

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/5-2.png?raw=true)

Lalu, kita keluar dari directory `dumbways` lalu merubah file config **nginx** dengan `sudo nano nginx.conf`
Disini, kita masukkan directory kita tadi ke virtual host, agar instance **Multipass** tadi dapat dijalankan melalui nginx.
Kita masukkan `include /etc/nginx/dumbways/*`

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/5-3.png?raw=true)

Jalankan `sudo nginx -t` untuk memeriksa apakah ada kesalahan syntax pada file .conf kita.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/5-4.png?raw=true)

Lalu, untuk menguji apakah nginx berjalan, kita masuk kedalam instance VM aplikasi 1 lalu menjalankan aplikasi wayshub.
Karena kita menggunakan PM2, kita jalankan dengan `pm2 start npm --name "wayshub" -- start`

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/6.png?raw=true)

Sampai disini, kita memiliki :
- Halaman web yang dapat diakses melalui domain "wayshub-ads.xyz"
- Penghubungan antara _VM app1_ dengan _gateway_ melalui **nginx**
- _VM app1_ menjalankan `npm` menggunakan PM2

## Load Balancing
Setelah berhasil menjalankan web server dengan **Reverse Proxy**, sekarang kita lakukan **Load Balancing** dengan step-by-step yang kurang lebih sama.
Namun disini, ada perbedaan sedikit di file .conf

Kita akses kembali `etc/nginx/dumbways` lalu membuat file konfigurasi baru dengan nama `loadbal.conf`.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-7/media/7.png?raw=true)

Disini, kita isi `yeet` dengan _IP Address_ dari ke-2 instance VM **multipass** tadi, dan kita ganti address proxy kita melalui `yeet:3000`.

Kita lakukan instalasi yang sama dengan _VM app1_ ke _VM app2_, sehingga kita dapat mengimplementasikan **Load Balancing**.

Sampai disini, kita memiliki :
- Halaman web yang menggunakan **Load Balancing** melalui domain "lb.wayshub-ads.xyz"
- **Load Balancing** diantara ke-2 server VM di **multipass**

> Note : Karena saya menjalankan instance **multipass** didalam VM, maka di file Sys32/drivers/etc/hosts saya tambahkan IP Server dan domain yang ingin diakses karena _Windows_ tidak mengenal ke-2 IP dari instance **multipass** namun **nginx** mengenal ke-2nya dan dapat diakses melalui IP server dari VMware.


