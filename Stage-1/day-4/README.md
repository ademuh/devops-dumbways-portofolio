# Version Control System
**GIT** merupakan salah satu Version Control System yang paling umum digunakan. Menggunakan **distributed revision control**, database dari GIT akan disimpan online dan di PC masing-masing developer.

# Repository Server GitHub
## 1. Config SSH ke GitHub
Disini, karena kita menggunakan Git kita butuh repository online, dengan menggunakan **GitHub** kita akan membuat config agar dapat melakukan segala kebutuhan GIT melalui server.

Pertama, kita menjalankan command `git config --global user.name <username github>` dan `git config --global user.email <email github>`
Lalu, di check melalui command `git config --list` untuk memastikan apakah username dan email sudah terdaftar.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/1.png?raw=true)

Disini saya akan menggunakan SSH untuk mengendalikan git, maka dari itu saya akan genereate sebuah key SSH dengan command `ssh-keygen`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/1-1.png?raw=true)

Untuk mendapatkan key dari SSH untuk integrasi ke **GitHub**, kita langsung gunakan command `cat .ssh/id_rsa.pub`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/1-2.png?raw=true)
> Note : ditekankan untuk menggunakan key dengan format .pub, karena kita hanya membutuhkan Public Key

Setelah itu, masuk ke link https://github.com/settings/keys untuk mengisi public key SSH.
Lalu klik tombol **"New SSH Key"**

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/1-3.png?raw=true)

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/1-4.png?raw=true)

Lalu kita jalankan command `ssh -T git@github.com` (Pilih "yes" saat diminta)

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/1-5.png?raw=true)

Jika sudah, disini akun SSH kita sudah terhubung dengan **GitHub** sehingga kita bisa mengatur GIT langsung dari CLI.

## 2. Git Repository
Disini, kita akan mencoba membuat repository untuk aplikasi **NodeJS** yang sudah kita buat.
Pertama kita masuk ke directory `nodejs` lalu menggunakan `git init` agar kita bisa initialize git di directory tersebut.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2.png?raw=true)

Disini, kita cek menggunakan `git status`. Disini semua file yang ada di nodejs akan masuk ke _Untracked Files_ karena belum di masukkan ke tahap _Staging_.
Karena ada directory `node_modules` didalamnya, kita masukkan ke `.gitignore` agar pada saat kita mengurus repository tidak akan dibawa.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2-1.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2-2.png?raw=true)

Lalu, kita gunakan `sudo nano .gitignore` untuk mengisi directory/file yang akan diabaikan oleh GIT.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2-3.png?raw=true)

Jika sudah melakukan langkah diatas, pada saat kita check kembali `node_modules` sudah tidak ada dan akan muncul `.gitignore`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2-4.png?raw=true)

Sekarang, kita jalankan `git add .` agar semua file di majukan ke tahap _Staging_.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2-5.png?raw=true)

Jika sudah siap untuk _Commit_, maka kita jalankan command `git remote add origin <url-repo-github>`.
Bisa kita cek juga apakah URL remote sudah sesuai dengan `git remote -v`

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2-6.png?raw=true)
![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2-7.png?raw=true)

Karena repository sudah siap, maka kita akan melakukan _Commit_ dengan `git commit -m "message commit"`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2-8.png?raw=true)

Karena sudah di _Commit_, kita _Push_ repository kita dengan command `git push <remote> <branch>` agar repository dapat dipush ke GitHub.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/2-9.png?raw=true)

Sekarang, kita akan menambahkan _branch_ dengan nama _Development_, _Staging_ dan _Production_ dengan menggunakan command `git branch <nama branch>`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/3.png?raw=true)

Jika sudah, bisa kita check _branch_ yang sudah dibuat dengan command `git branch -a`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/3-1.png?raw=true)

Kalau ingin ganti _branch_ yang digunakan, bisa menggunakan command `git checkout <branch>`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/3-2.png?raw=true)

# GIT commands

| Command   |      Fungsi  | Usage |
|:----------:|-------------|--------------------|
| **clean** | Membersihkan **Untracked Files** yang ada di directory repository  | git clean [Di dalam repository] |
| **fetch** | Sama dengan _pull_, namun tidak akan melakukan merge otomatis kedalam repository local | git fetch <remote> <branch> / git fetch --all|
| **log** | Mengcek history perubahan git tersebut  | git log [Di dalam repository] |

## git clean

Disini, ada file bernama `untracked`, git clean akan membersihkan file yang tidak masuk ke tahap _Staging_

1. `git status` untuk melihat file yang _Untracked_.
2. menjalankan `git clean -n` untuk mengetahui file apa aja yang akan dihapus.
3. menjalankan 'git clean -f' (-f = force, memaksakan untuk menghapus file untracked)

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/5.png?raw=true)

## git fetch

Fetch bersifat sama dengan _pull_ namun ia tidak langsung melakukan merge melainkan memberikan kita apa yang kira-kira akan dirubah jika kita melakukan _pull_

1. Disini, sudah ada file `fetch_test` didalam branch `master` dan saya menggunakan branch `development`
2. menjalankan command `git fetch origin master` untuk mendapatkan informasi apa yang berubah.
3. memfinalisasikan perubahan dengan `git pull origin master`.

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/6.png?raw=true)

## git log

Disini, `git log` memberikan informasi perubahan pada repository itu beserta messagenya. 

![](https://github.com/ademuh/devops13-dumbways-ade/blob/main/Stage-1/day-4/media/7.png?raw=true)
