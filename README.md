 📘 UTS Data Mining : Analisis Imbalanced Data
 
 Anggota Kelompok : 
 - Muhammad Zahran Albara (122140240)

   - Casey Z.D Manurung (122140054)

   - Royfran Roger Valentino (122140239)

📊 Analisis & Penanganan *Imbalanced Data* pada Prediksi Permintaan dan Pembelian Obat

🧠 Deskripsi Proyek

Proyek ini bertujuan untuk mempelajari cara **menangani ketidakseimbangan data (imbalanced data)** dalam konteks prediksi kategori obat berdasarkan data transaksi dan stok.  
Dalam dunia farmasi, sering kali transaksi **didominasi oleh "Obat Bebas"** dibandingkan **"Obat Resep Dokter"**, sehingga model cenderung bias terhadap kelas mayoritas.

💻 Cara Menjalankan Notebook

1. Clone repo ini:
   ```bash
   git clone https://github.com/Zahran812/UTS_Data_Mining.git
   cd UTS_Data_Mining
2.  Aktifkan environment :

    python -m venv venv
    source venv/Scripts/activate  # di Windows
    
    atau
    
    source venv/bin/activate      # di Linux/Mac
3. Install dependensi:
    pip install -r requirements.txt
4. Jalankan file utama:
    Buka file imbalanced_analysis.ipynb
    Jalankan semua sel satu per satu dari atas ke bawah (Ctrl+Enter atau Shift+Enter).
5. Lihat hasil:

    Output metrik akan muncul di bawah sel evaluasi.

    Visualisasi seperti confusion matrix dan ROC curve akan tampil langsung di notebook.


Dataset yang digunakan:
- **`df_pembelian`** → data transaksi pembelian obat (±140.000 baris, hasil pembersihan).
- **`df_stok`** → data stok obat, termasuk kolom `KODE`, `NAMA PRODUK`, `LOKASI`, `QTY.STOK`, dan `UNIT`.

---

🧹 Tahap 1 — Pembersihan Data
Data mentah memiliki kolom gabung dan format tidak standar.  
Langkah-langkah pembersihan yang dilakukan:
- Menghapus baris kosong dan garis pemisah.
- Menormalkan format angka (misal `1.234,00` menjadi `1234.00`).
- Memisahkan kolom `KODE`, `NAMA PRODUK`, `LOKASI`, `UNIT`, dan `QTY.STOK`.
- Menghapus spasi berlebih dan duplikat.
- Hasil akhir disimpan sebagai:
  - `Dataset_Stok_clean.csv`
  - `Dataset_Pembelian_clean.csv`

---

⚖️ Tahap 2 — Simulasi *Imbalanced Dataset*
Dari data pembelian yang bersih, dibuat simulasi ketidakseimbangan kategori:
- **95%** transaksi = *Obat Bebas*  
- **5%** transaksi = *Obat Resep Dokter*

Tujuan simulasi: menguji berbagai pendekatan *imbalanced data handling*.

---

🔍 Tahap 3 — Analisis Distribusi Kategori
Distribusi awal divisualisasikan menggunakan `seaborn`:

sns.countplot(x=df_pembelian["kategori"])

Menampilkan distribusi jumlah transaksi per kategori (Obat Bebas vs Obat Resep Dokter).



🧩 Tahap 4 — Penanganan Imbalanced Data
1️⃣ Resampling Techniques (Data-Level Methods)

Metode: SMOTE (Synthetic Minority Oversampling Technique)
Menyeimbangkan dataset dengan menambahkan contoh sintetis kelas minoritas.

Visualisasi:

Distribusi kategori sebelum & sesudah SMOTE

Peningkatan jumlah data minoritas

Kelebihan: cepat dan mudah diterapkan.
Kekurangan: dapat menambah noise jika data minoritas awalnya sangat sedikit.

2️⃣ Algorithm-Level Methods

Metode: Random Forest Classifier dengan parameter class_weight='balanced'

Model ini memberi bobot kesalahan lebih besar untuk kelas minoritas agar tidak diabaikan selama pelatihan.

model_balanced = RandomForestClassifier(class_weight='balanced')


Kelebihan: tidak perlu menambah data sintetis.
Kekurangan: performa tergantung pada distribusi alami data.

3️⃣ Hybrid Methods

Menggabungkan:

SMOTE (Resampling)

class_weight='balanced' (Algorithm Adjustment)

Pendekatan ini terbukti paling seimbang antara recall tinggi dan precision stabil.

⚙️ Tahap 5 — Model yang Digunakan
🎯 Random Forest Classifier

Random Forest adalah algoritma ensemble learning yang menggabungkan banyak pohon keputusan (decision trees) untuk membuat prediksi akhir dengan metode voting.

Kelebihan:

Mampu menangani data non-linear dan beragam fitur.

Tidak mudah overfitting.

Dapat menghitung pentingnya fitur (feature importance).

Parameter penting:

n_estimators: jumlah pohon keputusan.

max_depth: kedalaman maksimal setiap pohon.

class_weight: menangani ketidakseimbangan kelas.

📊 Tahap 6 — Evaluasi Model

Karena dataset tidak seimbang, akurasi saja tidak cukup.
Metrik yang digunakan:

Metrik	Arti	Fokus
Precision	Seberapa banyak prediksi positif yang benar	Ketepatan
Recall (Sensitivity)	Seberapa banyak kasus positif yang terdeteksi	Kelengkapan
F1-Score	Rata-rata harmonis Precision dan Recall	Keseimbangan
ROC-AUC	Kemampuan model membedakan dua kelas	Discriminative Power

Visualisasi evaluasi:

    1.Confusion Matrix

    2.ROC Curve

    3.Grafik batang Precision, Recall, F1 untuk tiap kategori

Kesimpulan : Model prediksi stok obat menjadi lebih adil dan tidak bias terhadap kategori mayoritas setelah diterapkan metode imbalanced data handling (khususnya pendekatan hybrid).
Pendekatan ini direkomendasikan untuk kasus nyata di mana proporsi data sangat timpang.
