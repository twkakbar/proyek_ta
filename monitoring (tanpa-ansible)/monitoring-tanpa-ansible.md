
# 3. Node Exporter

**Node Eksporter** adalah perangkat lunak yang di gunakan tepat di samping aplikasi yang ingin diperoleh metriknya. dengan kata lain Node Eksporter berfungsi untuk mengambil suatu informasi yang nantinya akan diterima oleh sistem monitoring seperti `Prometheus`, mengumpulkan data yang diperlukan dari aplikasi, mengubahnya menjadi format yang lebih mudah, kemudian mengembalikannya sebagai respons terhadap sistem monitoring.

## Instalasi Node Exporter 

Sekarang kita akan melakukan instalasi **node exporter** untuk landasan monitoring server dengan prometheus dan grafana yang akan kita buat nantinya. Untuk melakukan instalasi kalian dapat mengikuti langkah-langkah di bawah ini :

- Masuk ke dalam server dan lakukan update dan upgrade.

  ```
  sudo apt update; sudo apt upgrade
  ```

- Selanjutnya kita lakukan instalasi terlebih dahulu **node exporter**nya.

  :::
  Lakukan instalasi pada ketiga buah server yang telah kalian buat.
  :::

  ```
  wget https://github.com/prometheus/node_exporter/releases/download/v0.15.2/node_exporter-0.15.2.linux-amd64.tar.gz
  ```


  ![Img 1](assets/1.png)


- Selanjutnya extract `node exporter` yang telah kalian lakukan instalasi.

  ```
  tar -xf node_exporter-0.15.2.linux-amd64.tar.gz
  ```


  ![Img 1](assets/2.png)


- Selanjutnya pindahkan isi dari `node exporter` yang telah kalian install ke dalam `/usr/local/bin`.

  ```
  sudo mv node_exporter-0.15.2.linux-amd64/node_exporter /usr/local/bin
  ```


  ![Img 1](assets/3.png)


- Sekarang kita tambahkan user untuk `node exporter` yang telah kita pindahkan sebelumnya.

  ```
  sudo useradd -rs /bin/false node_exporter
  ```


  ![Img 1](assets/4.png)


- Selanjutnya buat file konfigurasi pada `/etc/systemd/system/` dengan nama `node_exporter.service`, setelah itu masukkan konfigurasi berikut ini :

  ![Img 1](assets/5.png)

  ![Img 1](assets/6.png)

  ```
  sudo nano /etc/systemd/system/node_exporter.service
  ```

  ```
  [Unit]
  Description=Node Exporter
  After=network.target

  [Service]
  User=node_exporter
  Group=node_exporter
  Type=simple
  ExecStart=/usr/local/bin/node_exporter

  [Install]
  WantedBy=multi-user.target
  ```

  keterangan :
  [Unit] adalah nama dari si aplikasinya
  [Service] adalah informasi dari aplikasi
  [Install] adalah informasi untuk `node exporter` bisa di jalankan oleh user lain selain `node exporter`



- Karena kita tadi telah menambahkan konfigurasi untuk file service `node exporter`, sekarang kita akan melakukan reload untuk si servicenya. Untuk perintahnya kalian dapat menggunakan perintah dibawah ini :

  ```
  sudo systemctl daemon-reload
  ```

![Img 1](assets/55.png) 
  
  

- Sekarang kita tinggal menghidupukan `node exporter` kita.

  ```
  sudo systemctl enable node_exporter
  ```

  
  ![Img 1](assets/7.png)
  

- Karena kita pertama kali membuat `node exporter` ini, sekarang kita akan menjalankan service dari si `node exporter`.

  ```
  sudo systemctl start node_exporter
  ```

  
  ![Img 1](assets/8.png)
  

- Jika tahapan diatas telah kalian jalankan sekarang kita coba cek apakah `node exporter` kita telah berjalan atau tidak.

  ```
  sudo systemctl status node_exporter
  ```


  ![Img 1](assets/9.png)
 

- Sekarang kita coba akses `node exporter` kita pada web.browser. Node exporter berjalan di atas port:9100

  http://(your server IP):9100


  ![Img 1](assets/10.png)


- Sekarang coba kalian klik pada bagian `metrics` selanjutnya kalian akan diarahkan ke menu seperti gambar dibawah ini, disini kalian dapat melihat sistem yang sedang berjalan maupun apa saja yang sedang dijalankan tetapi tampilannya kurang mengenakkan makannya disini kita perlu prometheus agar dapat membaca kinerja sistem kita secara mudah.

  
  ![Img 1](assets/11.png)


 
