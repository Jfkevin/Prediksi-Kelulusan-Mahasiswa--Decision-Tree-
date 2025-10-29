# 🎓 Prediksi Kelulusan Mahasiswa Menggunakan Algoritma Decision Tree
### _Proyek UTS – IS411 Data Modelling (Universitas Multimedia Nusantara)_

---

## 🧠 Ringkasan Proyek
Proyek ini bertujuan untuk **memprediksi kelulusan mahasiswa (Pass/Fail)** berdasarkan data akademik menggunakan pendekatan **data modelling** dan **algoritma machine learning Decision Tree**. Proyek ini dikembangkan sebagai bagian dari **Ujian Tengah Semester (UTS)** mata kuliah **IS411 – Data Modelling**, yang berfokus pada penerapan teknik **pembersihan data, analisis deskriptif**, serta **pemodelan klasifikasi** untuk memahami pola kelulusan mahasiswa.

---

## 👨‍💻 Identitas Mahasiswa
| Nama | NIM | Kelas |
|------|-----|--------|
| **Farrelius Kevin** | 00000081783 | IS411 – A |

---

## 📘 Informasi Mata Kuliah
**Mata Kuliah:** IS411 – Data Modelling  
**Semester:** 7 Ganjil 2025/2026  
**Topik:** Analisis dan Pemodelan Prediksi Kelulusan Mahasiswa  

---

## 🧾 Deskripsi Dataset
Dataset yang digunakan: `student_graduation_data.csv`

| Kolom | Deskripsi |
|--------|------------|
| NIM | Nomor Induk Mahasiswa |
| ANGKATAN | Tahun Angkatan |
| SEMESTER | Kode Semester |
| KODE_MK | Kode Mata Kuliah |
| NAMA_MK | Nama Mata Kuliah |
| SKS | Jumlah Kredit (Satuan Kredit Semester) |
| NILAI | Nilai numerik (0–100) |
| GRADE | Nilai huruf (A, B, C, D, E) |

📊 **Jumlah Data:** 30.870 baris  
📈 **Jumlah Kolom:** 8 fitur  

---

## ⚙️ Alur Pengerjaan

### 1️⃣ Import dan Pembersihan Data
- Dataset dibaca menggunakan `pandas.read_csv()` dengan parameter `sep=';'` karena file menggunakan pemisah titik koma.  
- Ditemukan nilai kosong (missing value) pada kolom `NAMA_MK`, `NILAI`, dan `GRADE`.  
- Nilai kosong pada kolom `GRADE` diisi menggunakan fungsi pemetaan berdasarkan nilai `NILAI`, dengan konversi:

A (≥85), A- (≥80), B+ (≥75), B (≥70), B- (≥65), C+ (≥60), C (≥55), D (≥45), E (<45)

📉 Setelah proses pembersihan:
- Kolom `GRADE` bebas dari nilai kosong.
- Data siap untuk dianalisis lebih lanjut.

---

### 2️⃣ Analisis Deskriptif
Dilakukan analisis rata-rata nilai per mata kuliah untuk mengetahui mata kuliah dengan performa tertinggi dan terendah.

| Kategori | Nama Mata Kuliah | Rata-rata Nilai |
|-----------|------------------|-----------------|
| Tertinggi | SK632 – Jaringan Komputer Terapan 2 | **98.00** |
| Terendah | Algoritma dan Struktur Data | **45.82** |

📊 Visualisasi tambahan dibuat menggunakan grafik batang (*barplot*) untuk menampilkan 10 mata kuliah dengan nilai rata-rata tertinggi.

---

### 3️⃣ Pemodelan Machine Learning

#### 🎯 Tujuan
Membangun model klasifikasi untuk menentukan apakah seorang mahasiswa **lulus (Pass)** atau **tidak lulus (Fail)** berdasarkan data akademik.

#### 🧩 Tahapan
1. **Persiapan Data**
   - Menghapus baris dengan nilai kosong pada kolom `NILAI`.
   - Mengelompokkan data berdasarkan `NIM`.
   - Menjumlahkan total `SKS` dan menggabungkan daftar `GRADE` tiap mahasiswa.

2. **Pembuatan Kolom LABEL**
   - Mahasiswa dinyatakan **Pass** apabila:
     - Total SKS ≥ 144  
     - Tidak memiliki nilai D, E, atau F  
   - Selain itu dikategorikan sebagai **Fail**.

3. **Encoding**
   - Label dikonversi menjadi numerik:
     - Fail → 0  
     - Pass → 1

