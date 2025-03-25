**1.Latar Belakang**

Dalam dunia pengelolaan data, database management system (DBMS) menjadi komponen penting dalam berbagai aplikasi. MySQL adalah salah satu database relational (RDBMS) yang banyak digunakan karena keandalannya dalam menangani data secara terstruktur. Oleh karena itu, dalam tugas ini akan dilakukan instalasi dan konfigurasi MySQL, termasuk mengubah pengaturan default dan mengamankan database.

**2. Problem yang Diangkat**
Bagaimana cara menginstal dan mengonfigurasi server database MySQL dengan benar, termasuk:

- Perbedaan database relational dan unrelational serta kapan masing-masing digunakan.

- Instalasi MySQL dari awal.

- Perubahan konfigurasi default, seperti port dan buffer pool size.

- Mengamankan database dengan mengubah password root.

- Membuat database kelompok_AB_nama_mhs menggunakan command prompt.


**3.Solusi / Skenario Aktivitas**

1. Database Relational (RDBMS)
Database relational menyimpan data dalam bentuk tabel dengan relasi antar tabel.

Contoh: MySQL, PostgreSQL, MariaDB

Digunakan ketika data memiliki struktur yang jelas dan membutuhkan transaksi ACID.

2. Database Unrelational (NoSQL)
Database unrelational menyimpan data dalam bentuk fleksibel seperti dokumen, key-value, atau graph.

Contoh: MongoDB, Redis, Cassandra

Digunakan ketika data tidak memiliki struktur tetap atau membutuhkan skalabilitas tinggi.


**4.Pembahasan**

**Hasil Konfigurasi dan Uji Coba**
- 1.Port berhasil diubah ke 3309, dibuktikan dengan:
  
  **netstat -an | find "3309"**

- 2.Buffer pool size berhasil diperbesar, dicek dengan perintah:


  **SHOW VARIABLES LIKE 'innodb_buffer_pool_size';**

- 3.Password root berhasil diubah, diverifikasi dengan login ulang.

- 4.Database berhasil dibuat, diverifikasi dengan perintah:

  **SHOW DATABASES;**

**5. Kesimpulan**
- Database relational digunakan untuk data terstruktur, sementara database unrelational cocok untuk data fleksibel.

- Instalasi MySQL berjalan dengan baik dan server dikonfigurasi sesuai kebutuhan.

- Konfigurasi berhasil diterapkan, termasuk perubahan port, buffer pool, dan password root.

- Database berhasil dibuat menggunakan command prompt.

**6.Bukti Dukung**
- Sebelum Perubahan Port
  
  ![image](https://github.com/user-attachments/assets/6e7364a7-75b2-4f73-ae46-3d8cba785a36)

- Sesudah Perubahan Port

  ![image](https://github.com/user-attachments/assets/34545e82-3349-4ff3-95f9-c4787764bfd4)


- Sebelum ubah buffer pool size(default)

  ![image](https://github.com/user-attachments/assets/4b554112-6c89-49b0-8aa2-6222e7d7104b)

- Sesudah Ubah buffer pool size(ubah menjadi 4G)

  ![image](https://github.com/user-attachments/assets/f774956b-bd8f-4323-8252-23573f040da9)


- Verifikasi Database Yang Sudah DIbuat

  ![image](https://github.com/user-attachments/assets/807c525c-0084-434f-ac6b-76a81ec7a7b2)

**7.Referensi**
- https://dev.mysql.com/doc/refman/8.0/en/installing.html




