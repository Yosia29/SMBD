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



**4.Pembahasan**

**A. Proses Import Database Employee**

- Import database employee.sql menggunakan perintah:

  SOURCE /path/to/employee.sql;

-Verifikasi hasil import dengan:

SELECT * FROM employee

**B. Penggunaan Indexing untuk Optimasi Query**

1. Menjalankan Query Tanpa Index

   EXPLAIN SELECT * FROM employee WHERE first_name = 'Georgi';

2. Menambahkan Index pada first_name dan last_name

   ALTER TABLE employee ADD INDEX idx_full_name (first_name, last_name);

3. Menjalankan Query Setelah Index Ditambahkan

   EXPLAIN SELECT * FROM employee WHERE first_name = 'Georgi' AND last_name = 'Bahr';

**C. Pengujian Performa Query**

1. Sebelum Index Ditambahkan

   SELECT * FROM employee WHERE first_name = 'Georgi' AND last_name = 'Bahr';

2. Sesudah Index Ditambahkan

   ALTER TABLE employee ADD INDEX idx_full_name (first_name, last_name);
SELECT * FROM employee WHERE first_name = 'Georgi' AND last_name = 'Bahr';

**D. Menambahkan Kolom dan Optimasi Lainnya**

1. Menambahkan Kolom nama_departemen pada dept_manager dan dept_emp

 ALTER TABLE dept_manager ADD COLUMN nama_departemen VARCHAR(255);
ALTER TABLE dept_emp ADD COLUMN nama_departemen VARCHAR(255);

2. Menampilkan Gaji Tertinggi di Departemen d006

   SELECT e.first_name, e.last_name, s.amount  
FROM employee e  
JOIN salary s ON e.emp_no = s.emp_no  
JOIN dept_emp d ON e.emp_no = d.emp_no  
WHERE d.dept_no = 'd006'  
AND s.amount = (  
    SELECT MAX(s2.amount)  
    FROM salary s2  
    JOIN dept_emp d2 ON s2.emp_no = d2.emp_no  
    WHERE d2.dept_no = 'd006'
);

3. Menambahkan Kolom umur pada Tabel employee dan Mengisinya Secara Otomatis

   ALTER TABLE employee ADD COLUMN umur INT;
UPDATE employee SET umur = TIMESTAMPDIFF(YEAR, birth_date, CURDATE());


4. Menambahkan Foreign Key Index

   ALTER TABLE dept_manager ADD CONSTRAINT fk_dept FOREIGN KEY (dept_no) REFERENCES department(dept_no);



**5. Kesimpulan**

- Indexing dapat meningkatkan kecepatan query secara signifikan.

- Penggunaan composite index (first_name, last_name) mampu mempercepat pencarian hingga 4x lebih cepat.

- Menambahkan kolom tambahan (nama_departemen, umur) membantu analisis data lebih lanjut.

- Foreign key index digunakan untuk menjaga referensial integritas antar tabel.



**6.Bukti Dukung**
- Uji Performa Query Sebelum Composite

![image](https://github.com/user-attachments/assets/225628f8-c74c-4333-a8da-62350741df70)


- Uji Performa Query Sesudah Composite

 ![image](https://github.com/user-attachments/assets/553a9d68-b399-4b60-876f-d1d3fa53d210)


  


**7.Referensi**
- https://dev.mysql.com/doc/refman/8.0/en/explain-output.html
