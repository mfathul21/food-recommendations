# Laporan Proyek Machine Learning - Muhammad Fathul Radhiansyah

## Project Overview

Dalam industri makanan yang terus berkembang pesat, baik restoran maupun layanan pengiriman makanan, semakin banyaknya variasi menu yang ditawarkan dapat menjadi dilema bagi pelanggan. Keputusan tentang apa yang akan dipesan seringkali memakan waktu dan dapat menjadi pengalaman yang kurang memuaskan jika tidak sesuai dengan preferensi atau keinginan pelanggan. Situasi ini menciptakan kesempatan untuk mengimplementasikan sistem rekomendasi makanan yang dapat memberikan rekomendasi yang dipersonalisasi dan sesuai dengan selera masing-masing pelanggan.

Sistem rekomendasi makanan bertujuan untuk memperbaiki pengalaman pelanggan dengan menyediakan rekomendasi menu yang cocok berdasarkan preferensi makanan sebelumnya, rating yang diberikan, atau bahkan preferensi makanan yang diungkapkan secara eksplisit [1]. Dengan memanfaatkan teknik-teknik seperti analisis data, machine learning, dan pemrosesan bahasa alami, sistem ini dapat menghasilkan rekomendasi yang lebih akurat dan relevan dari waktu ke waktu.

Selain meningkatkan kepuasan pelanggan, implementasi sistem rekomendasi makanan juga dapat membantu pemilik usaha makanan untuk meningkatkan penjualan dengan mengarahkan pelanggan kepada menu-menu yang lebih mungkin mereka sukai. Dengan memanfaatkan teknologi ini, diharapkan dapat menciptakan pengalaman makan yang lebih menyenangkan, meningkatkan loyalitas pelanggan, dan pada akhirnya memperkuat posisi kompetitif dalam industri makanan yang kompetitif ini.

## Business Understanding

### Problem Statements

Berdasarkan kondisi yang telah diuraikan sebelumnya, perusahaan akan mengembangkan sebuah sistem rekomendasi makanan untuk pelanggan secara terpersonalisasi, untuk menjawab permasalahan berikut.

- Berdasarkan data pelanggan, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating yang dimiliki, bagaimana restoran dapat merekomendasikan makanan lain yang mungkin disukai dan belum pernah dipesan oleh pelanggan?

### Goals

Untuk menjawab pertanyaan tersebut, buatlah sistem rekomendasi dengan tujuan atau goals sebagai berikut:

- Menghasilkan sejumlah rekomendasi makanan yang dipersonalisasi untuk pelanggan dengan teknik content-based filtering.
- Menghasilkan sejumlah rekomendasi makanan yang sesuai dengan preferensi pelanggan dan belum pernah dikunjungi sebelumnya dengan menggunakan teknik collaborative filtering, dengan tingkat kesalahan yang rendah (RMSE kurang dari 0.35).

### Solution statements

Dalam proyek ini, tujuan utamanya adalah mengembangkan sebuah sistem rekomendasi makanan yang dipersonalisasi untuk pelanggan. Teknik content-based filtering akan digunakan untuk menghasilkan rekomendasi yang disesuaikan dengan preferensi pelanggan berdasarkan data pelanggan yang ada. Selain itu, data rating yang dimiliki akan digunakan untuk merekomendasikan makanan baru yang mungkin disukai oleh pelanggan, namun belum pernah dipesan sebelumnya. Salah satu fokus utamanya adalah memastikan tingkat kesalahan model rekomendasi (RMSE) kurang dari 0.35, sehingga rekomendasi yang dihasilkan dapat menjadi panduan yang berguna bagi pelanggan dalam memilih makanan yang sesuai dengan preferensinya. Dengan menggabungkan kedua teknik filtering ini, pengalaman rekomendasi yang lebih baik dan terpersonalisasi dapat diberikan kepada pelanggan.

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

| Attribute | Count | Unique | Top                 | Frequency |
|-----------|-------|--------|---------------------|-----------|
| Name      | 400   | 400    | summer squash salad | 1         |
| C_Type    | 400   | 16     | Indian              | 88        |
| Veg_Non   | 400   | 2      | veg                 | 238       |
| Describe  | 400   | 397    | Variety of rice     | 2         |

- Fitur Name: Terdiri dari 400 nilai unik
- Fitur C_Type: Terdiri dari 16 nilai unik (Top: Indian)
- Fitur Veg_Non: Terdiri dari 2 nilai unik (Top: veg)
- Fitur Describe: Terdiri dari 397 nilai unik (Top: riety of rice)

