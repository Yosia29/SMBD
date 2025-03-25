**1.Latar Belakang**

Dalam database dengan jumlah data yang besar, partisi (partitioning) adalah teknik yang berguna untuk meningkatkan performa query dengan membagi tabel menjadi beberapa bagian yang lebih kecil. Pada tugas ini, kita akan menerapkan partisi berdasarkan tahun transaksi pada tabel tr_penjualan, kemudian menguji perbedaan performa antara tabel yang menggunakan partisi dan yang tidak.

- Menambahkan index pada kolom pencarian.

- Menggunakan composite index untuk meningkatkan efisiensi query.

- Melakukan analisis performa query sebelum dan sesudah indexing.

**2. Problem yang Diangkat**

Bagaimana cara meningkatkan efisiensi eksekusi query pada database tr_penjualan dengan:

- Membuat tabel dengan partisi berdasarkan tahun transaksi.

- Mengisi tabel partisi dengan data dari tabel utama.

- Mengukur perbedaan performa antara tabel partisi dan tabel biasa.

- Melakukan pengujian query dengan berbagai kondisi pencarian.

**3.Solusi / Skenario Aktivitas**

Langkah-langkah yang dilakukan dalam tugas ini:

- Membuat tabel tr_penjualan_partisi dengan partisi berdasarkan tahun transaksi.

- Mengisi tabel partisi dengan data yang sesuai.

- Membandingkan performa query antara tabel partisi dan tabel biasa (tr_penjualan_raw).

- Melakukan uji coba dengan berbagai kondisi pencarian.

**4.Pembahasan**

**A. Pembuatan Tabel dengan Partisi**

- Tabel tr_penjualan_partisi dibuat dengan partisi berdasarkan tahun transaksi menggunakan PARTITION BY RANGE::

  CREATE TABLE tr_penjualan_partisi ( 
    tgl_transaksi DATETIME DEFAULT NULL, 
    kode_cabang VARCHAR(10) DEFAULT NULL, 
    kode_kasir VARCHAR(10) DEFAULT NULL, 
    kode_item VARCHAR(7) DEFAULT NULL, 
    kode_produk VARCHAR(12) DEFAULT NULL, 
    jumlah_pembelian INT(11) DEFAULT NULL, 
    nama_kasir VARCHAR(40) DEFAULT NULL, 
    harga INT(6) DEFAULT NULL 
) 
PARTITION BY RANGE (YEAR(tgl_transaksi)) ( 
    PARTITION p0 VALUES LESS THAN (2008), 
    PARTITION p1 VALUES LESS THAN (2009), 
    PARTITION p2 VALUES LESS THAN (2010), 
    PARTITION p3 VALUES LESS THAN (2011), 
    PARTITION p4 VALUES LESS THAN (2012), 
    PARTITION p5 VALUES LESS THAN (2013), 
    PARTITION p6 VALUES LESS THAN (2014), 
    PARTITION p7 VALUES LESS THAN (2015) 
);

**B. B. Mengisi Data ke dalam Partisi**

1. Memasukkan Data dari tr_penjualan ke dalam tr_penjualan_partisi

   INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item, 
kode_produk, jumlah_pembelian, nama_kasir, harga) 
SELECT tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, 
nama_kasir, harga 
FROM tr_penjualan 
WHERE YEAR(tgl_transaksi) = 2011;

**C. Membandingkan Performansi Query Antara Tabel Partisi dan Tabel Biasa**

1. Membuat Tabel tr_penjualan_raw (Tanpa Partisi)

  CREATE TABLE tr_penjualan_raw ( 
    tgl_transaksi DATETIME DEFAULT NULL, 
    kode_cabang VARCHAR(10) DEFAULT NULL, 
    kode_kasir VARCHAR(10) DEFAULT NULL, 
    kode_item VARCHAR(7) DEFAULT NULL, 
    kode_produk VARCHAR(12) DEFAULT NULL, 
    jumlah_pembelian INT(11) DEFAULT NULL, 
    nama_kasir VARCHAR(40) DEFAULT NULL, 
    harga INT(6) DEFAULT NULL 
);


2. Kemudian, isi tabel dengan data yang sama dari tr_penjualan_partisi:

   INSERT INTO tr_penjualan_raw 
SELECT * FROM tr_penjualan_partisi;


**D. Pengujian Performansi Query**

1. Pengujian Query Berdasarkan tgl_transaksi
   - Query berikut diuji 10 kali dan dicatat rata-rata waktunya:

-- Query pada tabel tanpa partisi
SELECT * FROM tr_penjualan_raw 
WHERE tgl_transaksi > DATE('2010-08-01') 
AND tgl_transaksi < DATE('2011-07-31');

-- Query pada tabel dengan partisi
SELECT * FROM tr_penjualan_partisi 
WHERE tgl_transaksi > DATE('2010-08-01') 
AND tgl_transaksi < DATE('2011-07-31');


2. Pengujian Query Berdasarkan jumlah_pembelian

  -- Query pada tabel tanpa partisi
SELECT * FROM tr_penjualan_raw WHERE jumlah_pembelian > 5;

-- Query pada tabel dengan partisi
SELECT * FROM tr_penjualan_partisi WHERE jumlah_pembelian > 5;

3. Menambahkan Kolom umur pada Tabel employee dan Mengisinya Secara Otomatis

   ALTER TABLE employee ADD COLUMN umur INT;
UPDATE employee SET umur = TIMESTAMPDIFF(YEAR, birth_date, CURDATE());


**5. Kesimpulan**

- Partisi sangat efektif untuk query yang berbasis tgl_transaksi, karena hanya membaca partisi tertentu.

- Query tanpa partisi membutuhkan full table scan, sehingga lebih lambat.

- Partisi tidak memberikan keuntungan signifikan jika query tidak menggunakan kolom yang dijadikan dasar partisi.

- Indeks masih diperlukan untuk optimasi query yang tidak memanfaatkan partisi.


**6.Bukti Dukung**
- Pembuatan tabel partisi

 ![image](https://github.com/user-attachments/assets/8356fe6b-d63b-4194-9be4-1196cffe8698)

- hasil query sebelum partisi
  
  ![image](https://github.com/user-attachments/assets/9a9ac20b-a7a8-44f1-8f01-920cdbd23d3b)


- hasil query sesudah partisi

![image](https://github.com/user-attachments/assets/571621c5-b44a-403c-9e82-4dafc7dc8212)


  


**7.Referensi**
- https://dev.mysql.com/doc/refman/8.0/en/explain-output.html
