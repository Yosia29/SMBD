**1.Latar Belakang**

Dalam pengelolaan database, bottleneck dalam MySQL dapat menyebabkan performa query yang lambat dan inefisien. Bottleneck ini sering terjadi akibat:

Kurangnya indexing pada kolom yang sering digunakan dalam WHERE atau JOIN.

Full table scan, yang menyebabkan pencarian data menjadi lambat.

Query yang tidak optimal, misalnya penggunaan fungsi dalam kondisi WHERE.

Buffer pool yang tidak cukup besar, sehingga memperlambat akses data.

Kurangnya penggunaan query cache, yang dapat mengurangi eksekusi ulang query yang sama.

Tugas ini bertujuan untuk mengoptimalkan performa database dengan menerapkan teknik indexing, query optimization, query cache, dan buffer pool tuning.

**2. Problem yang Diangkat**

Bagaimana cara meningkatkan efisiensi eksekusi query pada database minimarket dengan:

- Menambahkan index pada kolom yang sering digunakan dalam pencarian.

- Menggunakan query cache untuk mempercepat eksekusi ulang query yang sama.

- Mengoptimalkan query dengan EXPLAIN untuk menemukan bottleneck.

- Menguji perbedaan performa sebelum dan sesudah optimasi.

**3.Solusi / Skenario Aktivitas**

**Teknik Optimasi yang Diterapkan**

- Menggunakan Indexing → Mempercepat pencarian data dengan indeks.

- Query Optimization → Menulis query yang lebih efisien.

- Query Cache → Menyimpan hasil query agar lebih cepat saat dieksekusi ulang.

- Buffer Pool Tuning → Mengatur innodb_buffer_pool_size agar lebih optimal.

- Analisis EXPLAIN → Memeriksa bagaimana MySQL menjalankan query untuk menemukan bottleneck.

**4.Pembahasan**

**A. Menambahkan Index pada tr_penjualan_raw**

- Import database employee.sql menggunakan perintah:

  CREATE INDEX idx_tgl_transaksi ON tr_penjualan_raw(tgl_transaksi); 
CREATE INDEX idx_kode_item ON tr_penjualan_raw(kode_item); 
CREATE INDEX idx_nama_kasir ON tr_penjualan_raw(nama_kasir); 
CREATE INDEX idx_harga ON tr_penjualan_raw(harga); 

**B. Menggunakan Query Cache**

1. Menjalankan Query Tanpa Index

  SHOW VARIABLES LIKE 'query_cache_size';


2. Menambahkan Index pada first_name dan last_name

   ALTER TABLE employee ADD INDEX idx_full_name (first_name, last_name);

3. Menjalankan Query Setelah Index Ditambahkan

   EXPLAIN SELECT * FROM employee WHERE first_name = 'Georgi' AND last_name = 'Bahr';

**C. Optimasi Query**

1. Query Filter Berdasarkan Tahun Transaksi

   SELECT * FROM tr_penjualan_raw WHERE YEAR(tgl_transaksi) = 2024;


2. Query dengan IN untuk Banyak Item

 SELECT * FROM tr_penjualan_raw WHERE kode_item IN ('ITEM1', 'ITEM2', 'ITEM3', ..., 'ITEM500');

3. Query LIKE dengan Wildcard (%)

SELECT * FROM tr_penjualan_raw WHERE nama_kasir LIKE '%John%';


**D.Optimasi Query MAX(harga)**

1. Query sebelum optimasi:

 SELECT MAX(harga) FROM tr_penjualan_raw WHERE kode_cabang = 'CB001';


2. Tambahkan Index pada kode_cabang
CREATE INDEX idx_kode_cabang ON tr_penjualan_raw (kode_cabang);


3. Tambahkan Index pada harga

   CREATE INDEX idx_harga ON tr_penjualan_raw (harga);



4.Gunakan Index dalam Query

SELECT MAX(harga) FROM tr_penjualan_raw WHERE kode_cabang = 'CB001';


**5. Kesimpulan**

Dari hasil pengujian, diperoleh beberapa kesimpulan:

- Indexing sangat efektif dalam meningkatkan performa query berbasis pencarian.

- Query cache dapat mempercepat eksekusi ulang query yang sering digunakan.

- Fungsi dalam WHERE seperti YEAR() harus dihindari karena menghambat penggunaan index.

- Wildcard (% di awal LIKE statement) menyebabkan full table scan, sehingga FULLTEXT index lebih disarankan.

- Index pada harga dan kode_cabang mempercepat query MAX(harga) secara signifikan.


**6.Bukti Dukung**
- Hasil Waktu Eksekusi Query

![image](https://github.com/user-attachments/assets/5cef316a-5b99-41ab-a83a-be548fb4d9d1)



  


**7.Referensi**
- https://dev.mysql.com/doc/refman/8.0/en/optimization.html
