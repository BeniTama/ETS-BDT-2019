# Tugas ETS

## Daftar Isi
1. [Instalasi Cluster mySQL](#1-instalasi-cluster-mysql)
2. [Instalasi Wordpress](#2-instalasi-wordpress)
3. [Simulasi Fail-over Pada Cluster](#3-simulasi-fail-over-pada-cluster)
4. [Pengukuran Response Time Menggunakan J-Meter](#4-pengukuran-response-time-menggunakan-jmeter)

## 1. Instalasi Cluster MySQL

Pada mySQL cluster ini, digunakan empat cluster, dengan keterangan sebagai berikut:

| No | IP Address | Hostname | Fungsi |
| --- | --- | --- | --- |
| 1 | 192.168.33.11 | clusterdb1 | Node Manager |
| 2 | 192.168.33.12 | clusterdb2 | Server 1 dan Node 1 |
| 3 | 192.168.33.13 | clusterdb3 | Server 2 dan Node 2 |
| 4 | 192.168.33.14 | clusterdb4 | Load Balancer (ProxySQL) |

Instalasi Wordpress dilakukan setelah melakukan instalasi dan konfigurasi untuk MySQL Cluster. Dokumentasinya dapat didapatkan pada tugas sebelumnya:
[Tugas 1: Implementasi MySQL Cluster](https://github.com/BeniTama/MySQL-Cluster-with-Load-Balancer)

## 2. Instalasi Wordpress

Langkah pertama yang harus dilakukan adalah membuat database baru pada cluster server, yaitu antara ``clusterdb2`` atau ``clusterdb3``. Kemudian diberikan hak akses terhadap user kepada kedua cluster.
![](/pictures/instalasi-1.PNG)

Kemudian dilakukan instalasi apache2 dan php pada ``clusterdb4`` sebagai kebutuhan Wordpress.
```
sudo apt-get install apache2
sudo apt-get install php -y
sudo apt-get install php-mysql
sudo apt-get install -y php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-snmp php-soap php-tidy curl
```

Kemudian copy tar instalasi wordpress kedalam clusterdb4 dan lakukan untar menggunakan perintah berikut:
```
sudo cp '/vagrant/install/wordpress-5.1.1.tar.gz' '/var/www/html/'
tar -xvf wordpress-5.1.1.tar.gz
```

Kemudian lakukan rename ``wp-config-sample.php`` menjadi ``wp-config.php`` menggunakan perintah:
```
sudo mv wp-config-sample.php wp-config.php
```
Kemudian buka filenya dan lakukan perubahan pada konfigurasi databasenya seperti gambar dibawah ini:
![](/pictures/instalasi-2.PNG)

Setelah itu ubah database engine pada ``wordpress\wp-admin\includes\schema.php`` menjadi ``ndb``.
![](/pictures/instalasi-3.PNG)

Setelah konfigurasi selesai dilakukan, instalasi wordpress dapat dimulai dengan membuka alamat ``http://192.168.33.14/wordpress`` pada browser. Kemudian, ikuti instruksi yang ditunjukkan.
![](/pictures/instalasi-4.PNG)

Setelah instalasi wordpress berhasil dilakukan, pengguna dapat mulai mengoperasikan wordpress.
![](/pictures/instalasi-5.PNG)

## 3. Simulasi Fail-over Pada Cluster

Dibagian ini akan dilakukan simulasi fail-over pada cluster. Dibawah ini adalah status bahwa semua node menyala dan terkoneksi dengan baik.

![](/pictures/testing-1.PNG)

Pengujian dilakukan dengan mematikan salah satu node. Pada kasus ini node clusterdb3 dimatikan.

![](/pictures/testing-2.PNG)

Ketika node clusterdb3 dimatikan, otomatis node yang berjalan adalah clusterdb2.

![](/pictures/testing-3.PNG)

Setelah melakukan posting saat clusterdb3 dimatikan, clusterdb3 dinyalakan kembali, kemudian clusterdb2 dimatikan.

![](/pictures/testing-4.PNG)

Saat membuka wordpress, hasil post saat clusterdb3 dimatikan masih dapat muncul. Hal ini berarti replikasi berhasil dilakukan.

![](/pictures/testing-5.PNG)

## 4. Pengukuran Response Time Menggunakan J-Meter

![](/pictures/jmeter-1.PNG)
![](/pictures/jmeter-2.PNG)
![](/pictures/jmeter-3.PNG)
![](/pictures/jmeter-4.PNG)
![](/pictures/jmeter-5.PNG)
