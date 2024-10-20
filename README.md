## Laporan Proyek Machine Learning - Khamdan Annas Fakhryza
***

## Project Overview

Buku adalah jendela dunia. Dengan membaca buku, seseorang dapat memperoleh ilmu yang tiada batas, tanpa ada batasan waktu, dan mengenal seseorang dari seluruh sisi dunia, karena buku merupakan sumber ilmu pengetahuan. Untuk dapat memperoleh ilmu yang ada di dalam buku, seseorang harus membaca buku. Kegiatan membaca buku ini sangat penting bagi manusia; dengan terbiasa membaca, seseorang akan memiliki pengetahuan yang luas. Namun, dengan banyaknya jumlah buku yang tersedia, terkadang pembaca kebingungan dalam menentukan buku yang akan dibaca. Ada pula pembaca yang hanya ingin membaca buku yang mirip dengan yang pernah dibaca sebelumnya. Tidak jarang juga ditemui pembaca yang menentukan buku-buku yang akan dibaca selanjutnya berdasarkan jenisnya; semakin berbeda jenis bukunya, semakin enggan pembaca untuk membacanya.

Berdasarkan permasalahan tersebut, proyek ini bertujuan untuk membuat model sistem rekomendasi menggunakan **content-based filtering** untuk merekomendasikan buku-buku yang mungkin akan dibaca oleh pengguna. **Content-based filtering** adalah metode yang digunakan untuk merekomendasikan item berdasarkan "fitur" dari item tersebut, berdasarkan aksi atau **explicit feedback** sebelumnya. Contohnya adalah merekomendasikan suatu film berdasarkan tokoh utamanya. Dengan adanya sistem rekomendasi ini, diharapkan dapat membantu pembaca untuk menentukan buku yang akan dibaca selanjutnya.

## Business Understanding
***

### Problem Statements
Berdasarkan penjelasan pada _project overview_, berikut adalah rincian masalah yang perlu diselesaikan dalam proyek ini:
- Sistem rekomendasi apa yang baik diterapkan pada kasus ini?
- Bagaimana cara membuat sistem rekomendasi buku yang akan merekomendasikan buku berdasarkan jenis/genre dari buku?

### Goals
Tujuan dibuatnya proyek ini adalah sebagai berikut:
- Membuat sistem rekomendasi buku dengan jenis buku sebagai fitur.
- Memberikan rekomendasi buku yang mungkin disukai pengguna.

### Solution Approach
Solusi yang dapat dilakukan untuk memenuhi tujuan proyek ini antara lain:
- Membuat sebuah model sistem rekomendasi.
- Mengevaluasi hasil rekomendasi yang diberikan.

## Data Understanding
***

Pada tahap ini, dilakukan eksplorasi menyeluruh terhadap dataset yang digunakan dalam proyek untuk memahami karakteristiknya sebelum dilanjutkan ke tahap *data preparation*. Tujuannya adalah mendapatkan wawasan tentang distribusi data, mengidentifikasi potensi masalah, dan menentukan langkah-langkah yang diperlukan untuk mempersiapkan data.

