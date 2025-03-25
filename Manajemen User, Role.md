**1.Latar Belakang**

Dalam sistem database, pengelolaan user dan hak akses sangat penting untuk memastikan bahwa data hanya dapat diakses oleh pengguna yang memiliki izin tertentu. Oleh karena itu, pada pertemuan ini, akan dipelajari cara mengelola akun pengguna, memberikan hak akses (privilege), membuat role, serta melakukan monitoring aktivitas pengguna dalam MySQL.

**2. Problem yang Diangkat**

Bagaimana cara mengimplementasikan manajemen user, role, dan privilege dalam MySQL agar:

- User hanya memiliki akses yang sesuai dengan kebutuhannya.

- Database lebih aman dengan sistem role-based access control.

- Administrasi database lebih mudah melalui role.

- Monitoring aktivitas pengguna dapat dilakukan secara efektif.


**3.Solusi / Skenario Aktivitas**

Solusi yang diterapkan meliputi:

- Membuat dan menghapus user dalam MySQL.

- Memberikan privilege ke user secara langsung atau melalui role.

- Mengelola role untuk mempermudah pengelolaan hak akses.

- Memonitor aktivitas pengguna menggunakan general log.


**4.Pembahasan**

**A. Manajemen Akun Pengguna (User Management)**
  
 **1. Membuat User Baru**

 CREATE USER 'user_example'@'localhost' IDENTIFIED BY 'password123';


**2. Menghapus User**

DROP USER 'user_example'@'localhost';

**B. Manajemen Hak Akses (Privilege Management)**

1. Memberikan Hak Akses Langsung ke User

- memberikan akses SELECT dan INSERT ke user:

  GRANT SELECT, INSERT ON database_example.* TO 'user_example'@'localhost';

2. Menghapus Hak Akses dari User

REVOKE INSERT ON database_example.* FROM 'user_example'@'localhost';

3. Menerapkan Perubahan Hak Akses

   FLUSH PRIVILEGES;

**C. Manajemen Role dalam MySQL**

1. Membuat Role Baru

   CREATE ROLE 'role_developer';

2. Memberikan Hak Akses ke Role

GRANT SELECT, INSERT ON database_example.* TO 'role_developer';

3. Memberikan Role ke User

   GRANT 'role_developer' TO 'user_example'@'localhost';

4. Menghapus Role dari User

   REVOKE 'role_developer' FROM 'user_example'@'localhost';

**D. Monitoring Akses dan Aktivitas Pengguna**

1. Mengaktifkan General Log untuk Memonitor Aktivitas

   SET global general_log = 1;
SET global log_output = 'table';

2. Melihat Log Aktivitas Pengguna

   SELECT * FROM mysql.general_log;


**5. Kesimpulan**
- Manajemen user dalam MySQL memungkinkan admin untuk membuat dan menghapus user sesuai kebutuhan.

- Privilege dapat diberikan langsung ke user atau melalui role untuk kemudahan administrasi.

- Role mempermudah pengelolaan hak akses, terutama dalam organisasi yang memiliki banyak pengguna.

- Monitoring aktivitas pengguna dengan general log memungkinkan DBA untuk mengawasi query yang dijalankan dan mengidentifikasi aktivitas mencurigakan.



**6.Bukti Dukung**

📷 Screenshot Hasil Pembuatan User

![image](https://github.com/user-attachments/assets/9611a174-afab-4aa2-b1b1-96e4ca64d632)

📷 Screenshot Hasil Pembuatan Role

![image](https://github.com/user-attachments/assets/bce054d4-b3b1-4ec9-8580-51d5c4a2057a)

📷 Screenshot Log Aktivitas Pengguna

![image](https://github.com/user-attachments/assets/c7f02920-aeaa-48ac-9ecb-bc23b7ab0408)




**7.Referensi**
- [https://dev.mysql.com/doc/refman/8.0/en/installing.html](https://dev.mysql.com/doc/refman/8.0/en/roles.html)
