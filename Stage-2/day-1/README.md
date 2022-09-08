# Cloud Computing

## Instance w/ IDCH

Disini, kita akan membuat 2 server yaitu **appserver** dan **gateway**.
Pada website IDCH, kita pilih opsi **Virtual Server** dimana kita akan menjalankan server kita melalui _Cloud Server_ yang sudah disediakan.

Kita tinggal input username, password, nama instance dan juga resource yang ingin digunakan
> Username      : ads
> Nama Instance : ade-appserver;ade-gateway
> Resource      : 1 CPU, 1 GB RAM, 20 GB Disk

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-1/media/a.jpg?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-1/media/a(1).png?raw=true)

Melalui Terminal/SSH built-in di IDCH, kita bisa langsung mengakses server yang sudah dibuat.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-1/media/a(3).png?raw=true)

Karena kita ingin akses server melalui SSH, lalu memasukkan isi `id_rsa.pub` kedalam `authorized_keys` agar server bisa diakses menggunakan private key dan tanpa password.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-1/media/a(4).png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-1/media/a(5).png?raw=true)

Setelah semua konfigurasi selesai, kita dapat mengakses server melalui SSH.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-1/media/a(7).png?raw=true)

## Setup Front-End

1. `git clone <URL HTTPS Github>`
2. `curl` untuk instalasi nvm

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/a(8).png?raw=true)

3. `exec bash`
4. `nvm install 16`
5. `npm i pm2 -g`
6. `pm2 init simple` atau `pm2 ecosystem simple`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/a(9).png?raw=true)

7. `nano ecosystem.config.js`
8. Source Code :
      ```module.exports = { 
            apps : [{
              name    : "Dumbflix",
              script  : "npm start"
              }]
              }```
9. `pm2 start ecosystem.config.js`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/a(11).png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/a(10).png?raw=true)

## Setup dengan DNS Cloudflare

Disini, kita harus membuat address DNS kita terlebih dahulu. Kita masuk kedalam Dashboard Cloudflare lalu akses _Website_ yang ingin kita gunakan, lalu masuk ke menu _DNS_.
Tekan tombol "add record" lalu isi sesuai dengan sub-domain dan IP appserver kita.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/b.png?raw=true)

Sehingga, website kita bisa diakses dengan sub-domain dan IPv4 (A) yang telah ditentukan.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-2/day-2/media/a(17).png?raw=true)
