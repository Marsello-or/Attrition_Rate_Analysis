# Proyek Akhir: Menyelesaikan Permasalahan Perusahaan Edutech (PT Jaya Jaya Maju)

## Business Understanding

PT Jaya Jaya Maju merupakan salah satu perusahaan multinasional berkembang yang telah berdiri sejak tahun 2000 dan saat ini menaungi lebih dari 1.000 karyawan yang tersebar di berbagai wilayah operasional negeri. Meskipun secara makro bisnis perusahaan terus bertumbuh, manajemen menghadapi tantangan internal yang sangat serius di bidang pengelolaan sumber daya manusia (SDM). Tantangan tersebut adalah tingginya angka *Attrition Rate* (rasio jumlah karyawan keluar dibandingkan total karyawan keseluruhan) yang telah menembus angka di atas 10% (tepatnya mencapai **16,92%** berdasarkan data historis yang divalidasi).

Tingginya volatilitas perputaran karyawan ini berpotensi mengganggu stabilitas produktivitas kerja, memicu hilangnya talenta-talenta kunci (*key talent*), serta membengkaknya beban finansial akibat proses rekrutmen dan pelatihan karyawan baru secara terus-menerus. Menanggapi situasi ini, manajer departemen HR meminta bantuan analitik untuk mengidentifikasi akar permasalahan pemicu tingginya angka *attrition* serta membangun sistem mitigasi risiko berbasis kecerdasan buatan (*Artificial Intelligence*).

### Permasalahan Bisnis & Jawaban Berbasis Data

Berikut adalah permasalahan bisnis utama dari departemen HR beserta jawaban konkret yang berhasil dibuktikan melalui proyek data science ini:

1. **Identifikasi Faktor Kunci:** Faktor-faktor operasional, finansial, dan demografis apa saja yang paling signifikan memengaruhi *attrition rate* karyawan di PT Jaya Jaya Maju?
   * **Jawaban:** Berdasarkan nilai *Feature Importance* dari model terbaik (CatBoost), faktor paling mematikan yang mendorong keputusan karyawan untuk keluar adalah **Kebijakan Lembur (`OverTime_Yes`)** yang memicu *burnout* kerja, diikuti oleh **Tingkat Opsi Saham (`StockOptionLevel`)** sebagai faktor pengikat finansial jangka panjang, serta karakteristik demografi **Status Pernikahan Lajang (`MaritalStatus_Single`)**.
2. **Karakteristik & Profil Risiko:** Bagaimana profil atau karakteristik utama dari karyawan yang memiliki kecenderungan tinggi untuk mengundurkan diri?
   * **Jawaban:** Hasil analisis deskriptif dan visualisasi dashboard menunjukkan bahwa profil karyawan yang paling rentan mengalami *attrition* di PT Jaya Jaya Maju adalah **karyawan berstatus lajang (laju keluar 26,70%)**, berada di **rentang pendapatan bulanan rendah (*Monthly Income* di bawah $5.000)**, ditempatkan di **Departemen Sales (attrition rate 20,69%)**, dan memegang peran spesifik sebagai **Sales Representative (attrition rate kritis 43,1%)**.
3. **Mitigasi Preventif:** Bagaimana merancang sistem pengawasan berbasis kecerdasan buatan untuk mendeteksi karyawan aktif saat ini yang masuk ke dalam kategori risiko tinggi (*High Risk*) sebelum mereka memutuskan keluar?
   * **Jawaban:** Solusi diimplementasikan menggunakan model **CatBoost Classifier** dengan penyesuaian *decision threshold* sensitif bisnis sebesar **0.35**. Sistem ini berhasil menjaring **9 karyawan aktif saat ini yang masuk kategori High Risk** pada data pengujian, di mana pola perilaku kerja dan kompensasi mereka sudah 75% identik dengan data historis mantan karyawan yang telah keluar. Daftar ini disajikan secara *real-time* pada menu *Risk Watchlist* di halaman 3 Business Dashboard untuk memicu tindakan *Stay Interview* oleh HR.
4. **Justifikasi Nilai Investasi:** Bagaimana mengonversi performa akurasi model prediktif data science menjadi efisiensi penghematan anggaran HR secara riil?
   * **Jawaban:** Dilakukan melalui metode *Cost-Benefit Analysis* (CBA). Dengan tingkat jaring radar (*Recall*) sebesar 75%, model CatBoost berhasil menangkap mayoritas sinyal karyawan berisiko. Melalui biaya program retensi preventif sebesar \$1.500 per orang versus biaya kerugian penuh kehilangan karyawan sebesar \$10.000 per orang, penerapan model ini secara nyata memberikan **total penghematan anggaran bersih sebesar \$139.500 (meningkat hampir 4 kali lipat lebih efisien** dibandingkan pendekatan model klasifikasi standar).

