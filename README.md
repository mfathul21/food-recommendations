# Laporan Proyek Machine Learning - Muhammad Fathul Radhiansyah

## Project Overview

Dalam industri makanan yang terus berkembang pesat, baik restoran maupun layanan pengiriman makanan, semakin banyaknya variasi menu yang ditawarkan dapat menjadi dilema bagi pelanggan. Keputusan tentang apa yang akan dipesan seringkali memakan waktu dan dapat menjadi pengalaman yang kurang memuaskan jika tidak sesuai dengan preferensi atau keinginan pelanggan. Situasi ini menciptakan kesempatan untuk mengimplementasikan sistem rekomendasi makanan yang dapat memberikan rekomendasi yang dipersonalisasi dan sesuai dengan selera masing-masing pelanggan.

Sistem rekomendasi makanan bertujuan untuk memperbaiki pengalaman pelanggan dengan menyediakan rekomendasi menu yang cocok berdasarkan preferensi makanan sebelumnya, rating yang diberikan, atau bahkan preferensi makanan yang diungkapkan secara eksplisit [1]. Dengan memanfaatkan teknik-teknik seperti analisis data, machine learning, dan pemrosesan bahasa alami, sistem ini dapat menghasilkan rekomendasi yang lebih akurat dan relevan dari waktu ke waktu.

