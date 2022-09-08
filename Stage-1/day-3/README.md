# Application in Server
**Aplikasi / Application** merupakan sebuah software yang digunakan oleh user untuk menjalankan sebuah fungsi yang spesifik yang bisa berjalan secara mandiri, bersamaan dengan sekelompok program.

# Enviroment Application
## 1. NodeJS
Sebelum kita bisa menjalankan aplikasi berbasis **NodeJS**, kita harus membuat _enviroment_ di server kita. Gunakan command `mkdir nodejs` (directory) dan langsung pindah ke directorynya.

Setelah itu, run command `npm init -y` untuk menginstal `package.json`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/1-2.png?raw=true)

Lalu, kita gunakan sebuah script simpel untuk percobaan, dengan membuat file `index.js` dan gunakan `nano index.js` untuk membuat script _"Hello World"_.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/1-3.png?raw=true)

Kita juga akan install **ExpressJS** menggunakan Node Package Manager (**NPM**) menggunakan command :
`npm install express --save`

Lalu kita coba jalankan script yang kita sudah buat dengan command `node index.js`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/1-4.png?raw=true)

Di web browser akan terlihat seperti ini :

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/1-5.png?raw=true)
> Note : sebelumnya, command `sudo ufw allow 3000` sudah dijalankan agar port 3000 dapat diakses.

## 2. Golang
**Golang** atau biasa disebut **Go**, merupakan bahasa yang di develop di _Google_ yang memiliki level yang sama dengan _Java_.

Untuk menggunakan **Golang**, kita harus download enginenya dengan command :
1. `wget https://golang.org/dl/go1.16.5.linux-amd64.tar.gz && sudo su`
2. `rm -rf /usr/local/go && tar -C /usr/local -xzf go1.16.5.linux-amd64.tar.gz && exit`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/2.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/2-1.png?raw=true)

Sebelum kita bisa menjalankan Go, kita tambahkan PATH di `.bashrc`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/2-3.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/2-2.png?raw=true)

Untuk memastikan apakah Go terinstal, kita run command `go version`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/2-4.png?raw=true)

Lalu kita membuat script golang dengan `touch index.go` dan mengedit isi dari `index.go`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/2-6.png?raw=true)

Kita run scriptnya dengan menggunakan `go run index.go`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/2-7.png?raw=true)

## 3. Python3
**Python3** sudah di include didalam Ubuntu Server, sehingga kita tinggal cek versi yang kita miliki dengan `Python3 -V`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/3.png?raw=true)

Lalu kita install Package Manager dari Python3 agar kita bisa install `flask` agar kita dapat menjalankan app **Python3**.
Kita jalankan command `sudo apt install python3-pip`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/3-1.png?raw=true)

Lalu kita install `flask` melalui `PIP`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/3-2.png?raw=true)

Setelah itu kita membuat file `index.py` dan membuat script "Hello world!".

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/3-4.png?raw=true)
> Note : disini, `app.run()` diganti ke `app.run(host="0.0.0.0")` karena default dari _flask_ itu menggunakan localhost, bukan IP Address.

Kita jalankan script tersebut dengan command `Python3 index.py` lalu akses melalui IP dengan port 5000.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/3-5.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/3-6.png?raw=true)
> Note : sebelumnya, command `sudo ufw allow 5000` sudah dijalankan agar port 5000 dapat diakses.

# Menggunakan PM2 untuk Aplikasi
Agar aplikasi dalam server dapat berjalan terus, dibutuhkan sebuah process manager, disini kita akan menggunakan **PM2** untuk menjalankan aplikasi dari **NodeJS** dan **Python3**

Pertama, kita akan lakukan instalasi menggunakan command `npm install pm2@latest -g`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/4.png?raw=true)

Jika instalasi sudah selesai dengan baik, bisa kita check dengan command `pm2 -v`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/4-1.png?raw=true)

Untuk menjalankan sebuah aplikasi didalam **PM2**, kita tinggal menjalankan command `pm2 start <file>`. Karena **PM2** dirancang khusus **NodeJS**, kita bisa langsung menjalankan aplikasinya

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/4-2.png?raw=true)

Untuk menjalankan **Python3**, kita cukup menjalankan  `pm2 start index.py --interpreter=python3`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/5.png?raw=true)

Melihat gambar diatas, untuk menjalankan aplikasi melalui **localtunnel** kita tinggal input port yang digunakan (NodeJS 3000;Python 5000)
Sehingga, URL yang digenerate **localtunnel** dapat diakses secara publik.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-3/media/5-1.png?raw=true)

