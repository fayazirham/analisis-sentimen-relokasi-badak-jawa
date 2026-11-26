# Analisis Sentimen Komentar YouTube tentang Relokasi Badak Jawa

Proyek ini menganalisis komentar YouTube mengenai wacana relokasi Badak Jawa. Notebook menggabungkan pelabelan berbasis **Indonesia Sentiment Lexicon (InSet)** dan klasifikasi teks menggunakan model **Bidirectional LSTM**.

## Ringkasan alur

1. Mencari video YouTube berbahasa Indonesia dengan kata kunci `relokasi badak jawa`.
2. Mengambil komentar tingkat atas dari video yang ditemukan.
3. Membersihkan teks, melakukan case folding, tokenisasi, penghapusan stopword dengan mempertahankan kata negasi, dan stemming menggunakan Sastrawi.
4. Memberi label sentimen negatif, netral, atau positif berdasarkan skor kata pada kamus InSet.
5. Mengeksplorasi distribusi sentimen dan kata-kata yang sering muncul.
6. Melatih dan mengevaluasi model Bidirectional LSTM.

## Struktur proyek

```text
badak-lstm/
├── data/
│   ├── processed/       # CSV hasil pemrosesan lokal (diabaikan Git)
│   └── raw/             # CSV hasil crawling lokal (diabaikan Git)
├── docs/
│   └── Relokasi_Badak_Lexicon_LSTM.pdf
├── notebooks/
│   └── Relokasi_Badak_Lexicon_LSTM.ipynb
├── .gitignore
├── README.md
└── requirements.txt
```

## Persyaratan

- Python versi yang kompatibel dengan TensorFlow yang akan dipasang.
- Akses internet saat notebook mengunduh resource NLTK dan kamus InSet.
- Kunci YouTube Data API v3 jika ingin mengambil data langsung dari YouTube.

## Instalasi

Buka terminal pada direktori utama proyek, lalu buat dan aktifkan virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Jika PowerShell menolak aktivasi environment, jalankan perintah aktivasi yang sesuai untuk terminal yang digunakan atau jalankan Python dari `.venv` secara langsung.

## Menjalankan notebook

1. Buka `notebooks/Relokasi_Badak_Lexicon_LSTM.ipynb` menggunakan Jupyter atau unggah ke Google Colab.
2. Jalankan sel secara berurutan dari atas.
3. Notebook mengunduh resource NLTK dan berkas kamus InSet saat diperlukan.
4. Untuk crawling YouTube, atur `YOUTUBE_API_KEY` terlebih dahulu.
5. Jika CSV hasil crawling sudah tersedia, notebook dapat memuatnya alih-alih melakukan crawling ulang.

### Mengatur kunci YouTube API

Di Google Colab, simpan kunci melalui **Secrets** dengan nama `YOUTUBE_API_KEY`. Untuk sesi PowerShell lokal, kunci dapat diberikan sebagai environment variable sementara:

```powershell
$env:YOUTUBE_API_KEY = "KUNCI_API_ANDA"
```

Jangan menuliskan kunci asli ke notebook, README, atau berkas yang akan dibagikan. Jangan commit berkas `.env`.

## Berkas data CSV

Simpan data lokal pada lokasi berikut:

| Berkas | Lokasi yang disarankan |
|---|---|
| `youtube_comments_relokasi_badak_jawa.csv` | `data/raw/` |
| `comment_lengkap.csv` | `data/processed/` |
| `comment_data_clean_polarity.csv` | `data/processed/` |

**Catatan path:** pada versi notebook saat ini, nama CSV ditulis relatif terhadap *working directory*. Notebook tidak otomatis mencari input di `data/raw/`; sesuaikan `RAW_COMMENTS_FILE` ke `data/raw/youtube_comments_relokasi_badak_jawa.csv` jika notebook dijalankan dari direktori utama proyek. Sel yang menulis `comment_lengkap.csv` dan `comment_data_clean_polarity.csv` juga menyimpan hasil ke *working directory* saat ini. Sesuaikan path `to_csv(...)` ke `data/processed/` bila ingin menyimpannya di sana.

CSV sengaja dikecualikan dari Git melalui `.gitignore`. Data dapat memuat teks komentar dan nama/identitas kanal atau penulis; periksa dan anonimisasi bila diperlukan sebelum membagikan.

## Hasil yang tercatat

Pada keluaran notebook yang tersedia:

- Komentar yang terkumpul: **1.925**
- Komentar setelah preprocessing: **1.901**
- Distribusi label InSet: **1.228 negatif**, **340 netral**, dan **333 positif**
- Akurasi pada data uji: **68,24%**
- Macro F1-score: **0,60**

Nilai tersebut adalah hasil dari data dan konfigurasi yang tersimpan di notebook, bukan jaminan hasil akan sama saat dijalankan ulang. Pengambilan video/komentar, perubahan kamus, versi pustaka, dan proses pelatihan dapat memengaruhi hasil.

## Batasan interpretasi

- Label target dibuat otomatis dari InSet, bukan anotasi manual. Karena itu, akurasi LSTM mengukur kesesuaian dengan label leksikon dan **belum membuktikan kesesuaian dengan penilaian manusia**.
- Kata negasi dipertahankan pada preprocessing, tetapi pelabelan leksikon tidak memahami cakupan negasi atau sarkasme secara penuh.
- Data tidak seimbang; pada keluaran yang tersimpan, F1 kelas positif adalah **0,41** dan recall kelas netral **0,44**.
- Pengambilan komentar dibatasi pada hasil pencarian dan komentar tingkat atas, sehingga sampel tidak mewakili seluruh opini publik.
- Untuk evaluasi yang lebih kuat, anotasi sebagian data secara manual dan gunakan pemisahan data berdasarkan video.

## Keamanan dan privasi

- Jangan commit kunci API, berkas `.env`, atau data mentah/hasil olahan.
- CSV lokal, cache, dan checkpoint model telah dikecualikan dari Git melalui `.gitignore`.
- Sebelum membagikan dataset atau keluaran notebook, periksa apakah terdapat informasi yang dapat mengidentifikasi penulis komentar.
