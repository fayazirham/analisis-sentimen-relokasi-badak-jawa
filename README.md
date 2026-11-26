# Analisis Sentimen Komentar Relokasi Badak

Notebook ini menganalisis sentimen komentar YouTube tentang relokasi badak menggunakan leksikon InSet dan model LSTM. Alur analisis mencakup pengambilan komentar, prapemrosesan teks Bahasa Indonesia, pelabelan sentimen berbasis leksikon, visualisasi, serta klasifikasi dengan LSTM.

## Struktur proyek

```text
badak-lstm/
├── data/
│   ├── processed/       # Data hasil pemrosesan lokal, tidak dilacak Git
│   └── raw/             # Data hasil crawling lokal, tidak dilacak Git
├── docs/
│   └── Relokasi_Badak_Lexicon_LSTM.pdf
├── notebooks/
│   └── Relokasi_Badak_Lexicon_LSTM.ipynb
├── .gitignore
├── README.md
└── requirements.txt
```

## Persiapan

Gunakan versi Python yang didukung oleh versi TensorFlow di lingkungan Anda. Buat dan aktifkan virtual environment, lalu pasang dependensi:

```bash
python -m venv .venv
python -m pip install -r requirements.txt
```

Buka notebook di Jupyter atau Google Colab dan jalankan sel sesuai urutan. Notebook dapat mengunduh resource NLTK dan leksikon InSet saat dijalankan; proses itu memerlukan akses internet.

## YouTube Data API

Untuk sel yang mengambil video dan komentar, sediakan kunci YouTube Data API melalui environment variable `YOUTUBE_API_KEY`. Di Google Colab, kunci dapat disimpan di Colab Secrets dengan nama yang sama. Jangan menaruh nilai kunci di notebook, README, atau berkas proyek.

## Data

Hasil crawling dan data olahan tidak disertakan. Simpan data lokal pada `data/raw/` atau `data/processed/`; berkas CSV dan keluaran model/checkpoint sengaja dikecualikan dari Git.
