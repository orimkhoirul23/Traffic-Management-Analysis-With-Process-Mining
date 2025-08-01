# Traffic-Management-Analysis-With-Process-Mining
# Penggunaan K-Means Clustering untuk Meningkatkan Hasil Process Mining pada Data Road Traffic Fine Management

## Daftar Isi
- [Pendahuluan](#pendahuluan)
- [Tujuan Proyek](#tujuan-proyek)
- [Dataset](#dataset)
- [Metodologi](#metodologi)
  - [Process Mining](#process-mining)
  - [Data Preprocessing](#data-preprocessing)
  - [K-Means Clustering](#k-means-clustering)
  - [Process Discovery dengan Inductive Miner](#process-discovery-dengan-inductive-miner)
  - [Evaluasi Model (Conformance Checking)](#evaluasi-model-conformance-checking)
- [Hasil dan Pembahasan](#hasil-dan-pembahasan)
- [Kesimpulan](#kesimpulan)
- [Saran](#saran)
- [Referensi](#referensi)

## Pendahuluan

Dalam pengelolaan proses bisnis modern, memahami alur proses yang kompleks sangat krusial untuk meningkatkan efisiensi dan kualitas layanan [1]. Kompleksitas ini seringkali dapat menyebabkan masalah seperti keterlambatan, pekerjaan ulang, dan pemborosan sumber daya [2]. **Process Mining** adalah teknik analisis data yang memanfaatkan *event log* dari sistem informasi untuk **menemukan, memverifikasi, dan meningkatkan model proses aktual** [2, 3]. Teknik ini memungkinkan organisasi mengidentifikasi pola perilaku, variasi jalur proses, serta mengevaluasi kesesuaian antara proses yang dijalankan dengan model yang diharapkan [3].

Namun, salah satu tantangan besar dalam process mining adalah ketika *event log* sangat beragam dan mencerminkan banyak variasi proses, yang dapat menghasilkan model proses yang terlalu kompleks atau dikenal sebagai **"spaghetti model"** [3]. Untuk mengatasi ini, **teknik clustering** dapat diterapkan untuk mengelompokkan kasus-kasus serupa, membuat analisis process mining lebih terfokus dan representatif [4]. **Algoritma K-Means** merupakan metode clustering yang banyak digunakan untuk segmentasi data proses, dan telah terbukti dapat menyederhanakan model awal serta meningkatkan nilai fitness dalam evaluasi konformansi [4, 5].

Proyek ini berfokus pada penerapan K-Means clustering sebelum tahap *process discovery* dengan algoritma Inductive Miner pada dataset **Road Traffic Fine Management**, untuk mengukur dampaknya terhadap metrik evaluasi model proses seperti *fitness, precision, generalization*, dan *simplicity* [6].

## Tujuan Proyek

Proyek ini memiliki dua tujuan utama [6]:
1.  **Menganalisis efektivitas K-Means dalam meningkatkan hasil dari proses model.**
2.  **Mengevaluasi hasil evaluasi model process mining (fitness, precision, generalization, simplicity) pasca-clustering.**

## Dataset

Dataset yang digunakan dalam penelitian ini adalah **Road Traffic Fine Management Process**. Ini adalah kumpulan *event log* nyata yang berasal dari sistem informasi manajemen denda pelanggaran lalu lintas di Italia [7]. Dataset ini dipublikasikan oleh Eindhoven University of Technology melalui repositori 4TU.ResearchData dan telah banyak digunakan dalam penelitian proses bisnis [7].

*   **Isi Data**: Merekam alur proses penanganan denda lalu lintas, dari denda dikeluarkan hingga penyelesaiannya, termasuk aktivitas seperti pengiriman surat pemberitahuan, pembayaran, atau eskalasi ke pengadilan [8].
*   **Periode Data**: 1 Januari 2000 hingga 18 Juni 2013 [8].
*   **Jumlah Kasus**: Sekitar 150.370 kasus, di mana setiap kasus mewakili satu proses penanganan denda [8].

## Metodologi

Metodologi proyek ini melibatkan beberapa tahapan utama:

### Process Mining

Process mining secara umum mencakup tiga aktivitas utama [3]:
*   **Process Discovery**: Menemukan model proses dari *event log*.
*   **Conformance Checking**: Memverifikasi kesesuaian antara *event log* dan model proses.
*   **Process Enhancement**: Meningkatkan model proses berdasarkan analisis.

### Data Preprocessing

Tahap *data preprocessing* dilakukan untuk mengoptimalkan hasil clustering [9]. Atribut-atribut relevan untuk process mining (activity, resource, dan timestamp) difilter [8]. Kemudian, beberapa atribut baru dibuat, seperti:
*   **Duration**: Durasi dari suatu *trace* [9].
*   **Activity Count**: Jumlah aktivitas dari suatu *trace* [9].

Secara total, 8 atribut yang digunakan untuk clustering adalah: `amount`, `resource`, `dismissal`, `concept:name`, `vehicleClass`, `points`, `duration`, dan `activityCount` [10].

Selanjutnya, dilakukan *encoding* untuk data kategorikal dan *standarisasi* untuk data numerikal. Analisis **PCA (Principal Component Analysis)** dilakukan untuk menentukan atribut yang paling berpengaruh, dengan memilih fitur yang memiliki PC1 Weight > 0.2 [10].

### K-Means Clustering

Algoritma K-Means digunakan untuk mengelompokkan data [9]. Untuk menentukan nilai `k` terbaik (jumlah *cluster*), **metode Elbow dan Silhouette Score** digunakan [11]. Berdasarkan metode Elbow, **nilai k terbaik yang didapatkan adalah 5** [11].

Meskipun K-Means dapat memisahkan data dengan baik di beberapa bagian, terdapat beberapa data *noise* yang tidak dapat ditangani dengan optimal [11].

### Process Discovery dengan Inductive Miner

Setelah clustering, model proses dibuat menggunakan algoritma **Inductive Miner** dan ditampilkan dalam format Petri Net [12, 13]. Algoritma ini dipilih berdasarkan penelitian sebelumnya yang menunjukkan skor *fitness* yang lebih tinggi dibandingkan Alpha Miner pada dataset pengaduan pelanggan [14].

### Evaluasi Model (Conformance Checking)

Analisis konformansi adalah tahapan akhir penelitian yang mengevaluasi kesesuaian dan perbedaan antara peristiwa dalam *event log* dengan aktivitas dalam model proses [13]. Evaluasi dilakukan berdasarkan empat dimensi kualitas [13, 15]:

1.  **Fitness**: Mengukur sejauh mana perilaku dalam *event log* dapat direpresentasikan oleh *process model*. Nilai 1 menunjukkan bahwa model proses dapat sepenuhnya mereproduksi setiap *trace* dalam *event log* [16].
2.  **Precision**: Mengukur sejauh mana model dapat merepresentasikan perilaku proses yang dijelaskan dalam *event log* tanpa memberikan kemungkinan perilaku yang terlalu luas (*oversimplification*) [16].
3.  **Generalization**: Mengukur kemampuan model dalam menangani variasi yang mungkin tidak muncul dalam *event log*. Nilai mendekati 1 menunjukkan model mampu menangkap semua kemungkinan variasi [17].
4.  **Simplicity**: Mengukur seberapa mudah model dapat dipahami oleh manusia, berkaitan langsung dengan kompleksitas model. Nilai 1 menunjukkan model sederhana [9, 17].

## Hasil dan Pembahasan

Penerapan K-Means clustering sebelum *process discovery* menunjukkan dampak positif pada performa model, khususnya dalam konteks data yang kompleks dan heterogen [12].

Perbandingan hasil *conformance checking* **sebelum dan sesudah clustering** menunjukkan perubahan signifikan pada metrik evaluasi [18]:

*   **Fitness**: Nilai *fitness* **tetap** [18].
*   **Precision**: Terdapat **kenaikan signifikan sebesar 0.137**, dari 0.582 menjadi 0.719 [18]. Ini menunjukkan model menjadi lebih tepat dalam merepresentasikan perilaku proses yang sebenarnya ada dalam *event log*.
*   **Simplicity**: Terdapat **kenaikan sebesar 0.061**, dari 0.623 menjadi 0.684 [18]. Ini mengindikasikan bahwa model yang dihasilkan menjadi sedikit lebih sederhana dan mudah dipahami.
*   **Generalization**: Terdapat **penurunan sebesar 0.086**, dari 0.975 menjadi 0.889 [18]. Penurunan ini menunjukkan bahwa model yang dihasilkan menjadi lebih spesifik karena clustering membantu memisahkan jalur proses yang beragam, sehingga mengurangi kemampuan model untuk menggeneralisasi variasi yang tidak sering muncul [19, 20].

Karakteristik log untuk masing-masing dari 5 cluster yang dihasilkan dapat dilihat di dalam laporan, yang memisahkan *trace* berdasarkan aktivitas awal, aktivitas terakhir, jumlah *trace*, dan rata-rata durasi [21, 22]. Model proses terpisah untuk setiap cluster juga dihasilkan, membantu analisis yang lebih terfokus [22].

## Kesimpulan

Berdasarkan hasil penelitian, dapat disimpulkan bahwa **clustering menggunakan K-Means berhasil meningkatkan performa model proses, terutama dalam hal *precision*** [20]. Meskipun terdapat penurunan pada nilai *generalization*, hal ini menunjukkan bahwa model proses yang didapatkan menjadi lebih spesifik dan akurat dalam menangani variasi proses yang sering terjadi [20]. Integrasi teknik clustering sebelum tahap *process discovery* memberikan dampak positif, khususnya untuk data yang kompleks dan heterogen [12].

## Saran

Untuk penelitian selanjutnya, disarankan untuk **menjelajahi fitur-fitur baru yang bisa dibuat atau digunakan** dalam tahap *preprocessing* untuk potensi peningkatan hasil clustering dan model proses [20].

## Referensi

Daftar Pustaka lengkap dapat ditemukan di bagian `DAFTAR PUSTAKA` dalam dokumen asli [15, 20, 23-26].

---
