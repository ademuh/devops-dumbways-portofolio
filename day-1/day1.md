# Introduction to DevOps

DevOps merupakan gabungan antara *Development* dan *Operations*. Engineer DevOps bertanggung jawab untuk menghubungi team development dan operations untuk mengurangi waktu release sebuah fitur/app dan mengurangi tingkat kegagalan serta mempercepat waktu perbaikan jika dibutuhkan.


# Instalasi Ubuntu Server @ VMware

## Step 1 : Menyiapkan ISO & Installer VMware
Untuk ISO ubuntu bisa di download melalui :
[Get Ubuntu Server | Download | Ubuntu](https://ubuntu.com/download/server)

![Website Ubuntu](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/0.png?raw=true)

Untuk VMware bisa di download melalui :
[Download VMware Workstation Player | VMware](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)

![Website VMware](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/1.png?raw=true)

## Step 2 : Instalasi VMware
Lakukan instalasi VMware sebagaimana menginstall aplikasi biasanya.

![Window Installasi VMware](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/1-1.png?raw=true)

## Step 3 : Membuat Virtual Machine baru
Setelah instalasi & restart PC selesai, VMware bisa langsung dibuka untuk kita membuat Virtual Machine baru.

![Tampilan VMware](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/2.png?raw=true)

Pilih "Create a new Virtual Machine", lalu *browse* lokasi ISO dari Ubuntu Server yang sudah kita download, dan juga inputkan username dan password serta nama Virtual Machine di VMware untuk instalasi Ubuntu tersebut.

![Browse ISO](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/3.png?raw=true)

![Username/Password VM](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/4.png?raw=true)

![Nama VM](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/5.png?raw=true)

Setelah itu, kita akan melakukan alokasi disk space dan resource yang digunakan.

> Resource yang saya gunakan : 
> - 2-core CPU 
> - 2048MB RAM
> - 12.5GB Storage (*Split virtual disk into multiple files*  untuk saving space di PC)
> - Bridge Network Adapter (karena akan menggunakan IP yang ada di PC)

![Storage VM](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/6.png?raw=true)

![Resource VM](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/7.png?raw=true)

![Network VM](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/7-1.png?raw=true)

Jika sudah, maka di VMware akan muncul virtual machine yang sudah dibuat.

![VM Berhasil dibuat](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/7-2.png?raw=true)

## Step 4 : Instalasi Ubuntu dalam VMware
Disini, setelah konfigurasi untuk virtual machine sudah siap, kita bisa langsung start VMnya dan melakukan instalasi ubuntu server.
Pertama, saat kita sudah masuk ke VM akan diminta untuk memilih language.

![Installasi dalam VM 1](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/8.png?raw=true)

Disini, saya memilih opsi *Continue without Updates* untuk menghindari update dan mempercepat instalasi.

![Installasi dalam VM 2](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/9.png?raw=true)

Lalu kita bisa skip setup keyboard layout dan langsung melakukan konfigurasi network dalam VM.

![Installasi dalam VM 3](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/10.png?raw=true)

Konfigurasi Network yang digunakan :
 - *DHCPv4* dirubah menjadi *static*
 - IP Address 192.168.100.208
 - Gateway 192.168.100.1
 - Subnet Mask 192.168.100.0/24
 - Name Server 8.8.8.8

![Installasi dalam VM 4](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/11.png?raw=true)

Untuk proxy settings dan config ubuntu mirror bisa kita skip.

![Installasi dalam VM 5](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/12.png?raw=true)

![Installasi dalam VM 6](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/13.png?raw=true)

Disini, kita akan manage storage di dalam VM, maka dari itu kita pilih *Custom storage layout* agar bisa membuat 2 buah partisi.

![Installasi dalam VM 7](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/14.png?raw=true)

Kita pilih "free space" yang ada, lalu pilih *Add GPT Partition* lalu alokasi space yang ingin digunakan sebagai *root* (10GB)

![Storage Config VM](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/14-1.png?raw=true)

![Storage Config VM 2](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/14-2.png?raw=true)

dan kita alokasikan sisanya sebagai *swap* untuk memori cadangan server.

![Storage Config VM 3](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/14-3.png?raw=true)

Jika sudah, detail dari storage sudah bisa kita lihat, dari sini kita bisa melanjutkan proses instalasi.

![Storage Config VM 4](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/14-4.png?raw=true)

Pilih *continue* untuk melanjutkan.

![Installasi dalam VM 8](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/16.png?raw=true)

Lalu disini kita bisa setup nama kita, username dan password untuk login di servernya nanti.

![Installasi dalam VM 9](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/17.png?raw=true)

Menu ini bisa di skip untuk melanjutkan instalasi.

![Installasi dalam VM 10](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/18.png?raw=true)

Disini, kita centang *Install OpenSSH server* agar kita bisa melakukan login remote

![Installasi dalam VM 11](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/19.png?raw=true)

Karena kita ingin *Docker* di install, bisa kita centang di menu ini sebelum instalasi kita jalankan.

![Installasi dalam VM 12](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/20.png?raw=true)

Jika sudah, kita akan langsung masuk ke log instalasi sembari proses berjalan.

![Installasi dalam VM 13](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/21.png?raw=true)

## Step 5 : Login ke Ubuntu Server

Jika instalasi sudah selesai, kita bisa langsung masuk kedalam OS Ubuntu Server.
Disini kita bisa login menggunakan username dan password yang sudah kita buat.

![Ubuntu VM](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/23.png?raw=true)

Untuk memeriksa apakah kita memiliki koneksi internet, kita akan langsung ping google menggunakan command `ping google.com`

![Ubuntu VM 2](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-1/media/25.png?raw=true)