Selain meningkatkan kepuasan pelanggan, implementasi sistem rekomendasi makanan juga dapat membantu pemilik usaha makanan untuk meningkatkan penjualan dengan mengarahkan pelanggan kepada menu-menu yang lebih mungkin mereka sukai. Dengan memanfaatkan teknologi ini, diharapkan dapat menciptakan pengalaman makan yang lebih menyenangkan, meningkatkan loyalitas pelanggan, dan pada akhirnya memperkuat posisi kompetitif dalam industri makanan yang kompetitif ini.

  Format Referensi: [Judul Referensi](https://scholar.google.com/) 

## Business Understanding

### Problem Statements

Berdasarkan kondisi yang telah diuraikan sebelumnya, perusahaan akan mengembangkan sebuah sistem rekomendasi makanan untuk pelanggan secara terpersonalisasi, untuk menjawab permasalahan berikut.

- Berdasarkan data pelanggan, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating yang Anda miliki, bagaimana restoran dapat merekomendasikan makanan lain yang mungkin disukai dan belum pernah dipesan oleh pelanggan?

### Goals

Untuk menjawab pertanyaan tersebut, buatlah sistem rekomendasi dengan tujuan atau goals sebagai berikut:

- Menghasilkan sejumlah rekomendasi makanan yang dipersonalisasi untuk pelanggan dengan teknik content-based filtering.
- Menghasilkan sejumlah rekomendasi makanan yang sesuai dengan preferensi pelanggan dan belum pernah dikunjungi sebelumnya dengan teknik collaborative filtering.

### Solution statements

Untuk mencapai tujuan tersebut, langkah-langkah berikut akan diambil:

1. **Data Understanding:** Data Understanding adalah tahap awal proyek untuk memahami data yang dimiliki. Dalam kasus ini, kita memiliki 2 file terpisah mengenai deskripsi makanan dan rating.
2. **Univariate Exploratory Data Analysis:** Pada tahap ini, dilakukan analisis dan eksplorasi setiap variabel pada data. Jika dibutuhkan, dapat melakukan eksplorasi lebih lanjut mengenai keterkaitan antara satu variabel dengan variabel lainnya.
3. **Data Preparation:** Pada tahap ini, dilakukan proses mempersiapkan data dan melakukan beberapa teknik seperti mengatasi missing value, data duplikat, dan data yang kotor. 
4. **Model Development dengan Content Based Filtering:** Pada tahap ini, akan dikembangkan sistem rekomendasi menggunakan teknik content-based filtering. Teknik ini merekomendasikan item berdasarkan kesamaan dengan item yang disukai pengguna berdasarkan fitur atau deskripsi item tersebut. Representasi fitur frekuensi kemunculan dari setiap kategori atau deskripsi makanan akan dihasilkan menggunakan Count Vectorizer, dan tingkat kesamaan antara makanan dihitung dengan menggunakan cosine similarity. Berdasarkan kesamaan ini, akan dibuat rekomendasi makanan untuk pelanggan.
5. **Model Development dengan Collaborative Filtering:** Pada tahap ini, sistem merekomendasikan sejumlah makanan berdasarkan rating yang telah diberikan sebelumnya. Dari data rating pengguna, kita akan mengidentifikasi makanan-makanan yang mirip dan belum pernah dipesan oleh pengguna untuk direkomendasikan.

## Data Understanding
Data yang digunakan pada proyek kali ini adalah "Food Recomendation System" yang diunduh dari <a href="https://www.kaggle.com/datasets/schemersays/food-recommendation-system">Kaggle API</a>. Dataset ini merepresentasikan data yang berhubungan dengan sistem rekomendasi makanan. Dua dataset disertakan dalam file dataset ini. Pertama, termasuk dataset yang terkait dengan makanan, bahan, masakan yang terlibat. Kedua, termasuk dataset dari sistem rating untuk sistem rekomendasi.

Dataset pertama memiliki 400 baris dengan 5 fitur, yang terdiri fitur non-numerik seperti Name, C_Type, Veg_Non, dan Describe, serta fitur numerik yaitu Food_ID. Sedangkan, untuk dataset kedua memilki 512 baris data dengan 3 fitur numerik, yaitu User_ID, Food_ID, dan Rating.

Variabel-variabel pada kedua datasets tersebut adalah sebagai berikut:
- User_ID: ID unik untuk setiap pengguna atau pelanggan.
- Food_ID: ID unik untuk setiap makanan dalam dataset.
- C_Type: Jenis makanan yang termasuk dalam kategori tertentu, misalnya Chinese, Dessert, Indian, dll.
- Veg_Non: Status vegetarian atau non-vegetarian dari makanan.
- Describe: Deskripsi singkat tentang makanan, termasuk bahan utama, cita rasa, atau karakteristik lainnya.
- Rating: Nilai rating yang diberikan oleh pengguna untuk makanan tertentu, menunjukkan seberapa disukainya makanan tersebut oleh pengguna. Dengan nilai dalam rentang 1 hingga 10, di mana nilai yang lebih tinggi menunjukkan tingkat kepuasan yang lebih tinggi.

### Univariate Exploratory Data Analysis

**Food Dataset**

Statistika deskriptif untuk fitur numerik:

| Statistic | Food_ID |
|-----------|---------|
| count     | 400.000 |
| mean      | 200.500 |
| std       | 115.614 |
| min       | 1.000   |
| 25%       | 100.750 |
| 50%       | 200.500 |
| 75%       | 300.250 |
| max       | 400.000 |

Perhatikan, untuk fitur Food_ID pada foods memiliki range 1-400 dengan jumlah baris data yaitu 400 menunjukkan nilai unik pada fitur tersebut juga 400.

Statistika deskriptif untuk fitur non_numerik:

| Attribute | Count | Unique | Top             | Frequency |
|-----------|-------|--------|-----------------|-----------|
| Name      | 400   | 400    | summer squash salad | 1       |
| C_Type    | 400   | 16     | Indian          | 88        |
| Veg_Non   | 400   | 2      | veg             | 238       |
| Describe  | 400   | 397    | Variety of rice | 2         |

- Fitur Name: Terdiri dari 400 nilai unik
- Fitur C_Type: Terdiri dari 16 nilai unik (Top: Indian)
- Fitur Veg_Non: Terdiri dari 2 nilai unik (Top: veg)
- Fitur Describe: Terdiri dari 397 nilai unik (Top: riety of rice)

![c_type](https://drive.google.com/uc?id=1q_3cHovCGba3v_lP6v4DKQrQqSOOcmtQ)

Tipe masakan indian, healthy food, dan dessert merupakan top 3 untuk tipe masakan pada data. Selain itu, perhatikanlah terdapat nilai yang double yaitu Korean sehingga perlu ditinjau kembali mengapa demikian pada tahap Data Preparation.

![veg_non](https://drive.google.com/uc?id=1FSp3vXibtCVE0h20fSv3f7O2DPZktdgr)

Berdasarkan data, mayoritas makanan dari data yang digunakan merupakan masakan vegetarian, yaitu makanan yang memenuhi standar vegetarian dengan tidak memasukkan daging dan produk-produk yang berasal dari hewan.

**ratings datasets**

| Statistic | User_ID   | Food_ID   | Rating    |
|-----------|-----------|-----------|-----------|
| count     | 511.000   | 511.000   | 511.000   |
| mean      | 49.068    | 125.311   | 5.438     |
| std       | 28.739    | 91.293    | 2.866     |
| min       | 1.000     | 1.000     | 1.000     |
| 25%       | 25.000    | 45.500    | 3.000     |
| 50%       | 49.000    | 111.000   | 5.000     |
| 75%       | 72.000    | 204.000   | 8.000     |
| max       | 100.000   | 309.000   | 10.000    |

Dalam data, terdapat 511 baris data untuk fitur User_ID dan Food_ID, namun rentang nilai untuk masing-masing hanya mencakup 1 hingga 100 dan 1 hingga 309. Hal ini mengindikasikan bahwa beberapa pelanggan memberikan rating lebih dari sekali, begitu pula dengan makanan yang mendapatkan rating lebih dari sekali. Selain itu, fitur Rating menampilkan skala nilai dari 1 (terendah) hingga 10 (tertinggi).

![ratings](https://drive.google.com/uc?id=15QI3Q60L3hQBKJqAlUOkKsPVVJxXgu5e)

Persebaran rating yang diberikan oleh pengguna cukup merata untuk setiap nilai rating dalam rentang 1 hingga 10.

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
