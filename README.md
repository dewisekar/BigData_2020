## Dokumentasi Praktek ETL menggunakan KNIME

* [Business Understanding](#business-understanding)
* [Data Understanding](#data-understanding)
* [Data Preparation](#data-preparation)
* [Modeling](#modeling)
* [Evaluation](#evaluation)
* [Deployment](#deployment)<br>
Dataset asli: [vgsales.csv](https://github.com/dewisekar/BigData_Tugas1/blob/master/vgsales.csv)<br>

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
  
  4. Resorter Kolom<br>
  Untuk mempermudah pengolahan data, maka kolom "Rank" dipindah ke bagian depan menggunakan node "Column Resorter" yang dihubungkan dengan "Column Splitter" bagian atas.<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/column-splitter.PNG)</br>
  Pada node tersebut, kolom Rank diatur menjadi bagian paling pertama sebagai berikut. <br>
   ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/column-resorter-config.PNG)</br>
   Sehingga, hasil tabel setelah mengalami resorter adalah seperti ini<br>
   ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/column-resorter-hasil.PNG)</br>
   
  5. Menyimpan file pada CSV <br>
  Dataset yang sudah dipisah menjadi dua, selanjutnya akan disimpan pada CSV untuk bagian pertama. Penyimpanan akan dilakukan dengan cara menggunakan node "CSV Writer" yang disambungkan pada node "Column Resorter" bagian atas. <br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/csv-writer.PNG)</br>
  Lalu, node tersebut diatur tempat penyimpanan file CSV yang akan dibuat sebagai berikut:<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/csv-writer-config.PNG)</br>
  Sehingga, CSV hasil dari penyimpanan dapat dilihat sebagai berikut:<br>
  ![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/csv-upper.PNG)</br>
  
 6. Menyimpan file pada DB SQL<br>
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

Sehingga, setelah melalui proses di atas, maka dataset awal telah berhasil dibagi menjadi dua dan disimpan di tempat yang berbeda. Workflow keseluruhan sebagai berikut:<br>  
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/split/full.PNG)</br>

## Modeling
### Pembacaan data dari sumber yang berbeda
**Pembacaan dari file CSV**
1. Pembacaan file CSV dilakukan dengan menggunakan node "CSV Reader" di bawah ini.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/csv-reader.PNG)</br>
Pengaturan node tersebut dilakukan sebagai berikut.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/csv-reader-config.PNG)</br>
Arti dari pengaturan tersebut adalah, mengambil file CSV bernama videogamesales_upper yang berisi kolom-kolom dataset yang telah dipisah menjadi bagian pertama. Lalu, pengaturan diatur bahwa file tersebut memiliki Column dan Row Header. Hal ini menyebabkan kolom Rank pada data akan dianggap sebagai RowId, padahal kolom Rank tidak sama dengan RowId. Sehingga dibutuhkan pembentukan kolom RowId ulang. Hasil dari pembacaan file CSV dapat dilihat pada gambar di bawah.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/csv-reader-hasil.PNG)</br>
2. Penambahan RowId
Penambahan RowId dilakukan dengan cara menggunakan node RowId. Hal ini dimaksudkan untuk membuat RowId baru dan mengembalikan kolom Rank dengan mengambil nilai yang sekarang dianggap sebagai nilai dari kolom RowId. Node ini disambungkan dengan node CSV Reader.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/rowid-2.PNG)</br>
Pengaturan untuk menambahkan RowId dapat dilihat pada gambar berikut.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/rowid-2-config.PNG)</br>
Arti dari pengaturan tersebut adalah akan dibuat kolom RowId baru, serta akan dibuat kolom bernama "Rank" dengan nilai dari kolom RowId saat ini. Hasil dari penambahan RowId dan Rank dapat dilihat pada gambar berikut.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/rowid-2-hasil.PNG)</br>
Setelah melakukan proses di atas, maka data yang dibaca dari file CSV siap untuk dilakukan proses join.<br>

**Pembacaan dari DB MySQL**
1. Melakukan connect dengan MySQL connector<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/mysql-connector.PNG)</br>
MySql connector disambungkan kepada database berisi bagian dataset lainnya yang sudah disediakan sebelumnya. Pengaturaannya adalah sebagai berikut.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/mysql-connector-config.PNG)</br>
2. Melakukan pemilihan tabel
DB yang sudah terkoneksi akan dipilih tabel yang akan dibaca. Pemilihan tabel dilakukan dengan menggunakan node DB Table Selector. Node ini disambungkan dengan node MySQL connector.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/table-selector.PNG)</br>
Tabel yang dipilih adalah table videogamesales_bottom sehingga pengaturannya sebagai berikut.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/table-selector-config.PNG)</br>
Hasil dari pemilihan tabel dapat dilihat pada gambar di bawah ini.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/table-selector-hasil.PNG)</br>
3. Melakukan DB Reader
Tabel yang sudah dipilih harus melalui proses DB Reader agar bisa dilakukan proses join pada langkah selanjutnya. Proses ini menggunakan node DB Reader. Node ini disambungkan pada node DB Table Selector.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/db-reader.PNG)</br>
Pengaturan DB Reader dapat dilihat pada gambar berikut.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/db-reader-config.PNG)</br>
Sehingga, setelah dilakukan DB Reader maka hasil tabelnya sebagai berikut./<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/db-reader-hasil.PNG)</br>

### Melakukan Join Table
Kedua sumber dataset yang sudah berhasil dibaca pada KNIME, selanjutnya akan dijoinkan dengan node Joiner. Node joiner disambungkan dengan node RowId yang terhubung dengan CSV reader dan dengan node DB Table Reader.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/joiner.PNG)</br>
Node joiner diatur dengan memilih kolom mana yang merupakan primary key dan foreign key dari setiap sumber dataset sehingga data dapat dijoinkan. Pada kasus ini, proses join dilakukan berdasarkan kolom RowId.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/joiner-config.PNG)</br>
Sehingga, tabel hasil join merupakan sebagai berikut.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/joiner-hasil.PNG)</br>
Untuk merapikan tabel, maka kolom Rank akan dipindah ke posisi semula yaitu di paling depan dataset dengan menggunakan Colum Resorter.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/resorter.PNG)</br>
Pengaturan column resorter dapat dilihat pada gambar di bawah.
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/resorter-config.PNG)</br>
Sehingga, tabel yang telah di resort adalah sebagai berikut.<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/resorter-hasil.PNG)</br>

## Evaluation
* Tabel yang berhasil melalui proses join akan dibandngkan dengan file dataset asli.
* Tabel hasil join:<br>
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/modelling/resorter-hasil.PNG)</br>
* Tabel asli
![image](https://github.com/dewisekar/BigData_Tugas1/blob/master/images/deployment/csvasli.PNG)</br>
* Dari gambar di atas, dapat disimpulkan bahwa tabel hasil join sama dengan tabel dataset awal.<br>

## Deployment
