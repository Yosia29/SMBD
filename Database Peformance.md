**1.Latar Belakang**

Dalam pengelolaan database yang memiliki jumlah data besar, optimasi query menjadi aspek penting untuk meningkatkan performa. Salah satu teknik yang sering digunakan adalah indexing, yang memungkinkan query dapat dieksekusi lebih cepat. Pada tugas ini, dilakukan beberapa teknik optimasi pada database employee, seperti:

- Menambahkan index pada kolom pencarian.

- Menggunakan composite index untuk meningkatkan efisiensi query.

- Melakukan analisis performa query sebelum dan sesudah indexing.

**2. Problem yang Diangkat**

Bagaimana cara meningkatkan performa query dalam database employee dengan menerapkan indexing pada kolom yang sering digunakan dalam pencarian?

**3.Solusi / Skenario Aktivitas**

Solusi yang diimplementasikan meliputi:

- Mengimport database employee.sql dan melakukan query dasar.

- Menambahkan indeks pada kolom yang sering digunakan dalam pencarian.

- Melakukan uji performa query sebelum dan sesudah indexing menggunakan EXPLAIN.

- Mengevaluasi dampak indexing terhadap kecepatan query.



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
- Pengujian dengan menjalankan query yang where clause

  ![image](https://github.com/user-attachments/assets/8d089cf6-57bf-4390-94fc-9016ead9b6c4)

  ![image](https://github.com/user-attachments/assets/15134b99-dfe9-4bf9-bb85-67ff95a456f4)


  


**7.Referensi**
- https://dev.mysql.com/doc/refman/8.0/en/explain-output.html