![c_type](https://github.com/mfathul21/food-recomendations/blob/main/assets/c_type.jpg?raw=true)

Gambar 1. Distribution of Cuisine Type

Pada Gambar 1, tipe masakan indian, healthy food, dan dessert merupakan *top* 3 untuk tipe masakan pada data. Selain itu, perhatikanlah terdapat nilai yang *double* yaitu Korean sehingga perlu ditinjau kembali mengapa demikian pada tahap *Data Preparation*.

![veg_non](https://github.com/mfathul21/food-recomendations/blob/main/assets/veg_non.jpg?raw=true)

Gambar 2. Distribution of Vegetarian / Not

Berdasarkan Gambar 2, mayoritas makanan dari data yang digunakan merupakan masakan vegetarian, yaitu makanan yang memenuhi standar vegetarian dengan tidak memasukkan daging dan produk-produk yang berasal dari hewan.

**Ratings Dataset**

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

![ratings](https://github.com/mfathul21/food-recomendations/blob/main/assets/ratings.jpg?raw=true)

Gambar 3. Distribution of Ratings

Berdasarkan Gambar 3, persebaran rating yang diberikan oleh pengguna cukup merata untuk setiap nilai rating dalam rentang 1 hingga 10.

## Data Preparation
**Laporan Data Preparation**

Dalam tahap *Data Preparation*, beberapa teknik dan langkah-langkah dilakukan untuk membersihkan dan mempersiapkan data sebelum digunakan untuk sistem rekomendasi. Berikut adalah langkah-langkah yang dilakukan beserta penjelasannya:

1. **Membersihkan Data pada Kolom 'C_Type'**  
   - Dilakukan penghapusan spasi ekstra dalam nilai kolom 'C_Type' menggunakan fungsi `apply` dan `lambda`.
   - Langkah ini diperlukan untuk memastikan konsistensi data dan menghindari kesalahan dalam analisis berikutnya.

2. **Membersihkan dan Normalisasi Teks pada Fitur 'C_Type', 'Veg_Non', dan 'Describe'**
   - Setiap nilai dalam fitur 'C_Type', 'Veg_Non', dan 'Describe' dibersihkan dari karakter non-alphabet menggunakan regular expression.
   - Selanjutnya, teks pada fitur tersebut dinormalisasi menjadi huruf kecil (lowercase) untuk konsistensi analisis.
   - Langkah ini diperlukan karena data teks sering kali mengandung karakter non-alphabet atau berformat tidak konsisten, sehingga perlu dibersihkan dan dinormalisasi.

3. **Handling Missing Values pada DataFrame 'foods'**
   - Dilakukan pengecekan apakah terdapat nilai kosong (*missing values*) dalam DataFrame 'foods' menggunakan metode `.isna().any()`.
   - Tidak ada nilai kosong yang ditemukan dalam DataFrame 'foods', sehingga tidak perlu dilakukan langkah khusus untuk menangani *missing values*.

4. **Handling Duplicate Data pada DataFrame 'foods'**
   - Pengecekan dilakukan untuk melihat apakah terdapat data ganda (*duplicate data*) dalam DataFrame 'foods' menggunakan metode `.duplicated().any()`.
   - Tidak ada data ganda yang ditemukan dalam DataFrame 'foods', sehingga tidak perlu dilakukan langkah khusus untuk menangani *duplicate data*.

5. **Handling Missing Values pada DataFrame 'ratings'**
   - Dilakukan pengecekan apakah terdapat nilai kosong (*missing values*) dalam DataFrame 'ratings' menggunakan metode `.isna().any()`.
   - Ditemukan nilai kosong pada DataFrame 'ratings', kemudian dilakukan penghapusan baris yang mengandung nilai kosong tersebut menggunakan metode `.dropna()`.
   - Setelah itu, dilakukan pengecekan ulang untuk memastikan tidak ada lagi nilai kosong dalam DataFrame 'ratings'.

6. **Handling Duplicate Data pada DataFrame 'ratings'**
   - Pengecekan dilakukan untuk melihat apakah terdapat data ganda (*duplicate data*) dalam DataFrame 'ratings' menggunakan metode `.duplicated().any()`.
   - Tidak ditemukan data ganda dalam DataFrame 'ratings', sehingga tidak perlu dilakukan langkah khusus untuk menangani *duplicate data*.

7. **Konversi Tipe Data Kolom 'User_ID' dan 'Food_ID' pada DataFrame 'ratings'**
   - Dilakukan konversi tipe data kolom 'User_ID' dan 'Food_ID' pada DataFrame 'ratings' menjadi tipe data *integer* menggunakan metode `.astype(int)`.
   - Langkah ini diperlukan agar data pada kolom 'User_ID' dan 'Food_ID' dapat diproses dengan benar dalam analisis selanjutnya.

Dengan melakukan langkah-langkah di atas, data telah dipersiapkan dengan baik dan siap untuk digunakan dalam tahap *modelling*. Langkah-langkah tersebut diperlukan untuk memastikan kualitas dan konsistensi data sebelum dilakukan pengembangan model.

## Modeling

### Content Based Filtering

*Content-Based Recommendation* memanfaatkan informasi dari beberapa item atau dataset untuk merekomendasikan item yang relevan kepada pengguna berdasarkan informasi yang telah digunakan sebelumnya. Tujuan dari rekomendasi berbasis konten adalah untuk memprediksi kesamaan antara sejumlah informasi yang diberikan oleh pengguna. ***Content-Based Recommendation* memiliki keunggulan dalam personalisasi rekomendasi berdasarkan preferensi pengguna namun memiliki keterbatasan dalam keragaman rekomendasi karena cenderung merekomendasikan item yang mirip dengan yang digunakan pengguna sebelumnya**.

Pada tahap model, menggunakan *CountVectorizer* untuk mengubah data teks menjadi representasi numerik dengan menghitung frekuensi kemunculan kata-kata di setiap dokumen. Bobot kata-kata dihitung berdasarkan frekuensi kemunculannya di setiap dokumen. Selanjutnya, dengan mendefinisikan fungsi untuk sistem rekomendasi makanan dan menggunakan representasi numerik dari *CountVectorizer* serta menghitung kedekatan fitur menggunakan *Cosine Similarity*. Proses ini akan diuji pada salah satu makanan untuk menghasilkan rekomendasi yang sesuai.

| Food_ID | Name                      | C_Type  | Veg_Non | Describe                                  | soup                                               |
|---------|---------------------------|---------|---------|-------------------------------------------|----------------------------------------------------|
| 229     | eggless coffee cupcakes   | dessert | veg     | maida flour baking powder sugar cocoa powd... | dessert veg maida flour baking powder sugar... |

dengan, `food_recommendations('eggless coffee cupcakes')`

| No  | Name                           | C_Type   | Veg_Non |
|-----|--------------------------------|----------|---------|
| 0   | eggless vanilla cake           | dessert  | veg     |
| 1   | chocolate fudge cookies        | dessert  | veg     |
| 2   | microwave chocolate cake       | dessert  | veg     |
| 3   | filter coffee                  | beverage | veg     |
| 4   | double chocolate easter cookies| dessert  | veg     |


### Collaborative Filtering

*Collaborative Filtering* menggunakan transaksi produk atau item sebagai dasar untuk memahami perilaku pengguna dan merekomendasikan item yang disukai berdasarkan kesamaan preferensi dengan pengguna lain. ***Collaborative Filtering*, seperti yang digunakan dalam proyek ini, menawarkan rekomendasi personalisasi tinggi berdasarkan preferensi pengguna sebelumnya, tetapi dapat mengalami kesulitan saat menghadapi pengguna atau item baru dan cenderung merekomendasikan item yang mirip secara berulang**. Dalam proyek ini, akan menggunakan metode *User-Based Collaborative Filtering*. Sebelum membagi data menjadi data training dan testing, data akan diacak. Setelah pembagian data, akan dibangun model RecommendationNet untuk memprediksi skor kesesuaian antara pengguna dan restoran menggunakan teknik *embedding*. Model ini akan digunakan dalam sistem rekomendasi dengan menggunakan *Binary Crossentropy* sebagai *loss*, SGD (*Stochastic Gradient Descent*) sebagai *optimizer*, dan RMSE sebagai *metric evaluation*.

Setelah pelatihan model selesai, model akan digunakan dalam sistem rekomendasi untuk menghasilkan rekomendasi makanan sesuai preferensi pengguna. Proses ini diuji pada pengguna dengan data input yang mencakup preferensi makanan atau item lainnya yang disukai. Dengan menggunakan *Collaborative Filtering*, rekomendasi makanan disesuaikan dengan preferensi pengguna lain yang memiliki riwayat interaksi serupa. Hal ini memungkinkan sistem merekomendasikan makanan dengan kesamaan karakteristik atau preferensi dengan makanan yang disukai sebelumnya oleh pengguna.

**Showing recommendations for users: 67**

**Food with high ratings from user**

| No. | Name                                | C_Type            | Veg_Non                  |
|-----|-------------------------------------|-------------------|--------------------------|
| 1   | Almond and Raw Banana Galawat       | Indian            | Vegetarian               |
| 2   | Meat Lovers Pizza                   | Italian           | Non-Vegetarian           |
| 3   | Thai Prawn Curry & Baked Rice       | Thai              | Vegetarian               |
| 4   | Banana and Chia Tea Cake            | Dessert           | Vegetarian               |
| 5   | Steam Bunny Chicken Bao             | Japanese          | Non-Vegetarian           |

**Top 10 food recommendation**

| No. | Name                                                             | C_Type           | Veg_Non                  |
|-----|------------------------------------------------------------------|------------------|--------------------------|
| 1   | Tandoori Chicken                                                 | Indian           | Non-Vegetarian           |
| 2   | Chettinadu Chicken                                               | Indian           | Non-Vegetarian           |
| 3   | Black Rice                                                       | Healthy Food     | Vegetarian               |
| 4   | Sunga Pork                                                       | Japanese         | Vegetarian               |
| 5   | Miso-Butter Roast Chicken With Acorn Squash Panzanella           | Japanese         | Non-Vegetarian           |
| 6   | Spicy Korean Steak                                               | Korean           | Non-Vegetarian           |
| 7   | Quinoa Bowl and Berries                                          | Healthy Food     | Vegetarian               |
| 8   | Ricotta Gnocchi with Asparagus, Peas, and Morels                 | Italian          | Vegetarian               |
| 9   | Grilled Sweet Prawn                                              | Chinese          | Non-Vegetarian           |
| 10  | Pho Tai Rare Beef                                                | Vietnamese       | Non-Vegetarian           |


## Evaluation

Dalam proyek ini, dengan menggunakan *Root Mean Squared Error* (RMSE) sebagai metrik evaluasi untuk mengevaluasi kinerja model rekomendasi. RMSE adalah metrik yang cocok digunakan untuk masalah prediksi, khususnya saat ingin mengukur seberapa dekat prediksi model dengan nilai sebenarnya. RMSE dihitung dengan mengambil akar kuadrat dari rata-rata dari selisih kuadrat antara nilai prediksi dan nilai sebenarnya. Formulanya adalah sebagai berikut:

$$
\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}
$$

di mana:
- $n$ adalah jumlah sampel data,
- $y_i$ adalah nilai sebenarnya dari sampel ke-i, dan
- $\hat{y}_i$ adalah nilai prediksi dari sampel ke-i.


Dengan menggunakan metrik RMSE, kemampuan model dalam memprediksi nilai rating yang sebenarnya oleh pengguna dapat diukur. Semakin rendah nilai RMSE, semakin baik model dalam memprediksi nilai rating, yang mengindikasikan bahwa rekomendasi yang dihasilkan oleh sistem akan lebih akurat dan sesuai dengan preferensi pengguna. Oleh karena itu, kinerja model dievaluasi berdasarkan nilai RMSE yang dihasilkan pada data uji.

![metric_evaluasi](https://github.com/mfathul21/food-recomendations/blob/main/assets/model_metric.jpg?raw=true)

Gambar 4. Plot of RMSE Training & Validation

Berdasarkan hasil training model dengan learning rate 0.005, ukuran embedding 100, batch size 16, dan epoch 100, diperoleh nilai error sekitar 0.30 pada data training, sedangkan pada data validasi sebesar 0.32. Nilai tersebut dianggap cukup bagus untuk sistem rekomendasi. Dengan nilai RMSE yang rendah, dapat disimpulkan bahwa model rekomendasi mampu memberikan rekomendasi dengan akurasi yang baik, sesuai dengan preferensi pengguna berdasarkan data yang diberikan.

## References

[1] J. Wang, "Personalized Food Recommendation System Based on Machine Learning Techniques," IEEE Transactions on Computational Intelligence and AI in Games, vol. 8, no. 2, pp. 156-169, 2023.
