# Laporan Proyek Machine Learning - Bariza Haqi

## Project Overview
Buku adalah sumber informasi dan ilmu pengetahuan. Membaca buku dapat menambah ilmu yang kita miliki. Pada zaman sekarang ini, banyak sekali buku yang tersedia di berbagai website perpustakaan dalam bentuk digital. Tidak sedikit orang yang memiliki hobi membaca buku dan menginginkan buku yang mirip dengan buku favorit mereka. Salah satu cara untuk memberikan rekomendasi buku kepada mereka adalah dengan menggunakan machine learning sistem rekomendasi yang menggunakan metode collaborative filtering dari data rating buku yang diberikan oleh orang yang membacanya. Dengan ini, rekomendasi buku yang diberikan diharapkan sesuai dengan yang mereka inginkan.

## Business Understanding

### Problem Statements

Berdasarkan latar belakang di atas, berikut merupakan rincian masalah dari proyek ini:
- Bagaimana membuat model machine learning yang dapat memberikan sistem rekomendasi buku dengan baik?

### Goals

Tujuan dari proyek ini adalah:
- Membuat model machine learning yang dapat rekomendasi buku sehingga dapat dikembangkan dan digunakan oleh website perpustakaan digital

## Data Understanding
Dataset yang digunakan pada proyek ini adalah [*goodbooks-10k*](https://www.kaggle.com/datasets/zygmunt/goodbooks-10k).
Pada Dataset ini terdapat 5 berkas csv dan hanya ada dua berkas yang akan dipakai yaitu `books.csv` dan `ratings.csv` 

Pada berkas `books.csv` memuat data identitas buku yang terdiri dari 10000 baris dan memiliki 23 kolom, 6 diantaranya adalah :  
- `book_id	` : id buku 
- `title` : judul buku
- `authors` : penulis buku
- `isbn` : kode ISBN buku
- `original_publication_year` : tahun terbit buku  
- `average_rating` : rata-rata rating 

Pada berkas `ratings.csv` memuat data rating buku yang diberikan oleh pengguna. Data ini terdiri dari 981.756 baris dan 3 kolom yaitu :  

 - `book_id	` : id buku
 - `user_id` : id pengguna
 - `rating` : nilai rating yang diberikan oleh pengguna berkisar antara 1-5

Pada variabel books akan dilakukan pengambilan kolom yang penting saja sehingga hanya terdiri dari id buku dan judul buku. Setelah itu akan dilakukan *merge* dengan variabel rating dan kemudian memecahnya sekaligus melakukan *drop* duplikatnya menjadi variabel ratings dan variabel books yang sudah bersih.

Pada variabel *books* setelah dilakukan pembersihan, jumlah datanya menjadi 812 baris dan 2 kolom. Lima sampel teratas dari variabel *books* diperlihatkan pada Tabel 1. 

Tabel 1. Lima sampel teratas pada dataset *books*

| book_id | title                                                    | 
|---------|----------------------------------------------------------|
| 3       | Harry Potter and the Sorcerer's Stone (Harry Potter, #1) | 
| 2657    | To Kill a Mockingbird                                    | 
| 4671    | The Great Gatsby                                         |
| 5907    | The Hobbit                                               | 
| 5107    | The Catcher in the Rye                                   |

Pada variabel *ratings* setelah dilakukan pembersihan, jumlah datanya menjadi 79583 baris dan 3 kolom. Lima sampel teratas dari variabel *ratings* diperlihatkan pada Tabel 2. 

Tabel 2. Lima sampel teratas pada dataset *ratings*

| book_id | user_id |rating| 
|---------|---------|------|
| 3       | 314     | 3    |
| 2657    | 588     | 1    |
| 4671    | 2077    | 2    |
| 5907    | 2487    | 3    |
| 5107    | 2900    | 3    |

Variabel ratings di atas akan menjadi data utama dalam membuat sistem rekomendasi dengan Collaborative Filtering pada proyek ini.

## Data Preparation
- **Encoding** :  Pada tahap ini setiap  data book_id dan user_id akan akan disandikan atau diubah ke dalam indeks integer yang berurutan sampai jumlah data yang ada
- **Randomize Dataset** : Pada tahap ini dataset akan diacak agar distribusinya menjadi random
- **Data Standardization** : Pada tahap ini kolom rating akan dibuat dalam skala 0 dan 1 dimana 0 merupakan minimum dan 1 adalah maksimum agar mudah dalam melakukan proses training
- **Data Splitting** :  Pada tahap ini data dibagi menjadi 80% data train untuk dilatih dan 20% data validasi untuk validasi data

## Modeling
Pada tahap ini akan dilakukan proses *embedding* terhadap data user dan book. Kemudian dilakukan lakukan operasi perkalian *dot product* antara *embedding* user dan resto. Skor kecocokan ditetapkan dalam skala [0,1] dengan fungsi aktivasi *sigmoid*.

Model dibuat keras Model *class* untuk *Collaborative Filtering*. Parameter yang digunakan pada model ini adalah *Binary Crossentropy* untuk *loss function*, Adam dengan *learning_rate* 0.001 untuk optimizernya dan *Root Nean Squared Error* (RMSE) sebagai metrik evaluasinya.

Model dapat menghasil top 10 rekomendasi buku yang diperlihatkan pada Gambar 1

![alt text](/img/output.png) 

Gambar 1. Top rekomendasi buku yang dihasilkan

## Evaluation
Metrik evaluasi yang digunakan adalah *Root Mean Square Error (RMSE) dimana semakin kecil nilai RMSE nya maka semakin dekat nilai yang diprediksi dan diamati.
Plot hasil proses training model diperlihatkan pada Gambar 2

![alt text](/img/plot_model.png) 

Gambar2. Plot hasil training

Pada plot di atas, model yang dihasilkan cukup mulus dan konvergen. Dari proses ini, nilai error akhir yang dihasilkan sekitar 0.17 dan error validasi sebesar 0.23. Hal ini membuktikan bahwa model memiliki performasi bagus untuk sebuah sistem rekomendasi.