# Laporan Proyek Data Science: Mengurangi Attrition Rate Karyawan PT Jaya Jaya Maju

## 1. Latar Belakang Proyek
PT Jaya Jaya Maju merupakan salah satu perusahaan multinasional berkembang yang telah berdiri sejak tahun 2000 dan saat ini menaungi lebih dari 1.000 karyawan yang tersebar di berbagai wilayah operasional negeri. Meskipun secara makro bisnis perusahaan terus bertumbuh, manajemen menghadapi tantangan internal yang sangat serius di bidang pengelolaan sumber daya manusia (SDM). Tantangan tersebut adalah tingginya angka Attrition Rate (rasio jumlah karyawan keluar dibandingkan total karyawan keseluruhan) yang telah menembus angka di atas 10%.

Tingginya volatilitas perputaran karyawan ini berpotensi mengganggu stabilitas produktivitas kerja, memicu hilangnya talenta-talenta kunci (key talent), serta membengkaknya beban finansial akibat proses rekrutmen dan pelatihan karyawan baru secara terus-menerus. Menanggapi situasi ini, manajer departemen HR meminta bantuan analitik untuk mengidentifikasi akar permasalahan pemicu tingginya angka attrition serta membangun sistem mitigasi risiko berbasis kecerdasan buatan (Artificial Intelligence).

---

## 2. Ruang Lingkup Proyek (Scope)
* Analisis Data Eksploratif (EDA): Melakukan skrining menyeluruh terhadap karakteristik demografi, keuangan, beban kerja, dan hubungan industrial karyawan untuk menemukan tren atau pola perilaku attrition.
* Pembuktian Statistik: Menguji tingkat signifikansi hubungan variabel-variabel independen terhadap status loyalitas karyawan.
* Eksperimen Pemodelan Komparatif: Melatih, menguji, dan membandingkan 4 arsitektur algoritma Machine Learning berbasis pohon keputusan (Tree-Based) menggunakan ambang batas (threshold) kustom untuk mencari akurasi radar deteksi terbaik.
* Integrasi Hasil Analitik & Dashboard: Menyatukan data operasional historis dengan skor risiko buatan AI ke dalam satu berkas untuk divisualisasikan pada business dashboard interaktif.

---

## 3. Pertanyaan Bisnis (Business Questions)
Proyek ini dirancang secara terstruktur untuk menjawab 3 pertanyaan bisnis utama dari departemen HR:
1. Faktor apa saja yang paling signifikan memengaruhi attrition rate karyawan di PT Jaya Jaya Maju?
2. Bagaimana karakteristik atau profil utama dari karyawan yang memiliki kecenderungan tinggi untuk mengundurkan diri?
3. Siapa saja karyawan yang saat ini masih aktif yang masuk ke dalam kategori risiko tinggi (High Risk) untuk resign, sehingga HR dapat melakukan tindakan pencegahan lebih awal?

---

## 4. Kesimpulan Akhir Hasil Evaluasi (Notebook Summary)
Eksperimen dilakukan secara ketat dengan menguji 4 algoritma berbeda menggunakan ambang batas keputusan (Decision Threshold) kustom sebesar 0.35 untuk mengatasi masalah ketidakseimbangan kelas (class imbalance) pada data. 

Berikut adalah hasil performa riil dari keempat model pada data pengujian (Test Data):

| Model Name | ROC-AUC | Recall (Resign) | Precision (Resign) | F1-Score (Resign) | Global Accuracy |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Random Forest (SMOTE) | 0.8043 | 0.58 | 0.51 | 0.55 | 0.83 |
| Balanced Random Forest | 0.7950 | 0.83 | 0.27 | 0.41 | 0.58 |
| LightGBM (Weighted) | 0.7263 | 0.56 | 0.36 | 0.43 | 0.75 |
| CatBoost (Weighted) | 0.7994 | 0.75 | 0.37 | 0.50 | 0.74 |