### Sumber Data
[Sumber Data Utama](https://github.com/dicodingacademy/dicoding_dataset/blob/main/employee/employee_data.csv)

### Cakupan Proyek

* **Analisis Data Eksploratif (EDA):** Skrining menyeluruh terhadap karakteristik kompensasi, keuangan, beban kerja, kepuasan kerja, dan hubungan industrial untuk menemukan tren perilaku *attrition*.
* **Pembuktian Statistik:** Menguji tingkat signifikansi hubungan variabel-variabel independen terhadap status loyalitas karyawan secara kuantitatif.
* **Eksperimen Pemodelan Komparatif:** Melatih, menguji, dan membandingkan 4 arsitektur algoritma *Machine Learning* berbasis pohon keputusan (*Tree-Based*) menggunakan ambang batas keputusan (*decision threshold*) kustom guna memitigasi masalah data tidak seimbang (*class imbalance*).
* **Integrasi Dashboard Bisnis:** Menyatukan data operasional historis dengan skor risiko buatan AI ke dalam satu berkas final untuk divisualisasikan secara interaktif.
* **Deployment Engineering:** Membangun berkas siap pakai `predict.py` beserta penataan dependensinya agar tim HR dapat melakukan prediksi mandiri di masa depan.

### Persiapan

* **Sumber data:** Dataset internal HR PT Jaya Jaya Maju yang memuat data atribut kompensasi, jam kerja, kepuasan, demografi, serta masa kerja dari 1.058 karyawan aktif dan 179 karyawan keluar.
* **Setup environment:** Untuk menjalankan seluruh siklus pemodelan prediktif dan script deployment secara konsisten, pasang berkas pustaka pendukung berikut:

```text
pandas>=1.3.0
numpy>=1.20.0
scipy>=1.7.0
matplotlib>=3.4.0
seaborn>=0.11.0
scikit-learn>=1.0.0
imbalanced-learn>=0.8.0
lightgbm>=3.2.0
catboost>=1.0.0
joblib>=1.1.0
```

---

## Business Dashboard

Business Dashboard dirancang menggunakan Google Looker Studio untuk memfasilitasi tim HR dan jajaran eksekutif dalam memonitor kesehatan SDM serta mengeksekusi intervensi retensi karyawan secara preventif. Dashboard ini terdiri dari 3 halaman utama:

1. **Halaman 1: Attrition Executive Summary (Tingkat Makro)**
   * Menyajikan rangkuman status retensi makro (Total Karyawan Aktif: 1.058, Attrition Rate: 16,92%, Total Keluar: 179, Estimasi Kerugian Finansial: \$1,79 Juta).
   * Visualisasi sebaran per departemen (kebocoran terbesar di tim Sales sebesar 20,69%) dan per peran jabatan (posisi Sales Representative mengalami tingkat attrition kritis sebesar 43,1%).
2. **Halaman 2: Deep-Dive Drivers of Attrition (Faktor Pemicu)**
   * Membuktikan pengaruh ekstrem variabel kebijakan kerja lembur (*Overtime*), nihilnya opsi kepemilikan saham (*Stock Option Level 0*), dan risiko demografi status pernikahan lajang (*Single*) pada kelompok rentang pendapatan bulanan rendah.
3. **Halaman 3: AI Risk Watchlist & Mitigation (Tindakan Preventif)**
   * Menyediakan menu utama operasional HR berupa tabel prediksi karyawan aktif saat ini yang diidentifikasi berisiko tinggi (*High Risk*) lengkap dengan kolom *Risk Score (%)* dan *Risk Category* hasil pemodelan AI.
   * Menyajikan kalkulasi penghematan keuangan intervensi di mana penanganan terhadap 9 karyawan aktif yang saat ini terdeteksi *High Risk* membutuhkan biaya retensi sebesar \$13.500 namun berpotensi menyelamatkan anggaran bersih perusahaan sebesar **\$76.500**.

**[Akses Business Attrition Dashboard PT Jaya Jaya Maju](https://datastudio.google.com/reporting/b64aff7a-bcf4-4846-934b-2d273dd9e935)**

---

## Machine Learning Modeling & Evaluation

Eksperimen dilakukan secara ketat dengan menguji 4 jenis model *Tree-Based* memanfaatkan ambang batas keputusan (*Decision Threshold*) kustom sebesar **0.35** guna mengatasi ketidakseimbangan kelas (*class imbalance*) pada kelas target.

Berikut adalah hasil performa riil dari keempat model pada data pengujian (*Test Data*):

| Model Name | ROC-AUC | Recall (Resign) | Precision (Resign) | F1-Score (Resign) | Global Accuracy |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Random Forest (SMOTE)** | 0.8043 | 0.58 | 0.51 | 0.55 | 0.83 |
| **Balanced Random Forest** | 0.7950 | 0.83 | 0.27 | 0.41 | 0.58 |
| **LightGBM (Weighted)** | 0.7263 | 0.56 | 0.36 | 0.43 | 0.75 |
| **CatBoost (Weighted)** | 0.7994 | 0.75 | 0.37 | 0.50 | 0.74 |

### Analisis Karakteristik Model:
* **Random Forest (SMOTE):** Memberikan akurasi global (83%) dan presisi (51%) tertinggi, namun radarnya kurang sensitif karena meloloskan cukup banyak karyawan yang ingin keluar (*Recall* hanya 58%).
* **Balanced Random Forest:** Mencatatkan *Recall* tertinggi (83%), tetapi terlalu agresif sehingga akurasi global jatuh ke 58% dan presisi hancur di angka 27% akibat terlalu banyak menebak salah (*False Positive* ekstrem).
* **LightGBM (Weighted):** Menunjukkan performa paling rendah pada dataset tabular berukuran menengah ini dengan nilai ROC-AUC terkecil (0.7263).
* **CatBoost (Weighted):** Terpilih sebagai **Model Final Paling Robust**. Model ini berhasil mengunci nilai ROC-AUC yang tinggi (**0.7994**) serta mendongkrak daya jaring radar klasifikasi hingga mencapai **Recall 75%** (berhasil menangkap 27 dari 36 karyawan aktual yang bermasalah) dengan performa model keseluruhan yang jauh lebih stabil.

### Faktor Pemicu Utama (Feature Importance):
Berdasarkan visualisasi *Feature Importance* dari model terbaik, ditemukan bahwa 3 faktor utama penentu attrition di PT Jaya Jaya Maju adalah:
1. **Kebijakan Lembur (`OverTime_Yes`)** — Menempati peringkat pertama sebagai pemicu kelelahan fisik dan mental kerja (*burnout*).
2. **Tingkat Opsi Saham (`StockOptionLevel`)** — Tidak adanya hak kepemilikan saham membuat karyawan tidak memiliki ikatan finansial jangka panjang dengan masa depan perusahaan.
3. **Status Pernikahan Lajang (`MaritalStatus_Single`)** — Karyawan lajang memiliki fleksibilitas tinggi untuk berpindah kantor karena beban tanggung jawab domestik keluarga belum seberat karyawan yang sudah menikah.

---

## Conclusion

Proyek ini menyimpulkan bahwa tingginya laju *attrition* di PT Jaya Jaya Maju (16,92%) dapat dimitigasi secara efektif menggunakan pendekatan berbasis data prediktif. Melalui model kecerdasan buatan **CatBoost (Weighted)**, perusahaan berhasil mengubah paradigma pengelolaan retensi karyawan dari reaktif menjadi preventif. 

Model CatBoost ini terbukti memiliki kemampuan pemisahan risiko yang sangat robust (ROC-AUC: 0.7994) dan sangat sensitif dalam menangkap 75% karyawan bermasalah.

### Analisis Dampak Finansial Bisnis (Cost-Benefit Analysis)
Untuk menguji kelayakan proyek ini di mata jajaran eksekutif, metrik evaluasi disimulasikan ke dalam nilai finansial riil menggunakan tolok ukur biaya HR industri:
* **Cost of Attrition (Kerugian akibat 1 orang resign):** \$10.000 *(biaya rekrutmen ulang, waktu adaptasi, dan hilangnya produktivitas operasional)*.
* **Cost of Retention (Biaya intervensi pertahanan per orang):** \$1.500 *(anggaran insentif, penyesuaian fasilitas, atau waktu konseling khusus)*.

Melalui simulasi pada total **36 karyawan yang aktual mengundurkan diri** di data Test, berikut komparasi efisiensi penghematannya:

* **Skenario A: Menggunakan Model Random Forest (SMOTE)**
  * *Recall 58%* $\rightarrow$ Menjaring **21 orang** (*True Positive*), meloloskan **15 orang** (*False Negative*).
  * Penghematan dari karyawan yang diselamatkan: $21 \times (\$10.000 - \$1.500) = +\$178.500$
  * Kerugian akibat meloloskan karyawan: $15 \times \$10.000 = -\$150.000$
  * **Total Penghematan Bersih Anggaran: +\$28.500**

* **Skenario B: Menggunakan Model CatBoost (Weighted) — MODEL AKHIR**
  * *Recall 75%* $\rightarrow$ Menjaring **27 orang** (*True Positive*), meloloskan hanya **9 orang** (*False Negative*).
  * Penghematan dari karyawan yang diselamatkan: $27 \times (\$10.000 - \$1.500) = +\$229.500$
  * Kerugian akibat meloloskan karyawan: $9 \times \$10.000 = -\$90.000$
  * **Total Penghematan Bersih Anggaran: +\$139.500**

> **Justifikasi Bisnis:** Meskipun Random Forest memiliki akurasi global yang lebih tinggi, dari kacamata HR, meloloskan karyawan yang ingin keluar (*False Negative*) jauh lebih mahal harganya daripada salah memberikan perhatian kepada karyawan loyal (*False Positive*). Penerapan model CatBoost berhasil menyelamatkan anggaran operasional perusahaan sebesar **Anjlok Anggaran \$139.500 (Meningkat hampir 4x lipat lebih efisien)**.

### Rekomendasi Action Items

Berdasarkan kombinasi hasil pembuktian statistik dan visualisasi *Feature Importance*, berikut adalah beberapa rekomendasi *action items* strategis yang diajukan untuk divisi HR guna menyelesaikan permasalahan laju *attrition*:

- **Restrukturisasi Kebijakan Kerja Lembur (Overtime Controls):**
  Mengingat lembur merupakan pemicu utama *attrition* nomor satu, perusahaan wajib membatasi jam lembur mingguan maksimal dan menerapkan sistem peringatan dini *burnout*. Lakukan audit manajemen kerja secara berkala pada departemen yang memiliki jam kerja ekstrem (khususnya departemen Sales dan peran Sales Representative).
- **Evaluasi dan Pemerataan Program Opsi Kepemilikan Saham:**
  Variabel `StockOptionLevel` terbukti menjadi jangkar loyalitas yang sangat kuat. HR disarankan memperluas pemberian opsi kepemilikan saham atau insentif finansial jangka panjang, bukan hanya bagi level eksekutif senior, melainkan sebagai bentuk penghargaan bagi staf operasional berkinerja tinggi agar mereka merasa memiliki andil dalam masa depan perusahaan.
- **Program Keterikatan (Engagement) Khusus Karyawan Lajang:**
  Kelompok karyawan lajang mencatatkan kerentanan keluar yang tinggi. Perusahaan perlu membangun jalur karir yang transparan (*clear career path*), program mentoring berkala, serta aktivitas *work-life balance* kelompok untuk memperkuat ikatan emosional mereka dengan organisasi pada klaster pendapatan menengah ke bawah.

---

## Panduan Menjalankan Model (Per Tahapan)

Aplikasi ini dilengkapi dengan script mandiri `predict.py` untuk menguji risiko pada data karyawan baru di masa mendatang secara otomatis lewat terminal komputer tanpa perlu membuka Jupyter Notebook.

### Tahap 1: Setup Dependencies (Instalasi Library)
Buka aplikasi Terminal (macOS/Linux) atau Command Prompt (Windows) di folder tempat proyek ini disimpan, lalu jalankan perintah berikut untuk menginstal seluruh pustaka pendukung:

```bash
pip install -r requirements.txt
```

### Tahap 2: Memastikan Artefak Model Tersedia
Pastikan 3 file biner hasil ekspor dari proses training di notebook utama diletakkan di dalam folder direktori kerja yang sama dengan file `predict.py`:
* `model_attrition_catboost.pkl` *(File biner struktur dan bobot model CatBoost)*
* `scaler_attrition.pkl` *(File biner alat penskalaan fitur StandardScaler)*
* `feature_columns.pkl` *(Metadata berisi daftar urutan kolom encoding asli)*

### Tahap 3: Eksekusi Jalur Prediksi (Running Script)
Siapkan file CSV berisi data profil karyawan baru yang ingin diprediksi (misal: `karyawan_baru.csv`), lalu jalankan perintah di bawah ini melalui terminal Anda:

```bash
python predict.py karyawan_baru.csv hasil_prediksi_risiko.csv
```

### Tahap 4: Pemeriksaan Hasil Analitik Risiko
Script otomatis membaca data input baru, menyelaraskan kolom dummy, melakukan scaling, menerapkan threshold keputusan sensitif bisnis 0.35, dan mengekspor file baru bernama `hasil_prediksi_risiko.csv`. File output ini secara otomatis sudah diperkaya dengan dua kolom analitik baru:
* **`Risk_Score (%)`**: Probabilitas persentase kecenderungan karyawan tersebut untuk mengundurkan diri.
* **`Risk_Category`**: Label tingkatan tindakan penanganan bagi HR (*Low Risk*, *Medium Risk*, atau *High Risk*).