---

### 🧮 Model 1 – Menggunakan Fitur SKS Saja
Model pertama menggunakan satu fitur:
- **Fitur (X):** SKS  
- **Target (y):** LABEL  
- **Algoritma:** Decision Tree Classifier (`criterion='entropy'`, `max_depth=5`)

#### 🔍 Hasil Evaluasi
| Metrik | Nilai |
|---------|--------|
| Akurasi | 75.32% |
| Precision (Fail) | 0.96 |
| Recall (Fail) | 0.71 |
| Precision (Pass) | 0.49 |
| Recall (Pass) | 0.91 |

📈 **Interpretasi:**
Model cenderung hanya mengandalkan jumlah SKS sehingga mampu mengenali mahasiswa yang tidak lulus dengan baik, namun kurang akurat dalam memprediksi mahasiswa yang lulus.  
Struktur pohon keputusan menunjukkan batas pemisahan utama di **SKS ≤ 143.5**.

---

### 🧠 Model 2 – Penambahan Fitur Baru
Untuk meningkatkan akurasi, ditambahkan fitur baru:

| Fitur | Deskripsi |
|--------|------------|
| `Jml_Nilai_Fail` | Jumlah nilai tidak lulus (D, E, F) per mahasiswa |

**Fitur yang digunakan:** `SKS` dan `Jml_Nilai_Fail`  
**Algoritma:** Decision Tree Classifier dengan parameter yang sama.

#### 🔬 Hasil Evaluasi
| Metrik | Nilai |
|---------|--------|
| Akurasi | **100%** |
| Precision | 1.00 |
| Recall | 1.00 |
| F1-score | 1.00 |

📊 Model berhasil mencapai akurasi sempurna karena fitur yang digunakan sesuai dengan **aturan logis kelulusan mahasiswa**, yaitu jumlah SKS dan ada/tidaknya nilai D, E, F.  
Decision Tree dengan mudah menemukan pola tersebut secara eksplisit.

---

### 📈 Perbandingan Dua Model

| Model | Fitur | Akurasi | Keterangan |
|--------|--------|----------|-------------|
| Model 1 | SKS | 75.3% | Kurang akurat, fitur terbatas |
| Model 2 | SKS + Jml_Nilai_Fail | **100%** | Sangat akurat, fitur relevan dengan aturan kelulusan |

✅ **Kesimpulan sementara:**  
Model kedua memberikan hasil sempurna karena memanfaatkan fitur yang benar-benar mencerminkan kriteria kelulusan secara akademik.

---

## 📊 Visualisasi yang Ditampilkan
Notebook ini menampilkan beberapa visualisasi berikut:
- Heatmap posisi nilai kosong  
- Grafik batang 10 mata kuliah dengan nilai rata-rata tertinggi  
- Distribusi label Pass vs Fail  
- Visualisasi pohon keputusan  
- Confusion Matrix model 1 dan model 2  
- Grafik perbandingan akurasi antar model  

---

## 🧩 Teknologi yang Digunakan
| Kategori | Teknologi / Library |
|-----------|--------------------|
| Bahasa Pemrograman | Python 3.10+ |
| Analisis Data | Pandas, NumPy |
| Visualisasi | Matplotlib, Seaborn |
| Machine Learning | Scikit-learn |
| Lingkungan | Jupyter Notebook |

---

## 🧾 Kesimpulan
1. **Data preprocessing** berhasil menangani nilai kosong dan memastikan dataset siap digunakan.  
2. **Analisis deskriptif** mengidentifikasi variasi performa antar mata kuliah.  
3. **Model klasifikasi Decision Tree** berhasil memprediksi kelulusan mahasiswa dengan sangat baik, terutama setelah penambahan fitur `Jml_Nilai_Fail`.  
4. Hasil akhir menunjukkan bahwa kombinasi fitur yang tepat dapat merepresentasikan logika akademik secara sempurna.  
5. Akurasi model terbaik mencapai **100%**, menunjukkan kesesuaian penuh antara model dan aturan kelulusan.

---

## 🧑‍🏫 Ucapan Terima Kasih
Proyek ini dikembangkan sebagai bagian dari **Ujian Tengah Semester (UTS)**  
mata kuliah **IS411 – Data Modelling**,  
Program Studi Informatika, Fakultas Teknik dan Informatika, 
**Universitas Multimedia Nusantara (UMN)**.  

Seluruh analisis, kode, dan hasil merupakan karya asli mahasiswa:  
**Farrelius Kevin (00000081783)**  

---