### Analisis Karakteristik Model:
* Random Forest (SMOTE) memberikan akurasi global (83%) dan presisi (51%) tertinggi, namun radarnya kurang sensitif karena meloloskan cukup banyak karyawan yang ingin keluar (Recall hanya 58%).
* Balanced Random Forest mencatatkan Recall tertinggi (83%), tetapi terlalu agresif sehingga akurasi global jatuh ke 58% dan presisi hancur di angka 27% akibat terlalu banyak menebak salah (False Positive ekstrem).
* LightGBM (Weighted) menunjukkan performa paling rendah pada dataset tabular berukuran menengah ini dengan nilai ROC-AUC terkecil (0.7263).
* CatBoost (Weighted) terpilih sebagai Model Final Paling Robust. Model ini berhasil mengunci nilai ROC-AUC yang tinggi (0.7994) serta mendongkrak daya jaring radar klasifikasi hingga mencapai Recall 75% (berhasil menangkap 27 dari 36 karyawan aktual yang bermasalah) dengan performa model keseluruhan yang jauh lebih stabil.

### Faktor Pemicu Utama (Feature Importance):
Berdasarkan visualisasi Feature Importance dari model terbaik, ditemukan bahwa 3 faktor utama penentu attrition di PT Jaya Jaya Maju adalah:
1. Kebijakan Lembur (OverTime_Yes) — Menempati peringkat pertama sebagai pemicu kelelahan fisik dan mental kerja (burnout).
2. Tingkat Opsi Saham (StockOptionLevel) — Tidak adanya hak kepemilikan saham membuat karyawan tidak memiliki ikatan finansial jangka panjang dengan masa depan perusahaan.
3. Status Pernikahan Lajang (MaritalStatus_Single) — Karyawan lajang memiliki fleksibilitas tinggi untuk berpindah kantor karena beban tanggung jawab domestik keluarga belum seberat karyawan yang sudah menikah.

---

## 5. Analisis Dampak Finansial Bisnis (Cost-Benefit Analysis)
Ukuran keberhasilan proyek ini dievaluasi langsung menggunakan metrik finansial riil berdasarkan tolok ukur biaya operasional HR industri:
* Cost of Attrition (Kerugian akibat 1 orang resign): $10.000 (biaya rekrutmen ulang, waktu adaptasi, dan hilangnya produktivitas operasional).
* Cost of Retention (Biaya intervensi pertahanan per orang): $1.500 (anggaran insentif, penyesuaian fasilitas, atau waktu konseling khusus).

Melalui simulasi pada total 36 karyawan yang aktual mengundurkan diri di data Test, berikut komparasi efisiensi penghematannya:

* Skenario A: Menggunakan Model Random Forest (SMOTE)
    * Recall 58% -> Menjaring 21 orang (True Positive), meloloskan 15 orang (False Negative).
    * Penghematan dari karyawan yang diselamatkan: 21 x ($10.000 - $1.500) = +$178.500
    * Kerugian akibat meloloskan karyawan: 15 x $10.000 = -$150.000
    * Total Penghematan Bersih Anggaran: +$28.500

* Skenario B: Menggunakan Model CatBoost (Weighted)
    * Recall 75% -> Menjaring 27 orang (True Positive), meloloskan hanya 9 orang (False Negative).
    * Penghematan dari karyawan yang diselamatkan: 27 x ($10.000 - $1.500) = +$229.500
    * Kerugian akibat meloloskan karyawan: 9 x $10.000 = -$90.000
    * Total Penghematan Bersih Anggaran: +$139.500

>  Justifikasi Bisnis: Meskipun Random Forest memiliki akurasi global yang lebih tinggi, dari kacamata HR, meloloskan karyawan yang ingin keluar (False Negative) jauh lebih mahal harganya daripada salah memberikan perhatian kepada karyawan loyal (False Positive). Penerapan model CatBoost berhasil menyelamatkan anggaran operasional perusahaan sebesar $139.500 (Meningkat hampir 4x lipat lebih efisien).

---

