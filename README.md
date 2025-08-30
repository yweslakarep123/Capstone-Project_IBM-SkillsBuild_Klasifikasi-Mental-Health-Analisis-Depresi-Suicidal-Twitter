# Klasifikasi Kesehatan Mental & Analisis Penyebab Depresi Berdasarkan Data Twitter

Proyek ini bertujuan untuk mengklasifikasikan tweet ke dalam berbagai kategori kesehatan mental menggunakan model deep learning dan menganalisis penyebab umum di balik sentimen depresi dan pikiran bunuh diri menggunakan Large Language Model (LLM) dari IBM Granite.

## 📜 Daftar Isi
- [Tujuan Proyek](#-tujuan-proyek)
- [Dataset](#-dataset)
- [Arsitektur Model](#-arsitektur-model)
- [Hasil](#-hasil)
- [Instalasi & Penggunaan](#️-instalasi--penggunaan)
- [Struktur Proyek](#struktur-proyek)

## �� Tujuan Proyek

**Klasifikasi Teks**: Membangun model yang mampu mengklasifikasikan teks dari Twitter ke dalam 7 kategori kondisi kesehatan mental yang berbeda (Normal, Depresi, Cemas, Bipolar, Stres, Gangguan Kepribadian, dan Pikiran Bunuh Diri).

**Analisis Penyebab**: Menggunakan model IBM Granite untuk menganalisis dan merangkum faktor-faktor utama yang menyebabkan depresi dan pikiran bunuh diri berdasarkan konten tweet yang telah diklasifikasikan.

## 💾 Dataset

Proyek ini menggunakan dataset "Sentiment Analysis for Mental Health" yang tersedia di Kaggle. Dataset ini berisi kumpulan teks (tweet) yang telah diberi label sesuai dengan kondisi kesehatan mental.

- **Sumber**: [Sentiment Analysis for Mental Health on Kaggle](https://www.kaggle.com/datasets/narendrageek/mental-health-and-suicidal-ideation)
- **Total Data**: 52,681 sampel setelah pembersihan.
- **Kolom**: statement (teks tweet) dan status (label kategori).

Distribusi kategori dalam dataset tidak seimbang, sehingga teknik class weighting diterapkan selama pelatihan model untuk memastikan setiap kategori mendapat perhatian yang proporsional.

## �� Arsitektur Model

### 1. Model Klasifikasi: BertLSTMClassifier

Model ini adalah arsitektur hybrid yang menggabungkan kekuatan dari model Transformer dan LSTM untuk pemahaman konteks yang mendalam.

- **Embedding Layer**: Menggunakan tokenizer dan model embedding dari `ibm-granite/granite-embedding-30m-english`. Lapisan BERT di-freeze pada awalnya untuk memanfaatkan pengetahuan yang sudah ada.

- **Recurrent Layer**: Output dari BERT dimasukkan ke dalam Bi-directional LSTM (BiLSTM) untuk menangkap dependensi sekuensial dari teks.

- **Pooling & Classification**: Hasil dari BiLSTM (hidden state, mean pooling, max pooling) digabungkan dan dimasukkan ke dalam lapisan fully connected (Linear) dengan fungsi aktivasi Softmax untuk menghasilkan probabilitas klasifikasi.

### 2. Model Analisis: IBM Granite

Untuk menganalisis penyebab di balik sentimen, model `ibm-granite/granite-3.3-8b-instruct` digunakan melalui API Replicate.

- Model ini diberikan sampel tweet dari kategori "Depression" dan "Suicidal".
- Prompt dirancang untuk meminta model merangkum alasan utama dan menjelaskan faktor psikologis atau sosial di balik ekspresi tersebut.

## �� Hasil

### Kinerja Model Klasifikasi

Model berhasil mencapai akurasi sebesar **74.2%** pada data uji. Laporan klasifikasi detailnya adalah sebagai berikut:

| Kategori | Precision | Recall | F1-Score | Support |
|----------|-----------|---------|-----------|---------|
| Normal | 0.64 | 0.84 | 0.72 | 576 |
| Depression | 0.80 | 0.73 | 0.76 | 417 |
| Suicidal | 0.73 | 0.65 | 0.69 | 2,311 |
| Anxiety | 0.90 | 0.90 | 0.90 | 2,452 |
| Bipolar | 0.53 | 0.70 | 0.60 | 161 |
| Stress | 0.52 | 0.66 | 0.58 | 388 |
| Personality Disorder | 0.64 | 0.62 | 0.63 | 1,598 |

- **Accuracy**: 0.74
- **Macro Avg**: 0.68 | 0.73 | 0.70
- **Weighted Avg**: 0.75 | 0.74 | 0.74

### Ringkasan Analisis IBM Granite

#### Penyebab Depresi
- **Masalah Hubungan**: Pernikahan yang tidak bahagia, kurangnya pengertian dari teman/keluarga.
- **Perjuangan Kesehatan Mental**: Pertarungan berkelanjutan dengan depresi dan kecemasan.
- **Kurangnya Dukungan**: Tidak adanya empati atau bantuan praktis dari orang terdekat.
- **Perubahan dan Kehilangan dalam Hidup**: Peristiwa signifikan seperti putus cinta atau kehilangan orang yang dicintai.
- **Isolasi Sosial dan Kesepian**: Keinginan untuk koneksi yang tulus tidak terpenuhi.

#### Penyebab Pikiran Bunuh Diri
- **Perasaan Menjadi Beban**: Merasa tidak berharga dan membebani orang lain.
- **Kelelahan dan Keputusasaan**: Kelelahan emosional, mental, atau fisik yang berkepanjangan.
- **Isolasi Sosial**: Merasa disalahpahami dan tidak memiliki siapa pun untuk diajak bicara.
- **Tekanan dan Ekspektasi Tak Terpenuhi**: Tekanan akademis atau sosial yang berat.
- **Trauma Masa Lalu dan Diskriminasi**: Pengalaman traumatis atau diskriminasi (misalnya, homofobia).

## 🛠️ Instalasi & Penggunaan

Proyek ini dikembangkan di lingkungan Google Colab. Untuk menjalankannya, ikuti langkah-langkah berikut:

### 1. Kloning Repositori (Opsional)

```bash
git clone https://your-repository-url.git
cd your-repository-url
```

### 2. Siapkan Kredensial

- **Kaggle API**: Untuk mengunduh dataset, unggah file `kaggle.json` Anda ke environment Colab.
- **Replicate API**: Untuk menjalankan analisis dengan IBM Granite, simpan token API Replicate Anda sebagai secret di Google Colab dengan nama `REPLICATE_API_TOKEN`.

### 3. Instal Dependensi

Jalankan sel berikut di notebook untuk menginstal semua library yang dibutuhkan:

```python
!pip install wordcloud scikit-learn matplotlib seaborn replicate pandas numpy torch transformers
```

### 4. Jalankan Notebook

Buka file `Capstone_Project_IBM_SkillsBuild.ipynb` di Google Colab dan jalankan sel-selnya secara berurutan. Pastikan untuk memilih runtime dengan akselerasi GPU untuk mempercepat proses pelatihan.

## Struktur Proyek
