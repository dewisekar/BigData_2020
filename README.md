## Dokumentasi Praktek ETL menggunakan KNIME

* [Business Understanding](#business-understanding)
* [Data Understanding](#data-understanding)
* [Data Preparation](#data-preparation)
* [Modeling](#modeling)
* [Evaluation](#evaluation)
* [Deployment](#deployment)

## Business Understanding
Kemungkinan proses yang dapat dilakukan pada data ini:
1. Dari data tersebut, dapat didapatkan platform game yang paling banyak memiliki game dengan penjualan lebih dari 100.000 copy
2. Dari data tersebut, dapat didapatkan perusahaan publisher yang paling banyak memiliki game dengan penjualan lebih dari 100.000 copy
3. Dari data tersebut, dapat didapatkan genre game yang paling banyak memiliki game dengan penjualan lebih dari 100.000 copy

## Data Understanding
* Dataset ini merupakan data penjualan dari game-game berbagai platform dari tahun 1980 hingga 2016
* Dataset ini terdiri dari 11 kolom yaitu:
  1. Rank - Ranking dari penjualan secara keseluruhan
  2. Name - Nama game
  3. Platform - Platform game tersebut dimainkan (Contoh: PS4, PC)
  4. Year - Tahun game tersebut dirilis
  5. Genre - Genre dari game
  6. Publisher - Publisher dari game
  7. NA_Sales - Angka penjualan di Amerika Utara (dalam juta)
  8. EU_Sales - Angka penjualan di Eropa (dalam juta)
  9. JP_Sales - Angka penjualan di Jepang (dalam juta)
  10. Other_Sales - Angka penjualan di wilayah selain ke tiga wilayah di atas pada seluruh dunia (dalam juta)
  11. Global_Sales - Angka penjualan global (dalam juta)
  
## Data Preparation
* Dataset awal
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/dataset-awal.PNG)
Dataset di atas akan disimpan dalam bentuk CSV yang selanjutnya akan dibagi menjadi 2 bagian

* Pemisahan Dataset
Pemisahan dataset pada tugas ini dilakukan dengan KNIME dengan langkah-langkah berikut
  1. Pembacaan file CSV dataset</br>
  Pembacaan file CSV dilakukan dengan menggunakan csv reader</br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/csv-reader.PNG)</br>
  Saat melakukan konfigurasi pembacaan CSV, dilakukan pengaturan sebagai berikut</br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/add-file-csv.PNG)</br>
  Pada pengaturan tersebut, artinya menerangkan bahwa dataset memiliki baris judul, dan kolom "Rank" akan dianggap sebagai RowId pada KNIME sehingga dibutuhkan duplikasi kolom Rank pada tahap selanjutnya.</br>
  Hasil dari pembacaan file CSV dataset awal sebagai berikut:<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/tabel-read.PNG)</br>
  
  2. Duplikasi kolom "Rank"</br>
  Karena saat import CSV kolom "Rank" dianggap sebagai RowId, padahal kolom "Rank" memiliki nilai berbeda dengan RowId, sehingga kolom "Rank" butuh untuk dibuat lagi dengan mengambil nilai dari kolom RowId. Pada bagian ini akan menggunakan fungsi "RowId" seperti gambar di bawah.</br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/row-id.PNG)</br>
  Node tersebut digabungkan pada node CSV reader sebelumnya, dengan konfigurasi seperti berikut:</br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/add-rowid.PNG)</br>
  Arti dari pengaturan di atas adalah, akan dibuat sebuah kolom baru dengan nama "Rank" dengan mengambil nilai dari kolom RowId. Hasil dari penambahan kolom dapat dilihat pada gambar di bawah.</br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/tabel-rowid.PNG)</br>
  
  3. Membagi kolom-kolom tersebut menjadi dua bagian</br>
    Bagian ini dilakukan dengan menggunakan node "Column Splitter" yang dihubungkan dengan node "RowId"</br>
    ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/column-splitter.PNG)</br>
    Lalu, node tersebut akan dilakukan pengaturan sebagai berikut:</br>  
    ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/column-split-config.PNG)</br>
    Bagian kiri adalah kolom-kolom yang nantinya akan disimpan ke dalam file CSV, sedangkan bagian kanan adalah kolom-kolom yang akan disimpan pada database SQL. Preview dari kolom sebelah kiri bisa dilihat pada gambar berikut:<br>
    ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/splitted-upper.PNG)</br>
    Sedangkan, untuk kolom bagian kanan dapat dilihat pada gambar berikut:</br>
    ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/splitted-bottom.PNG)</br>
    
  4. Menyimpan file pada CSV <br>
  Dataset yang sudah dipisah menjadi dua, selanjutnya akan disimpan pada CSV untuk bagian pertama. Penyimpanan akan dilakukan dengan cara menggunakan node "CSV Writer" yang disambungkan pada node "Column Splitter" bagian atas. <br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/csv-writer.PNG)</br>
  Lalu, node tersebut diatur tempat penyimpanan file CSV yang akan dibuat sebagai berikut:<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/csv-writer-config.PNG)</br>
  Sehingga, CSV hasil dari penyimpanan dapat dilihat sebagai berikut:<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/csv-upper.PNG)</br>
  
 5. Menyimpan file pada DB SQL<br>
  Dataset yang sudah dipisah menjadi dua, selanjutnya akan disimpan pada CSV untuk bagian kedua. Penyimpanan akan dilakukan dengan cara menggunakan node "DB Writer" yang disambungkan pada node "Column Splitter" bagian atas dan MySQL Connector.<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/sql-connector.PNG)</br>
  Pengaturan untuk MySQL connector sebagai berikut:<br?
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/sql-connector-config.PNG)</br>
  MySql connector disambungkan kepada database yang sudah disediakan sebelumnya. Selanjutnya, menghubungkan node "DB Writer" dengan node MySql Connector dan Column Splitter bagian bawah/<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/db-writer.PNG)</br>
  Pada node DB Writer, dilakukan pengaturan untuk memilih tabel, dan kolom-kolom yang akan dieksport menjadi tabel SQL sebagai berikut:<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/db-writer-config.PNG)</br>
  Sehingga, tabel SQL hasil penyimpanan dapat dilihat sebagai berikut:<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/bottom.PNG)</br>

Sehingga, setelah melalui proses di atas, maka dataset awal telah berhasil dibagi menjadi dua dan disimpan di tempat yang berbeda<br>  
    
## Modeling
## Evaluation
## Deployment
