# Managing Server with Terminal
## Terminal
**Terminal** merupakan sebuah *Command Prompt* atau _Shell_ yang digunakan untuk mengontrol file, membuat folder ataupun memberikan dan mengubah akses untuk user dan sebagainya.
Terminal biasanya berbasis **CLI** (Command Line Interface)

Keuntungan menguasai terminal salah satunya lebih cepat daripada menggunakan GUI dimana kita tidak menunggu dan bisa langsung meng-issue sebuah command dan lebih hemat resource karena hanya mengirimkan command text.

# BASH Scripting

Disini, kita bisa langsung membuat file untuk menjalankan script untuk _Update_ dan _Upgrade_ system.
Gunakan command `nano upsystem.sh` dan isi filenya dengan command di gambar.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/1.png?raw=true)

Untuk menjalankan scriptnya, cukup menggunakan command `sh upsystem.sh`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/2.png?raw=true)

# Source Code yang digunakan :

## Update dan Upgrade system
`
sudo apt update
sudo apt upgrade
`

## Firewall
`
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 442
`

# Text Manipulation
## cat
`cat [nama file]`
Digunakan untuk memeriksa isi file.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3.png?raw=true)

`cat > [nama file]`
Digunakan untuk menyisipkan text kedalam file.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3-2.png?raw=true)

`cat [file1] [file2] > [file3]`
Digunakan untuk menggabungkan kedua file1 dan file2.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3-3.png?raw=true)

## sed
`sed -i 's/[text1]/[text2]/g' [file]`
Merubah text1 ke text2 di file yang ditentukan.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3-4.png?raw=true)

## grep
`grep [text] [file]`
Mencari text didalam file (gunakan * untuk mencari di directory).

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3-5.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3-6.png?raw=true)

## sort
`sort [file]`
Digunakan untuk sort isi file secara ascending.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3-7.png?raw=true)

`sort -r [file]`
Digunakan untuk sort isi file secara descending.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3-8.png?raw=true)

## echo
`echo "text"`
Digunakan untuk print text pada CLI.

`echo "text" > [file]`
Digunakan untuk replace isi file dengan sisipan text.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3-9.png?raw=true)

`echo "text" >> [new file]`
Digunakan untuk membuat file baru dan mengisinya dengan text.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/3-10.png?raw=true)

# System Montioring
## htop
Installation
`sudo apt install htop`

Run
`htop`

**htop** hampir sama dengan _Task Manager_ di Windows, digunakan untuk memeriksa resource yang digunakan system.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/4.png?raw=true)

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/4-8.png?raw=true)

## nmon
Installation
`sudo apt install nmon`

Run
`nmon`

**nmon** memiliki fungsi yang sama dengan **htop**. Namun lebih detail keterangan resourcenya.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/4-2.png?raw=true)

## lsof
**lsof** merupakan singkatan *List open Files*, digunakan untuk menunjukan seluruh file yang sedang berjalan di background

`lsof`
Menunjukkan semua file yang sedang berjalan.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/4-3.png?raw=true)

`lsof -u [nama user]`
Menunjukkan semua file yang sedang berjalan di user.
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/4-7.png?raw=true)

`lsof -i :3000`
Menunjukkan semua file yang berjalan di port 3000.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/4-4.png?raw=true)

## ps
**ps** merupakan singkatan dari _Process Status_ dimana dia memberikan informasi proses yang berjalan

`ps -f -u ads`
Menunjukkan process yang berjalan di user ads.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/4-5.png?raw=true)

`ps -aux`
Menunjukkan process yang berjalan di system.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-6/media/4-6.png?raw=true)