## 6. Recommendation Action Items untuk PT Jaya Jaya Maju
Berdasarkan kombinasi hasil pembuktian statistik dan visualisasi Feature Importance, berikut adalah rekomendasi strategis yang diajukan untuk divisi HR:

1. Restrukturisasi Kebijakan Kerja Lembur (Overtime Controls):
   Mengingat lembur merupakan pemicu utama attrition, perusahaan wajib membatasi jam lembur mingguan maksimal dan menerapkan sistem peringatan burnout. Lakukan audit manajemen kerja secara berkala pada departemen yang memiliki jam kerja ekstrem.
2. Evaluasi dan Pemerataan Program Opsi Kepemilikan Saham:
   Variabel StockOptionLevel terbukti menjadi jangkar loyalitas yang sangat kuat. HR disarankan memperluas pemberian opsi kepemilikan saham atau insentif jangka panjang, bukan hanya bagi level eksekutif senior, melainkan sebagai bentuk penghargaan bagi staf berkinerja tinggi agar mereka merasa memiliki andil dalam masa depan perusahaan.
3. Program Keterikatan (Engagement) Khusus Karyawan Lajang:
   Kelompok karyawan lajang mencatatkan kerentanan keluar yang tinggi. Perusahaan perlu membangun jalur karir yang transparan (clear career path), program mentoring berkala, serta aktivitas work-life balance kelompok untuk memperkuat ikatan emosional mereka dengan organisasi.

---

## 7. Panduan Menjalankan Model (Per Tahapan)

Aplikasi ini dilengkapi dengan script mandiri predict.py untuk menguji risiko pada data karyawan baru di masa mendatang secara otomatis lewat terminal komputer tanpa perlu membuka Jupyter Notebook.

### Tahap 1: Setup Dependencies (Instalasi Library)
Pastikan Python sudah terinstal di komputer Anda. Buka aplikasi Terminal (macOS/Linux) atau Command Prompt (Windows) di folder tempat proyek ini disimpan, lalu jalankan perintah berikut untuk menginstal seluruh pustaka pendukung:

```bash
pip install -r requirements.txt
```

### Tahap 2: Memastikan Artefak Model Tersedia
Pastikan 3 file biner hasil ekspor dari proses training di notebook utama diletakkan di dalam folder direktori kerja yang sama dengan file predict.py:
* model_attrition_catboost.pkl (File biner struktur dan bobot model CatBoost)
* scaler_attrition.pkl (File biner alat penskalaan fitur StandardScaler)
* feature_columns.pkl (Metadata berisi daftar urutan kolom encoding asli)

### Tahap 3: Eksekusi Jalur Prediksi (Running Script)
Siapkan file CSV berisi data profil karyawan baru yang ingin diprediksi (misal: karyawan_baru.csv), lalu jalankan perintah di bawah ini melalui terminal Anda:

```bash
python predict.py karyawan_baru.csv hasil_prediksi_risiko.csv
```

### Tahap 4: Pemeriksaan Hasil Analitik Risiko
Script otomatis membaca data input baru, menyelaraskan kolom dummy, melakukan scaling, menerapkan threshold keputusan sensitif bisnis 0.35, dan mengekspor file baru bernama hasil_prediksi_risiko.csv. File output ini secara otomatis sudah diperkaya dengan dua kolom analitik baru:
* Risk_Score (%): Probabilitas persentase kecenderungan karyawan tersebut untuk mengundurkan diri.
* Risk_Category: Label tingkatan tindakan penanganan bagi HR (Low Risk, Medium Risk, atau High Risk).

---

## 8. Link Akses Dashboard Interaktif (Google Looker Studio)
Untuk memantau sebaran data demografi historis, visualisasi korelasi faktor kerja, serta memantau daftar radar pantauan (Risk Watchlist) karyawan aktif secara real-time, silakan akses tautan Google Looker Studio resmi kami di bawah ini:

 **[Akses Business Attrition Dashboard PT Jaya Jaya Maju](https://datastudio.google.com/reporting/b64aff7a-bcf4-4846-934b-2d273dd9e935)**