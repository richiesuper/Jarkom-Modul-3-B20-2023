# Jarkom-Modul-3-B20-2023

## Informasi Kelompok

| Nama | NRP |
| ---- | --- |
| Richie Seputro | 5025211213 |
| Dimas Aria Pujangga | 5025211212 |

## Topologi
![topologi](assets/topologi.png)

0. Dimas

1. Dimas

2. Dimas

3. Dimas

4. Dimas

5. Dimas

6. Dimas

7. Dimas

8. Penjelasan untuk soal nomor 8 ada pada Grimoire pada link berikut:
   [Grimoire B20](https://docs.google.com/document/d/1nBn-T3pYLUuw57cAKFyD8BxiqECJ8dVF7kLpJApF_To/edit?usp=sharing)

9. Penjelasan untuk soal nomor 9 ada pada Grimoire pada link berikut:
   [Grimoire B20](https://docs.google.com/document/d/1nBn-T3pYLUuw57cAKFyD8BxiqECJ8dVF7kLpJApF_To/edit?usp=sharing)

10. Pertama, kita buat dulu folder `/etc/nginx/rahasisakita` dengan command `mkdir /etc/nginx/rahasisakita`.
    Lalu, lakukan command `htpasswd -cb /etc/nginx/rahasisakita/.htpasswd netics ajkb20` untuk membuat file autentikasi.
    Lalu, tambahkan baris-baris berikut ke *vhost* file dari `nginx` pada `/etc/nginx/sites-available/default`:
    ```
    location / {
        auth_basic "Frieren Private Club";
        auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;
    }
    ```
    Selanjutnya, akses laman website yang disediakan oleh load balancer Eisen dengan `lynx`:

    ![10a](img/10a.png)

    ![10b](img/10b.png)

    ![10c](img/10c.png)

    ![10d](img/10d.png)  
    
    ![10e](img/10e.png)

11. Pertama, tambahkan baris-baris berikut ke file `/etc/nginx/sites-available/default`:
    ```
    location ~ (.*)+/its(.*)+ {
        proxy_pass http://www.its.ac.id;
    }
    ```
    Jika kita melakukan request ke website dengan URL yang mengandung "/its" maka akan di-redirect ke laman milik ITS:

    ![11a](img/11a.png)
    
    ![11b](img/11b.png)
    
    ![11c](img/11c.png)

12. Pertama, tambahkan konfigurasi berikut ke `/etc/nginx/sites-available/default`:

    ![12](img/12.png)

    Selanjutnya, kita cek dulu IP address dari salah satu client dari masing-masing subnet x.x.3.x yaitu Revolte dan untuk subnet x.x.4.x yaitu Sein. Dicontohkan di sini menggunakan client Sein (untuk client Revolte juga sama persis langkah-langkahnya).

    ![12a](img/12a.png)
    
    Lalu, kita coba untuk akses Load Balancer dengan IP address Sein yang bukan merupakan IP address yang di-whitelist.
    
    ![12b](img/12b.png)
    
    ![12c](img/12c.png)
    
    Terlihat bahwa client Sein dilarang mengakses website.
    
    Ubah dulu IP address dari Sein menjadi IP address statik yang sudah di-whitelist.
    
    ![12d](img/12d.png)
    
    Coba ulang mengunjungi website.

    ![12e](img/12e.png)
    
    ![12f](img/12f.png)
    
    Terlihat bahwa Sein telah diperbolehkan untuk mengakses website.

    Ulangi langkah-langkah di atas untuk client Revolte.

13. Pertama, tambahkan command berikut ke `/root/.bashrc` atau jalankan secara manual di host Denken:

    ![13a](img/13a.png)
    
    Jangan lupa ubah bind-address dari server MariaDB ke `0.0.0.0` di file `/etc/mysql/mariadb.conf.d/50-server.cnf`.

    Setelah itu buatlah database dan user-user yang akan digunakan oleh worker Laravel dengan menjalankan perintah-perintah berikut di host Denken:

    ```
    mysql -u root -p
    Enter password: <masukkan password>

    CREATE USER 'kelompokb20'@'%' IDENTIFIED BY 'passwordb20';
    CREATE USER 'kelompokb20'@'localhost' IDENTIFIED BY 'passwordb20';
    CREATE DATABASE dbkelompokb20;
    GRANT ALL PRIVILEGES ON *.* TO 'kelompokb20'@'%';
    GRANT ALL PRIVILEGES ON *.* TO 'kelompokb20'@'localhost';
    ```
    
    Kemudian lakukan restart service MariaDB dengan `service mysql restart`.
    
    Terakhir, cek dari worker Laravel apakah bisa meng-issue command ke database di Denken.
    
    ![13b](img/13b.png)
    
    Terlihat bahwa Denken dapat diakses di Fern.

14. Kelompok kami mengalami kendala di nomor ini yang mengakibatkan kami tidak bisa lanjut mengerjakan nomor-nomor selanjutnya. Berikut penjelasannya:

    Pertama, jalankan command atau tambahkan command ke `/root/.bashrc` dari masing-masing worker Laravel yaitu Fern, Flamme, dan Frieren sebagai berikut:

    ![14a](img/14a.png)
    
    Namun, terjadi error di ketiga worker Laravel tersebut di bagian `composer install`. Berikut error message dari masing masing worker:

    Fern:

    ![14b](img/14b.png)
    
    Flamme:

    ![14c](img/14c.png)
    
    Frieren:

    ![14d](img/14d.png)
    
    Hal ini mengakibatkan Laravel project tidak dapat di-install sama sekali dan menjadi kendala kelompok kami.

15. Terkendala nomor 14.

16. Terkendala nomor 14.

17. Terkendala nomor 14.

18. Terkendala nomor 14.

19. Terkendala nomor 14.

20. Terkendala nomor 14.
