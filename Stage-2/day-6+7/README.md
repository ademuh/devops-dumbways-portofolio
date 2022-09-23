# Monitoring Server & Ansible

## Preemptive Setup
- 1 Instance IDCH ( 1 CPU, 2 GB RAM, 20GB Storage )
- Reverse Proxy untuk Prometheus, Grafana dan Node-exporter
- DNS Cloudflare (prom.ade;dashboard.ade;exporter.ade)

## Install Ansible
Karena _ansible_ membutuhkan dependency _python3-pip_, maka kita jalankan instalasi _python3_ terlebih dahulu.
Lalu jalankan command `python3 -m pip install --user Ansible` untuk memulai instalasi Ansible.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/1.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/1-1.png?raw=true)

Jika sudah, kita jalankan `ansible --version` dan `python3 -m pip show ansible` untuk memeriksa apakah _ansible_ sudah terinstall dengan baik.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/1-2.png?raw=true)

Lalu kita buat file :
- `Inventory` => Untuk menampung IP Server yang ingin kita config.
- `ansible.cfg` => sebagai konfigurasi ansible.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/1-3.png?raw=true)

## Ansible : ansible.builtin.user
Disini ktia akan membuat _ansible_ untuk membuat user baru di server, disini saya menggunakan user `ade`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/2-2.png?raw=true)

> Note : disini, sudah dilakukan instalasi whois dan sudah mengenkripsi "katasandi" dengan method sha-512.

Jika kita jalankan, maka akan membuat user baru di server yang dituju (appserver)
`ansible-playbook user-ade-add.yml`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/2-3.png?raw=true)

## Ansible : nginx Installation

**nginx.yml**

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/3-2.png?raw=true)

**ansible-playbook**

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/3.png?raw=true)

**Hasil**

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/3-1.png?raw=true)

## Ansible : Docker Installation

**docker.yml**

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/4-1.png?raw=true)

**ansible-playbook**

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/4.png?raw=true)

**Hasil**

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/4-2.png?raw=true)

## Monitoring Server : Data Source
Sebelumnya, kita akan menjalankan 3 instalasi dari service yang kita gunakan, yaitu :
- Node-exporter 
- Prometheus - metrics dari node-exporter agar bisa diakses menggunakan dashboard
- Grafana - dashboard

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/5-1.png?raw=true)

Jika instalasi sudah benar, kita bisa mengakses metrics melalui prometheus.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/6.png?raw=true)

Sekarang, kita juga bisa akses grafana melalui link dashboard yang sudah disediakan.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/6-1-0.png?raw=true)

Karena kita menggunakan prometheus, kita harus menambahkan data sourcenya, pliih opsi "Add a new Data Source" lalu pilih prometheus.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/6-1-1.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/6-1.png?raw=true)

Jika sudah sesuai, save konfigurasinya.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/6-2.png?raw=true)

Jika benar, maka akan di notif data source sudah bekerja dengan baik.

## Monitoring Server : Panel
Sekarang, kita tambahkan panel didalam grafana dengan memilih "Add a new Panel" lalu bisa kita lanjutkan dengan mengisi metrics yang ingin di ukur.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/6-3.png?raw=true)

Lalu kita bisa sesuaikan dengan kebutuhan kita, apa yang ingin dimonitoring dkk.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-6+7/media/6-4.png?raw=true)

> Metric - process/resource yang berjalan
> Labels - bisa digunakan untuk menggunakan container yang specific (melalui job di dalam prometheus.yml)