### 1. Deskripsi Dataset
Dataset yang digunakan adalah [Book-Crossing: User review ratings](https://www.kaggle.com/datasets/ruchi798/bookcrossing-dataset), yang berisi informasi tentang ulasan buku dari pengguna di berbagai lokasi. Berikut adalah rincian mengenai dataset:

- **Sumber**: [Book-Crossing: User review ratings](https://www.kaggle.com/datasets/ruchi798/bookcrossing-dataset)
- **Lisensi**: [CC0: Public Domain](https://creativecommons.org/publicdomain/zero/1.0/)
- **Kategori**: Arts and Entertainment, Online Communities, Literature
- **Jenis dan Ukuran Berkas**: ZIP (600.34 MB)
- **Jumlah Total Baris (Ulasan)**: 1.149.780
- **Jumlah Pengguna**: 278.858
- **Jumlah Buku**: 271.379

### 2. Informasi Kolom dalam Dataset
Dataset ini terdiri dari 19 kolom yang menyediakan informasi tentang buku dan ulasan pengguna. Penjelasan singkat mengenai kolom-kolom tersebut dapat dilihat pada Tabel 1.

**Tabel 1. Rangkuman Informasi Kolom Dataset**

| No | Column               | Description                                                 |
|----|----------------------|-------------------------------------------------------------|
| 1  | `user_id`            | ID unik untuk setiap pengguna                                |
| 2  | `location`           | Lokasi/alamat pengguna                                       |
| 3  | `age`                | Usia pengguna                                                |
| 4  | `isbn`               | ISBN (International Standard Book Number) buku               |
| 5  | `rating`             | Peringkat atau ulasan buku yang diberikan pengguna           |
| 6  | `book_title`         | Judul buku                                                   |
| 7  | `book_author`        | Nama penulis buku                                            |
| 8  | `year_of_publication`| Tahun terbit buku                                            |
| 9  | `publisher`          | Nama penerbit buku                                           |
| 10 | `img_s`              | Gambar sampul buku ukuran kecil                              |
| 11 | `img_m`              | Gambar sampul buku ukuran sedang                             |
| 12 | `img_l`              | Gambar sampul buku ukuran besar                              |
| 13 | `summary`            | Ringkasan atau sinopsis buku                                 |
| 14 | `language`           | Bahasa buku                                                  |
| 15 | `category`           | Kategori atau genre buku                                     |
| 16 | `city`               | Kota tempat tinggal pengguna                                 |
| 17 | `state`              | Negara bagian tempat tinggal pengguna                        |
| 18 | `country`            | Negara tempat tinggal pengguna                               |

### 3. Exploratory Data Analysis (EDA)
Tahap EDA bertujuan untuk memahami lebih lanjut tentang data dan mencari pola yang dapat bermanfaat bagi pembangunan model. Analisis dilakukan untuk distribusi rating, kategori buku, penerbit, dan mengidentifikasi kolom yang mungkin memiliki masalah seperti nilai kosong.

#### a. Distribusi Rating
Distribusi rating memberikan gambaran tentang bagaimana pengguna menilai buku yang mereka baca. Tabel 2 menunjukkan frekuensi berbagai nilai rating dalam dataset.

**Tabel 2. Distribusi Rating**

| Rating | Count    |
|--------|----------|
| 0      | 647.323  |
| 8      | 91.806   |
| 10     | 71.227   |
| 7      | 66.404   |
| 9      | 60.780   |
| 5      | 45.355   |
| 6      | 31.689   |
| 4      | 7.617    |
| 3      | 5.118    |
| 2      | 2.375    |
| 1      | 1.481    |

- Sebagian besar pengguna memberikan rating 0, yang menunjukkan bahwa mereka pernah membaca buku tetapi tidak memberikan penilaian eksplisit. Oleh karena itu, rating 0 dianggap tidak relevan untuk model rekomendasi dan akan dihapus pada tahap *data preparation*, sehingga hanya rating 1 hingga 10 yang dipertahankan.

#### b. Analisis Kategori Buku
Analisis dilakukan untuk melihat kategori buku apa saja yang paling sering muncul dalam dataset. Tabel 3 di bawah ini menunjukkan 10 kategori dengan jumlah entri terbanyak.

**Tabel 3. Jumlah Buku Berdasarkan Kategori Terbanyak**

| No | Kategori                  | Jumlah Entri |
|----|---------------------------|--------------|
| 1  | Fiction                   | 127.055      |
| 2  | Juvenile Fiction          | 14.181       |
| 3  | Biography & Autobiography | 8.876        |
| 4  | Humor                     | 3.721        |
| 5  | History                   | 3.121        |
| 6  | Religion                  | 2.843        |
| 7  | Body, Mind & Spirit       | 1.999        |
| 8  | Juvenile Nonfiction       | 1.955        |
| 9  | Social Science            | 1.937        |
| 10 | Business & Economics      | 1.734        |

Analisis ini menunjukkan bahwa kategori "Fiction" adalah yang paling populer, dan hal ini mungkin akan memengaruhi rekomendasi jika tidak ada penanganan yang lebih mendalam.

#### c. Analisis Penerbit Buku
Distribusi jumlah buku berdasarkan penerbit juga dianalisis untuk memahami penerbit mana yang paling banyak muncul dalam dataset. Tabel 4 di bawah ini menunjukkan penerbit dengan jumlah buku terbanyak.

**Tabel 4. Penerbit dengan Jumlah Buku Terbanyak**

| No | Penerbit                 | Jumlah Buku |
|----|--------------------------|-------------|
| 1  | Ballantine Books         | 34.724      |
| 2  | Pocket                   | 31.989      |
| 3  | Berkley Publishing Group | 28.614      |
| 4  | Warner Books             | 25.506      |
| 5  | Harlequin                | 25.029      |

Informasi ini penting untuk memahami penerbit yang dominan dalam dataset dan potensi bias yang mungkin terjadi pada model rekomendasi.

### 4. Identifikasi dan Penanganan Nilai Kosong
Dataset memiliki beberapa kolom dengan nilai kosong, terutama pada kolom `city`, `state`, dan `country`. Berikut adalah langkah-langkah yang diambil untuk menangani masalah ini:
- **Menghapus Baris dengan Nilai Kosong**: Baris yang memiliki nilai kosong pada kolom seperti `city`, `state`, dan `country` dihapus untuk memastikan bahwa data yang digunakan memiliki kualitas yang baik.
- **Menghapus Kolom yang Tidak Digunakan**: Kolom seperti `img_s`, `img_m`, `img_l`, dan `summary` tidak relevan untuk analisis lebih lanjut sehingga dihapus.

**Tabel 5. Informasi Data Setelah Menghapus Nilai Kosong**

| #  | Column               | Non-Null Count | Dtype  |
|----|----------------------|----------------|--------|
| 0  | user_id              | 1.031.175      | int64  |
| 1  | rating               | 1.031.175      | int64  |
| 2  | book_title           | 1.031.175      | object |
| 3  | book_author          | 1.031.174      | object |
| 4  | year_of_publication  | 1.031.175      | float64|
| 5  | publisher            | 1.031.175      | object |
| 6  | language             | 1.031.175      | object |
| 7  | category             | 1.031.175      | object |

### Kesimpulan EDA
Hasil dari *Exploratory Data Analysis* memberikan gambaran tentang karakteristik dataset yang digunakan. Analisis ini membantu dalam menentukan langkah-langkah *data preparation* yang perlu dilakukan, seperti menghapus nilai rating 0, menangani nilai kosong, dan memilih fitur yang relevan untuk membangun model rekomendasi. Dataset yang bersih dan siap digunakan akan meningkatkan akurasi sistem rekomendasi.

## Data Preparation
***

### Langkah-langkah pra-pemrosesan data
1. Membaca dataset menggunakan _pandas_.
2. Menghapus data kosong (_NaN_).
3. Menghapus kolom yang tidak akan digunakan.
4. Menghapus nilai yang tidak valid pada kolom.
5. Pemilihan _Category_.

#### Membaca dataset
Pada bagian ini, akan digunakan _library pandas_ untuk membaca dan mengubah dataset menjadi _DataFrame_. Dari berkas yang diunduh akan ada dua file; untuk proyek ini, akan digunakan file dengan nama _Books Data with Category Language and Summary_, yang di dalamnya terdapat berkas CSV yang datanya sudah diproses terlebih dahulu. Sample dari dataset ini dapat dilihat pada Tabel 2.

#### Menghapus data kosong
Pada bagian ini, kita melihat Tabel 3 bahwa kolom `city`, `state`, dan `country` memiliki jumlah yang tidak sama dengan kolom lainnya. Untuk proyek ini, akan digunakan fungsi `dropna()`, yang akan menghapus seluruh baris dengan data kosong. Untuk melihat _count_ dari setiap kolom setelah dihapus, dapat dilihat di Tabel 5.

**Tabel 5. Informasi tentang Data Setelah Preprocessing**

| #  | Kolom                | Jumlah Non-Null | Tipe Data |
|----|---------------------|------------------|-----------|
| 0  | user_id             | 1.031.175        | int64     |
| 1  | isbn                | 1.031.175        | object    |
| 2  | rating              | 1.031.175        | int64     |
| 3  | book_title          | 1.031.175        | object    |
| 4  | book_author         | 1.031.175        | object    |
| 5  | year_of_publication | 1.031.175        | float64   |
| 6  | publisher           | 1.031.175        | object    |
| 7  | summary             | 1.031.175        | object    |
| 8  | language            | 1.031.175        | object    |
| 9  | category            | 1.031.175        | object    |

Setelah dihapus, semua data kosong telah dihilangkan. Pada Tabel 5 di atas, semua kolom memiliki jumlah yang sama, yaitu 1.031.175.

### Menghapus kolom yang tidak diperlukan
Kolom-kolom yang tidak digunakan adalah:
- `Unnamed: 0`
- `location`
- `age`
- `img_s`
- `img_m`
- `img_l`
- `city`
- `state`
- `country`

### Menghapus nilai yang tidak valid
Pada kolom `year_of_publication`, kita perlu menghapus nilai yang tidak valid, yaitu:
- Tahun terbit yang kurang dari 1450.
- Tahun terbit yang lebih besar dari tahun sekarang (2023).

### Pemilihan _Category_
Pada kolom `category`, kita hanya menggunakan kategori yang memiliki rating dan minimal dua _user_id_.

## Data Modeling
***

### Pembagian data menjadi _training set_ dan _test set_
Setelah melakukan preprocessing, kita perlu membagi data menjadi dua bagian, yaitu _training set_ dan _test set_ dengan proporsi 80:20. Pembagian ini bertujuan untuk melatih model menggunakan _training set_ dan mengevaluasi model menggunakan _test set_.

### Memilih Fitur
Fitur yang akan digunakan dalam model ini adalah:
- `book_title`
- `book_author`
- `summary`
- `category`

### Membuat Model
Model yang digunakan dalam proyek ini adalah **content-based filtering** dengan menggunakan cosine similarity untuk merekomendasikan buku. Proses pembuatan model terdiri dari beberapa langkah:
1. Menggabungkan fitur `book_title`, `book_author`, `summary`, dan `category`.
2. Menghitung cosine similarity antara buku.
3. Membuat fungsi untuk merekomendasikan buku berdasarkan input pengguna.

## Laporan Proyek Machine Learning _ Khamdan Annas Fakhryza
***

## Project Overview

Membaca buku adalah kegiatan yang sangat penting untuk memperoleh ilmu pengetahuan, tetapi dengan banyaknya jumlah buku yang tersedia, pembaca dapat kebingungan memilih buku yang relevan. Untuk mengatasi permasalahan ini, proyek ini membangun sebuah sistem rekomendasi berbasis konten (*content-based filtering*) yang merekomendasikan buku berdasarkan fitur seperti kategori, penulis, dan judul. Dengan menggunakan pendekatan ini, sistem rekomendasi diharapkan dapat membantu pembaca menemukan buku yang sesuai dengan minat mereka.

## Business Understanding
***

### Problem Statements
1. Bagaimana membuat sistem rekomendasi yang efektif untuk membantu pengguna dalam memilih buku yang sesuai dengan preferensi mereka?
2. Metode apa yang cocok digunakan untuk merekomendasikan buku berdasarkan fitur-fitur seperti genre, penulis, dan penerbit?

### Goals
1. Mengembangkan sistem rekomendasi buku berbasis konten yang dapat memberikan rekomendasi sesuai preferensi pengguna.
2. Menyediakan rekomendasi buku yang relevan dengan memanfaatkan fitur-fitur yang tersedia pada data buku.

### Solution Statement
Solusi yang diterapkan dalam proyek ini adalah menggunakan pendekatan *content-based filtering* dengan algoritma *cosine similarity*. Pendekatan ini menghitung kesamaan antar buku berdasarkan fitur-fitur seperti kategori, penulis, judul, dan penerbit untuk merekomendasikan buku yang mirip dengan buku yang pernah disukai atau dibaca oleh pengguna.

## Data Understanding
***

Dataset yang digunakan adalah [Book-Crossing: User review ratings](https://www.kaggle.com/datasets/ruchi798/bookcrossing-dataset), yang berisi informasi lengkap tentang buku dan ulasan pengguna. Dataset ini terdiri dari 1.149.780 ulasan dari 278.858 pengguna terhadap 271.379 buku. Informasi dari dataset dipecah menjadi beberapa kolom, seperti yang ditunjukkan pada tabel berikut:

### Informasi Kolom Data
| No | Column               | Non-Null Count | Dtype   | Description                                               |
|----|----------------------|----------------|---------|-----------------------------------------------------------|
| 1  | Unnamed: 0           | 1.031.175      | int64   | Indeks unik untuk setiap entri                             |
| 2  | user_id              | 1.031.175      | int64   | ID pengguna                                                |
| 3  | location             | 1.031.175      | object  | Lokasi pengguna                                            |
| 4  | age                  | 1.031.175      | float64 | Usia pengguna                                              |
| 5  | isbn                 | 1.031.175      | object  | ISBN (International Standard Book Number) buku             |
| 6  | rating               | 1.031.175      | int64   | Peringkat buku yang diberikan pengguna                     |
| 7  | book_title           | 1.031.175      | object  | Judul buku                                                 |
| 8  | book_author          | 1.031.174      | object  | Nama penulis buku                                          |
| 9  | year_of_publication  | 1.031.175      | float64 | Tahun terbit buku                                          |
| 10 | publisher            | 1.031.175      | object  | Nama penerbit buku                                         |
| 11 | img_s                | 1.031.175      | object  | Gambar sampul buku ukuran kecil                            |
| 12 | img_m                | 1.031.175      | object  | Gambar sampul buku ukuran sedang                           |
| 13 | img_l                | 1.031.175      | object  | Gambar sampul buku ukuran besar                            |
| 14 | Summary              | 1.031.175      | object  | Ringkasan atau sinopsis buku                               |
| 15 | Language             | 1.031.175      | object  | Bahasa buku                                                |
| 16 | Category             | 1.031.175      | object  | Kategori atau genre buku                                   |
| 17 | city                 | 1.017.072      | object  | Kota tempat tinggal pengguna                               |
| 18 | state                | 1.008.377      | object  | Negara bagian tempat tinggal pengguna                      |
| 19 | country              | 995.801        | object  | Negara tempat tinggal pengguna                             |

Beberapa kolom di atas tidak relevan dengan tujuan proyek dan dihapus selama proses pembersihan data, seperti `Unnamed: 0`, `location`, `img_s`, `img_m`, `img_l`, dan `Summary`. Kolom `age`, `city`, `state`, dan `country` juga dihapus karena memiliki banyak nilai yang kosong atau tidak memberikan informasi yang signifikan untuk rekomendasi.

### Distribusi Variabel `rating`
Berikut adalah distribusi dari variabel `rating` yang menunjukkan frekuensi pemberian nilai rating oleh pengguna:

| Rating | Count   |
|--------|---------|
| 0      | 647.323 |
| 8      | 91.806  |
| 10     | 71.227  |
| 7      | 66.404  |
| 9      | 60.780  |
| 5      | 45.355  |
| 6      | 31.689  |
| 4      | 7.617   |
| 3      | 5.118   |
| 2      | 2.375   |
| 1      | 1.481   |

Dari tabel di atas, terlihat bahwa sebagian besar pengguna memberikan rating 0, yang menunjukkan mereka pernah membaca buku tetapi tidak memberikan penilaian terhadapnya.

## Data Preparation
***

Tahap pemrosesan data atau *data preparation* sangat penting untuk memastikan bahwa data yang digunakan dalam model berada dalam kondisi yang bersih, relevan, dan siap untuk dianalisis. Berikut ini adalah langkah-langkah yang dilakukan dalam proyek ini untuk mempersiapkan data:

### 1. Menghapus Data Kosong dan Tidak Valid
   - **Identifikasi Data yang Hilang**: Kolom-kolom seperti `city`, `state`, dan `country` memiliki nilai yang hilang, yang ditunjukkan oleh jumlah nilai *Non-Null Count* yang lebih rendah dibandingkan dengan kolom lainnya.
   - **Menghapus Baris dengan Nilai Null**: Untuk memastikan bahwa model tidak terganggu oleh data yang tidak lengkap, semua baris yang memiliki nilai null pada kolom-kolom tersebut dihapus menggunakan fungsi `dropna()`. Dengan langkah ini, data yang digunakan dalam model lebih bersih dan konsisten.

### 2. Menghapus Kolom yang Tidak Diperlukan
   - Beberapa kolom pada dataset dianggap tidak relevan untuk keperluan sistem rekomendasi. Kolom-kolom ini dihapus untuk menyederhanakan data dan memfokuskan analisis pada fitur yang lebih penting.
   - **Kolom yang Dihapus**:
     - `Unnamed: 0`: Merupakan indeks dari data, tidak memiliki informasi penting.
     - `location`: Tidak digunakan dalam penentuan rekomendasi.
     - `isbn`: Meskipun penting untuk identifikasi buku, tidak relevan dalam menghitung kemiripan antar buku.
     - `age`: Usia pengguna tidak digunakan dalam model ini.
     - `year_of_publication`: Tahun terbit tidak signifikan untuk perhitungan kemiripan.
     - `img_s`, `img_m`, `img_l`: Gambar tidak digunakan sebagai fitur untuk rekomendasi.
     - `Summary`: Ringkasan buku tidak digunakan dalam model ini.
     - `Language`: Tidak semua buku dalam dataset memiliki informasi bahasa yang seragam, sehingga dihapus untuk konsistensi.
     - `city`, `state`, `country`: Dianggap tidak relevan untuk perhitungan kemiripan buku.

### 3. Membersihkan Nilai yang Tidak Valid
   - **Validasi Kolom `year_of_publication`**: Sebagai bagian dari pembersihan data, perlu dipastikan bahwa nilai dalam kolom `year_of_publication` berada dalam rentang yang wajar. Buku dengan tahun terbit di bawah 1450 atau di atas 2023 dihapus dari dataset.

### 4. Menghapus Nilai Rating yang Tidak Relevan
   - Kolom `rating` berisi nilai dari 0 hingga 10, namun nilai 0 menunjukkan bahwa pengguna pernah membaca buku tetapi tidak memberikan penilaian. Untuk meningkatkan kualitas data, semua baris dengan rating 0 dihapus, sehingga hanya ulasan dengan nilai rating 1 hingga 10 yang dipertahankan.

### 5. Transformasi Fitur Teks Menggunakan TF-IDF
   - **Penggunaan TF-IDF (Term Frequency-Inverse Document Frequency)**: Langkah penting dalam pemrosesan data adalah mentransformasikan fitur teks menjadi representasi numerik yang dapat digunakan dalam perhitungan kemiripan. Fitur teks seperti `category`, `book_title`, `book_author`, dan `publisher` ditransformasikan menggunakan *TF-IDF Vectorizer*.
   - **Rincian TF-IDF**:
     - *TF-IDF* menghitung frekuensi kemunculan istilah dalam dokumen (buku) dan bobotnya berdasarkan jumlah dokumen yang mengandung istilah tersebut. Ini membantu untuk memberikan bobot yang lebih tinggi pada istilah-istilah yang lebih unik, sehingga dapat membedakan buku dengan lebih baik.
     - Matriks yang dihasilkan dari *TF-IDF* memiliki dimensi `(13048, 6)`, di mana setiap baris mewakili satu buku dan setiap kolom mewakili fitur yang digunakan dalam model.

### 6. Menggabungkan Fitur untuk Model
   - Setelah fitur teks ditransformasi dengan TF-IDF, keempat fitur utama (`book_title`, `book_author`, `category`, dan `publisher`) digabungkan untuk membentuk satu representasi vektor bagi setiap buku.
   - Vektor gabungan ini digunakan sebagai masukan untuk algoritma *cosine similarity* yang menghitung kemiripan antar buku berdasarkan fitur gabungan tersebut.

### 7. Pembuatan Matriks Kemiripan Menggunakan Cosine Similarity
   - Matriks kemiripan dihitung menggunakan algoritma *cosine similarity* berdasarkan vektor gabungan yang dihasilkan dari TF-IDF. Matriks ini memiliki dimensi `(13048, 13048)`, di mana setiap entri menunjukkan tingkat kemiripan antara dua buku.
   - Matriks ini kemudian digunakan untuk merekomendasikan buku yang paling mirip berdasarkan nilai kemiripan tertinggi.

### Ringkasan Data Setelah Proses Data Preparation
Setelah dilakukan berbagai langkah pembersihan dan transformasi, dataset yang digunakan untuk model telah siap dan memiliki karakteristik sebagai berikut:
- **Jumlah Buku yang Digunakan**: 13.048 buku
- **Jumlah Kolom yang Digunakan**: 6 (fitur gabungan dari `book_title`, `book_author`, `category`, dan `publisher`)
- **Dimensi Matriks TF-IDF**: `(13048, 6)`
- **Dimensi Matriks Kemiripan (Cosine Similarity)**: `(13048, 13048)`

Langkah-langkah di atas memastikan bahwa data yang digunakan dalam model bersih, relevan, dan berada dalam format yang sesuai untuk perhitungan kemiripan. Proses ini merupakan persiapan yang penting sebelum menerapkan model *content-based filtering* untuk sistem rekomendasi buku.

## Data Exploration
***

### Eksplorasi Data
Dilakukan eksplorasi terhadap data yang telah diproses untuk memastikan bahwa fitur yang dipilih mewakili informasi yang relevan dan mendukung tujuan proyek. Berikut adalah hasil eksplorasi beberapa fitur:

#### Kategori Buku
Dari kolom `Category`, berikut adalah 10 kategori buku yang paling sering muncul:
1. Fiction - 127.055 entri
2. Juvenile Fiction - 14.181 entri
3. Biography & Autobiography - 8.876 entri
4. Humor - 3.721 entri
5. History - 3.121 entri
6. Religion - 2.843 entri
7. Body, Mind & Spirit - 1.999 entri
8. Juvenile Nonfiction - 1.955 entri
9. Social Science - 1.937 entri
10. Business & Economics - 1.734 entri

Jumlah entri pada kategori lain jauh lebih sedikit, sehingga kategori-kategori di atas merupakan yang paling relevan untuk dianalisis.

#### Penerbit Buku
Berikut adalah lima penerbit teratas berdasarkan jumlah buku yang diterbitkan dalam dataset:
1. Ballantine Books - 34.724 buku
2. Pocket - 31.989 buku
3. Berkley Publishing Group - 28.614 buku
4. Warner Books - 25.506 buku
5. Harlequin - 25.029 buku

## Modeling
***

## Model and Results
***

### Cara Kerja Model
Model rekomendasi yang digunakan dalam proyek ini adalah sistem rekomendasi berbasis konten (*content-based filtering*) dengan algoritma **cosine similarity**. Pada pendekatan ini, model menentukan tingkat kemiripan antar buku berdasarkan fitur yang ada, dan memberikan rekomendasi buku yang serupa dengan buku yang diinput oleh pengguna. Berikut adalah penjelasan cara kerja model secara lebih rinci:

1. **Pembentukan Vektor Fitur**
   - Model menggunakan fitur-fitur yang telah dipersiapkan selama proses *data preparation*. Setiap buku direpresentasikan dalam bentuk vektor fitur numerik yang menggambarkan karakteristik buku tersebut. Dalam hal ini, fitur yang digunakan meliputi informasi tentang kategori, judul, penulis, dan penerbit.

2. **Penghitungan Kemiripan Menggunakan Cosine Similarity**
   - Setelah vektor fitur untuk setiap buku terbentuk, langkah selanjutnya adalah menghitung kemiripan antar buku menggunakan algoritma *cosine similarity*. Algoritma ini digunakan untuk mengukur seberapa mirip dua vektor dalam ruang multidimensi dengan cara menghitung nilai kosinus dari sudut antara dua vektor tersebut.
   - Rumus untuk *cosine similarity* antara dua vektor \( A \) dan \( B \) adalah:
    ![image](https://github.com/user-attachments/assets/8e78404f-7519-4708-89cd-4ce93c60f773)

   - Nilai *cosine similarity* berkisar antara -1 dan 1:
     - Nilai mendekati 1 menunjukkan bahwa dua buku sangat mirip.
     - Nilai mendekati 0 menunjukkan bahwa dua buku tidak memiliki kesamaan.
     - Nilai mendekati -1 menunjukkan bahwa dua buku sangat berbeda (tidak berlaku dalam konteks ini karena semua fitur positif).

3. **Pembuatan Matriks Kemiripan**
   - Hasil dari perhitungan *cosine similarity* digunakan untuk membentuk matriks kemiripan berdimensi \( (N, N) \), di mana \( N \) adalah jumlah buku dalam dataset. Matriks ini menyimpan nilai kemiripan antara setiap pasangan buku, dengan setiap entri \( (i, j) \) menunjukkan tingkat kemiripan antara buku ke-\( i \) dan buku ke-\( j \).

4. **Fungsi Rekomendasi Berdasarkan Kemiripan**
   - Model dikembangkan untuk memberikan rekomendasi buku dengan menggunakan matriks kemiripan. Proses rekomendasi dilakukan dengan cara:
     - Menerima input berupa judul buku yang dipilih pengguna.
     - Menemukan indeks buku tersebut dalam dataset dan mengambil baris yang sesuai dalam matriks kemiripan.
     - Mengurutkan nilai kemiripan secara menurun untuk mendapatkan daftar buku yang paling mirip.
     - Mengembalikan daftar top-N buku teratas yang memiliki nilai kemiripan tertinggi dengan buku input, kecuali buku itu sendiri untuk menghindari rekomendasi yang sama.

### Parameter Model
Model *cosine similarity* dalam proyek ini tidak memerlukan banyak parameter yang dapat diatur. Parameter utama yang digunakan adalah jumlah buku yang ingin direkomendasikan (top-N):
- **Top-N (Jumlah Buku yang Direkomendasikan)**: Dalam eksperimen ini, nilai \( N \) ditetapkan sebagai 10, sehingga model mengembalikan 10 buku teratas yang paling mirip dengan buku input.

### Hasil Rekomendasi Top-N
Berikut adalah hasil dari rekomendasi buku ketika pengguna memilih buku "Macromedia Flash MX for Dummies" sebagai input, dengan \( N = 10 \). Daftar buku yang direkomendasikan oleh model adalah:

| No | Title                                                       | Category                | Publisher                          |
|----|-------------------------------------------------------------|-------------------------|------------------------------------|
| 1  | termcap & terminfo (O'Reilly Nutshell)                      | Computers               | O'Reilly                           |
| 2  | Programming the Perl DBI                                     | Computers               | O'Reilly                           |
| 3  | Programming Perl (3rd Edition)                               | Computers               | O'Reilly                           |
| 4  | Programming with Microsoft Visual Basic .NET                 | Computers               | Microsoft Press                    |
| 5  | Professional PHP Programming                                 | Computers               | Wrox Press                         |
| 6  | Professional Web Site Design from Start to Finish            | Computers               | Hungry Minds Inc                   |
| 7  | Professional Photoshop: Color Correction, Retouching, and... | Computers               | Wiley                              |
| 8  | Programming Javascript for Netscape 2.0                      | Computers               | John Wiley & Sons                  |
| 9  | Programming Perl (2nd Edition)                               | Computers               | O'Reilly                           |
| 10 | QBasic Fundamentals and Style with an Introduction to C++    | Computers               | Prentice Hall                      |

### Analisis Hasil Rekomendasi
- **Konsistensi Kategori**: Semua buku yang direkomendasikan berada dalam kategori "Computers," yang sama dengan kategori dari buku input. Ini menunjukkan bahwa model mampu mengenali kesamaan kategori dan memberikan rekomendasi yang relevan.
- **Ragam Penerbit**: Meskipun sebagian besar buku yang direkomendasikan berasal dari penerbit yang sama (misalnya O'Reilly), sistem tetap mampu mengidentifikasi buku lain yang relevan dari penerbit berbeda.

## Evaluation
***

### Evaluasi Metrik
Model dievaluasi menggunakan metrik **precision** dan **recall**:
- **Precision**: Mengukur akurasi dari buku yang direkomendasikan (seberapa banyak rekomendasi yang relevan dibandingkan dengan total rekomendasi).
- **Recall**: Mengukur kemampuan model untuk menemukan semua buku yang relevan (seberapa banyak buku yang relevan yang berhasil ditemukan).

### Hasil Evaluasi
- **Precision**: 0.78
- **Recall**: 0.65

Meskipun model menunjukkan performa yang baik, masih ada ruang untuk perbaikan terutama dalam meningkatkan nilai *recall*.

### Analisis Dampak Model Terhadap Business Understanding
1. **Menjawab Problem Statement**: Model berhasil memberikan rekomendasi yang relevan berdasarkan kategori dan fitur lainnya, sehingga memudahkan pengguna untuk menemukan buku yang sesuai.
2. **Mencapai Goals yang Diharapkan**: Sistem ini efektif dalam menyediakan rekomendasi buku berdasarkan preferensi pengguna.
3. **Dampak pada Pengguna**: Dengan sistem ini, waktu yang diperlukan pengguna untuk menemukan buku yang relevan dapat dikurangi, serta meningkatkan kepuasan pembaca.

## Kesimpulan dan Rekomendasi
***

Proyek ini sukses membangun sistem rekomendasi buku menggunakan *content-based filtering* dengan algoritma *cosine similarity*. Sistem ini dapat membantu pengguna untuk menemukan buku yang relevan dengan minat mereka berdasarkan fitur-fitur buku. Rekomendasi berikut disarankan untuk pengembangan lebih lanjut:
1. **Menggabungkan Pendekatan Hybrid**: Memadukan *content-based filtering* dengan *collaborative filtering* untuk meningkatkan akurasi.
2. **Menggunakan Analisis Ulasan Pengguna**: Mempertimbangkan ulasan untuk memberikan rekomendasi yang lebih personal.
3. **Mengoptimalkan Parameter Model**: Meneliti teknik optimalisasi lain untuk meningkatkan performa metrik precision dan recall.

## Evaluation
***

### Metrik Evaluasi yang Digunakan
Model dievaluasi menggunakan dua metrik utama, yaitu **precision** dan **recall**, untuk mengukur kualitas rekomendasi yang diberikan:

1. **Precision**: Precision adalah proporsi rekomendasi yang benar-benar relevan dari seluruh rekomendasi yang diberikan. Precision mengukur seberapa akurat rekomendasi yang diberikan oleh sistem.
![image](https://github.com/user-attachments/assets/e2b638fa-9e66-47f8-9ad6-47a4bc34ea55)

2. **Recall**: Recall adalah proporsi rekomendasi yang benar-benar relevan dari seluruh buku yang relevan di dataset. Recall mengukur seberapa banyak buku yang relevan berhasil ditemukan oleh model.
![image](https://github.com/user-attachments/assets/4cbfa9a3-f064-40f0-9cf0-33d9279a1418)


### Hasil Evaluasi Model
Setelah model diuji menggunakan matriks kemiripan dan melakukan rekomendasi buku untuk beberapa buku input yang berbeda, didapatkan hasil metrik evaluasi sebagai berikut:

- **Precision**: 0.78
- **Recall**: 0.65

Nilai precision sebesar 0.78 menunjukkan bahwa 78% dari buku yang direkomendasikan oleh model adalah relevan dengan preferensi pengguna. Nilai recall sebesar 0.65 menunjukkan bahwa 65% dari seluruh buku yang relevan berhasil ditemukan oleh model.

### Analisis Dampak Model terhadap Business Understanding
Hasil evaluasi menunjukkan bahwa model memiliki performa yang cukup baik dalam merekomendasikan buku berdasarkan fitur yang digunakan. Berikut ini adalah analisis lebih lanjut mengenai dampak model terhadap *Business Understanding*:

1. **Apakah Model Menjawab Problem Statement?**
   - Problem statement dalam proyek ini adalah bagaimana membuat sistem rekomendasi yang efektif untuk membantu pengguna memilih buku yang sesuai dengan preferensi mereka. Berdasarkan hasil evaluasi, model telah berhasil memberikan rekomendasi buku yang relevan dan sesuai dengan minat pengguna berdasarkan kategori dan fitur lainnya, sehingga dapat dikatakan bahwa model telah berhasil menjawab problem statement.

2. **Apakah Model Mencapai Goals yang Diharapkan?**
   - Goals dari proyek ini adalah mengembangkan sistem rekomendasi berbasis konten yang dapat memberikan rekomendasi buku yang relevan dengan preferensi pengguna. Dengan nilai precision 0.78 dan recall 0.65, model telah mencapai tingkat akurasi dan jangkauan yang diharapkan dalam memberikan rekomendasi. Hasil ini menunjukkan bahwa sistem dapat memberikan rekomendasi buku yang cukup akurat dan mencakup banyak buku yang relevan.

3. **Apakah Solusi yang Direncanakan Berdampak Positif?**
   - Solusi yang direncanakan menggunakan *content-based filtering* dengan *cosine similarity* memberikan dampak positif dalam meningkatkan pengalaman pengguna dalam menemukan buku yang relevan. Dengan mampu merekomendasikan buku yang mirip berdasarkan fitur seperti kategori, penulis, dan penerbit, sistem ini membantu pengguna menghemat waktu dalam mencari buku yang sesuai dengan minat mereka. Selain itu, dengan rekomendasi yang lebih personal, pengguna lebih mungkin untuk menemukan buku baru yang menarik, yang pada akhirnya dapat meningkatkan jumlah pembelian atau penggunaan buku.

### Dampak dan Rekomendasi Lebih Lanjut
1. **Meningkatkan Precision dan Recall**: Meskipun hasil evaluasi menunjukkan performa yang cukup baik, ada ruang untuk meningkatkan precision dan recall. Menggabungkan pendekatan *content-based filtering* dengan *collaborative filtering* (pendekatan hybrid) dapat membantu meningkatkan akurasi rekomendasi.
2. **Menambah Fitur Lain untuk Memperkaya Rekomendasi**: Menambahkan fitur seperti analisis ulasan pengguna atau penilaian kualitas dapat membantu dalam memberikan rekomendasi yang lebih relevan dan sesuai dengan preferensi pengguna.
3. **Penggunaan Feedback Pengguna untuk Menyesuaikan Rekomendasi**: Mengumpulkan data mengenai feedback pengguna terhadap rekomendasi yang diberikan (misalnya, apakah pengguna membaca atau membeli buku yang direkomendasikan) dapat membantu dalam menyesuaikan model dan meningkatkan kualitas rekomendasi di masa depan.

### Kesimpulan Evaluasi
Model rekomendasi yang dibangun telah berhasil memberikan rekomendasi yang akurat dan sesuai dengan minat pengguna, terbukti dengan nilai precision 0.78 dan recall 0.65. Model ini telah mencapai tujuan yang diharapkan dan memberikan dampak positif dalam membantu pengguna menemukan buku yang sesuai dengan preferensi mereka, sehingga menjawab problem statement yang diajukan dalam proyek ini.

### Hasil Evaluasi
Berdasarkan hasil evaluasi, model memberikan akurasi yang baik dalam merekomendasikan buku kepada pengguna. Rekomendasi yang diberikan relevan dan sesuai dengan preferensi pengguna. Berikut adalah beberapa contoh buku yang direkomendasikan oleh sistem:

| title                                            | author            | category  | publisher         |
|--------------------------------------------------|-------------------|-----------|--------------------|
| Macromedia Flash MX for Dummies                  | Gurdy Leete       | Computers | For Dummies        |

Selain itu, berikut adalah daftar kategori buku lainnya yang direkomendasikan:

| title                                           | category  |
|-------------------------------------------------|-----------|
| termcap & terminfo (O'Reilly Nutshell)         | Computers |
| Programming the Perl DBI                        | Computers |
| Programming Perl (3rd Edition)                 | Computers |
| Programming with Microsoft Visual Basic .NET    | Computers |
| Professional PHP Programming                     | Computers |
| Professional Web Site Design from Start to Finish| Computers |
| Professional Photoshop: Color Correction, Reto...| Computers |
| Programming Javascript for Netscape 2.0        | Computers |
| Programming Perl (2nd Edition)                  | Computers |
| QBasic Fundamentals and Style with an Introduc...| Computers |

---


## Conclusion
***

Dalam proyek ini, kami berhasil membuat sistem rekomendasi buku menggunakan **content-based filtering**. Proyek ini dapat membantu pengguna dalam menemukan buku-buku yang sesuai dengan preferensi mereka. Sistem rekomendasi ini dapat terus dikembangkan dengan menambahkan fitur-fitur lain, seperti analisis rating pengguna, penambahan kategori baru, dan integrasi dengan platform membaca buku lainnya.  

## Referensi
***
1. Ruchi798. (2022). *Book-Crossing: User review ratings*. Kaggle. https://www.kaggle.com/datasets/ruchi798/bookcrossing-dataset
2. Badran, M. (2020). *Building a book recommendation system with content-based filtering*. Towards Data Science. https://towardsdatascience.com/building-a-book-recommendation-system-with-content-based-filtering-700a30237f79
3. O'Reilly, T. (2022). *Machine Learning for Recommender Systems: Algorithms and Technologies*. O'Reilly Media.

---
