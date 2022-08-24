# Introduction to DevOps

DevOps merupakan gabungan antara *Development* dan *Operations*. Engineer DevOps bertanggung jawab untuk menghubungi team development dan operations untuk mengurangi waktu release sebuah fitur/app dan mengurangi tingkat kegagalan serta mempercepat waktu perbaikan jika dibutuhkan.


# Instalasi Ubuntu Server @ VMware

## Step 1 : Menyiapkan ISO & Installer VMware
Untuk ISO ubuntu bisa di download melalui :
[Get Ubuntu Server | Download | Ubuntu](https://ubuntu.com/download/server)

![Website Ubuntu](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/0.png?raw=true)

Untuk VMware bisa di download melalui :
[Download VMware Workstation Player | VMware](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)

## Step 2 : Instalasi VMware
Lakukan instalasi VMware sebagaimana menginstall aplikasi biasanya.

## Step 3 : Membuat Virtual Machine baru
Setelah instalasi & restart PC selesai, VMware bisa langsung dibuka untuk kita membuat Virtual Machine baru.

Pilih "Create a new Virtual Machine", lalu *browse* lokasi ISO dari Ubuntu Server yang sudah kita download, dan juga inputkan username dan password serta nama Virtual Machine di VMware untuk instalasi Ubuntu tersebut.

Setelah itu, kita akan melakukan alokasi disk space dan resource yang digunakan.

Resource yang saya gunakan : 
 - 2-core CPU 
 - 2048MB RAM
 - 12.5GB Storage (*Split virtual disk into multiple files*  untuk saving space di PC)
 - Bridge Network Adapter (karena akan menggunakan IP yang ada di PC)

Jika sudah, maka di VMware akan muncul virtual machine yang sudah dibuat.

## Step 4 : Instalasi Ubuntu dalam VMware
Disini, setelah konfigurasi untuk virtual machine sudah siap, kita bisa langsung start VMnya dan melakukan instalasi ubuntu server.

Pertama, saat kita sudah masuk ke VM akan diminta untuk memilih language

Disini, saya memilih opsi *Continue without Updates* untuk menghindari update dan mempercepat instalasi.

Lalu kita bisa skip setup keyboard layout dan langsung melakukan konfigurasi network dalam VM.

Konfigurasi Network yang digunakan :
 - *DHCPv4* dirubah menjadi *static*
 - IP Address 192.168.100.208
 - Gateway 192.168.100.1
 - Subnet Mask 192.168.100.0/24
 - Name Server 8.8.8.8

Untuk proxy settings dan config ubuntu mirror bisa kita skip.

Disini, kita akan manage storage di dalam VM, maka dari itu kita pilih *Custom storage layout* agar bisa membuat 2 buah partisi.

Kita pilih "free space" yang ada, lalu pilih *Add GPT Partition* lalu alokasi space yang ingin digunakan sebagai *root* (10GB)

dan kita alokasikan sisanya sebagai *swap* untuk memori cadangan server.

Jika sudah, detail dari storage sudah bisa kita lihat, dari sini kita bisa melanjutkan proses instalasi.

Pilih *continue* untuk melanjutkan.

Lalu disini kita bisa setup nama kita, username dan password untuk login di servernya nanti.

Menu ini bisa di skip untuk melanjutkan instalasi.

Disini, kita centang *Install OpenSSH server* agar kita bisa melakukan login remote

Karena kita ingin *Docker* di install, bisa kita centang di menu ini sebelum instalasi kita jalankan.

Jika sudah, kita akan langsung masuk ke log instalasi sembari proses berjalan.

## Step 5 : Login ke Ubuntu Server

Jika instalasi sudah selesai, kita bisa langsung masuk kedalam OS Ubuntu Server.
Disini kita bisa login menggunakan username dan password yang sudah kita buat.

Untuk memeriksa apakah kita memiliki koneksi internet, kita akan langsung ping google menggunakan command `ping google.com`


