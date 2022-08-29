# CI/CD Integration melalui Cloudflare Pages
Cloudflare adalah salah satu CDN yang tersedia yang menyediakan **Cloudflare Pages**, dimana **Cloudflare Pages** digunakan untuk pembuatan front-end secara collaborative, dimana kita dapat menggunakan repository dari GIT untuk menjalankan pagenya.

# Deployment Cloudflare Pages
## Forking Repository Wayshub
Disini, kita akan melakukan Forking terlebih dahulu sebelum kita deploy page ke **Cloudflare Pages**.
Pertama, kita membuka repository **GitHub** yang ingin di fork.

Disini, kita pilih dropdown _Forks_ lalu _Create a new fork_.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/1.png?raw=true)

Lalu kita sesuaikan nama repository dan description yang diinginkan.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/2.png?raw=true)

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/3.png?raw=true)

Sekarang, kita akan menghubungkan repository GIT kita ke **Cloudflare Pages**.
Disini kita masuk ke dashboard Cloudflare, lalu pilih _Pages_ dan klik _Create a Project_.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/6.png?raw=true)

Karena disini kita akan menghubungkan repository Git, kita pilih opsi _Connect with Git_.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/7.png?raw=true)

Lalu pilih repository yang ingin di post. Disini kita memilih `wayshub-frontend`.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/8.png?raw=true)

Sesuaikan _branch_ dan _framework_ yang digunakan.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/9.png?raw=true)

Lalu kita pilih _Save & Deploy_.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/10.png?raw=true)

Kita akan masuk ke Log Deployment, sehingga kita bisa menunggu sampai Page berhasil terdeploy.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/11.png?raw=true)

Jika sudah berhasil, maka dibawah akan ada tulisan _Success_.

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/13.png?raw=true)

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/14.png?raw=true)

Hasil pembukaan web :

![](https://github.com/ademuh/devops13-dumbways-s1/blob/main/day-5/media/15.png?raw=true)
