# 🧬 Genetic Algorithm — Function Minimization

> **Case Based – Searching | Kecerdasan Buatan**
> Semester Genap 2025/2026 · S1 Rekayasa Perangkat Lunak · Telkom University

---

## 📌 Deskripsi

Repository ini berisi implementasi **Algoritma Genetika (Genetic Algorithm / GA)** dari nol menggunakan Python murni — tanpa library GA — untuk menyelesaikan masalah **minimisasi fungsi matematis dua variabel**:

$$f(x_1, x_2) = -\left( \sin(x_1)\cos(x_2)\tan(x_1 + x_2) + \frac{1}{2}e^{1 - \sqrt{x_2^2}} \right)$$

dengan domain:

$$-10 \leq x_1 \leq 10 \quad \text{dan} \quad -10 \leq x_2 \leq 10$$

---

## 🗂️ Struktur Repository

```
📁 repository
├── 📓 genetic_algorithm.ipynb   # Source code utama (Google Colab)
└── 📄 README.md
```

---

## ⚙️ Desain Algoritma Genetika

### Representasi Kromosom
Setiap kromosom direpresentasikan sebagai **string biner 20 bit**:

```
[ b1  b2  b3  b4  b5  b6  b7  b8  b9  b10 | b11 b12 b13 b14 b15 b16 b17 b18 b19 b20 ]
  └──────────── gen x1 (10 bit) ───────────┘ └──────────── gen x2 (10 bit) ───────────┘
```

Dekode dari biner ke nilai real menggunakan pemetaan linear:

$$x = -10 + 20 \times \frac{\text{desimal}}{1023}$$

### Parameter GA

| Parameter | Nilai | Keterangan |
|-----------|-------|------------|
| Ukuran Populasi | `50` | Jumlah kromosom per generasi |
| Panjang Kromosom | `20 bit` | 10 bit per variabel |
| Probabilitas Crossover (Pc) | `0.8` | Single-point crossover |
| Probabilitas Mutasi (Pm) | `0.01` | Bit-flip mutation per bit |
| Jumlah Generasi | `100` | Kriteria penghentian |
| Seleksi Orangtua | `Roulette Wheel` | Berbasis probabilitas fitness |
| Pergantian Generasi | `Generational Replacement` | Seluruh populasi diganti |

---

## 🔄 Alur Algoritma (5 Tahap)

```
START
  │
  ▼
[Tahap 1] Inisialisasi populasi acak
          → Dekode kromosom (biner → x1, x2)
          → Hitung f(x1, x2)
          → Transformasi fitness = 1 / (1 + (f - f_min))
          → Hitung P[i], Kumulatif C, dan Interval
  │
  ▼
[Tahap 2] Seleksi Roulette Wheel → pilih 2 orangtua
  │
  ▼
[Tahap 3] Single-point Crossover → hasilkan 2 anak
  │
  ▼
[Tahap 4] Bit-flip Mutation → modifikasi anak
  │
  ▼
[Tahap 5] Populasi baru terbentuk
  │
  └──► Ulangi Tahap 2-5 hingga 100 generasi
  │
  ▼
Output: kromosom terbaik, nilai x1 & x2, nilai f minimum
```

### Catatan Penting: Adaptasi untuk Minimisasi

Karena tujuannya **minimisasi** dan nilai `f` bisa **negatif**, fitness tidak bisa langsung dipakai untuk Roulette Wheel. Kami menggunakan transformasi:

```
fitness(i) = 1 / (1 + (f(i) - f_min_populasi))
```

Ini menjamin:
- ✅ Semua fitness selalu **positif** → aman untuk Roulette Wheel
- ✅ Kromosom dengan `f` terkecil mendapat **fitness tertinggi** (= 1)

---

## 🗂️ Struktur Cell di Notebook

| Cell | Fungsi | Tahap GA |
|------|--------|----------|
| Cell 1 | Import & Parameter | Setup |
| Cell 2 | `initialize_population()` | Tahap 1a |
| Cell 3 | `decode_chromosome()` | Tahap 1b |
| Cell 4 | `objective_function()` + `calculate_fitness()` | Tahap 1b & 1c |
| Cell 5 | `compute_probabilities()` | Tahap 1d |
| Cell 6 | `roulette_wheel_selection()` | Tahap 2 |
| Cell 7 | `crossover()` | Tahap 3 |
| Cell 8 | `mutate()` | Tahap 4 |
| Cell 9 | `create_next_generation()` | Tahap 5 |
| Cell 10 | `run_genetic_algorithm()` | Loop Utama |
| Cell 11 | Output Hasil | — |

---

## 🚀 Cara Menjalankan

### Di Google Colab
1. Buka file `genetic_algorithm.ipynb` di [Google Colab](https://colab.research.google.com/)
2. Jalankan semua cell secara berurutan (`Runtime → Run all`)
3. Lihat output di Cell 11

### Di Lokal (Jupyter Notebook)
```bash
# Clone repository
git clone https://github.com/[username]/[repo-name].git
cd [repo-name]

# Install Jupyter (jika belum ada)
pip install notebook

# Jalankan notebook
jupyter notebook genetic_algorithm.ipynb
```

> **Catatan:** Program hanya menggunakan modul bawaan Python (`random` dan `math`). Tidak ada dependensi tambahan yang perlu diinstall.

---

## 📤 Contoh Output

```
======================================================================
MULAI EVOLUSI GENETIC ALGORITHM
======================================================================
Generasi   0 | f terbaik =  -3.141592 | x1 =  1.5708 | x2 =  0.0000
Generasi  10 | f terbaik =  -4.203817 | x1 =  2.3140 | x2 = -0.5231
Generasi  20 | f terbaik =  -5.817342 | x1 =  3.1024 | x2 = -0.1045
...
Generasi  99 | f terbaik =  -7.234561 | x1 =  4.7124 | x2 =  0.0098
======================================================================
EVOLUSI SELESAI
======================================================================

======================================================================
==================== HASIL AKHIR GENETIC ALGORITHM ===================
======================================================================
Kromosom terbaik (20 bit) : 10110100011000000001
  - Gen x1 (10 bit)       : 1011010001
  - Gen x2 (10 bit)       : 1000000001
Nilai x1                  :  4.712400
Nilai x2                  :  0.009775
Nilai f(x1, x2) minimum   : -7.234561
======================================================================
```

*(Output aktual dapat berbeda tergantung random seed yang digunakan)*

---

## 📚 Referensi

- Holland, J.H. (1975). *Adaptation in Natural and Artificial Systems*. University of Michigan Press.
- De Jong, K.A. (1975). *Analysis of the Behavior of a Class of Genetic Adaptive Systems*. Doctoral dissertation, University of Michigan.
- Grefenstette, J.J. (1986). Optimization of Control Parameters for Genetic Algorithms. *IEEE Transactions on Systems, Man, and Cybernetics*, 16(1), 122–128.

---

## 👥 Tim Pengembang

| Nama | NIM |
|------|-----|
| Nadia Tambunan | 103122400005 |
| Najwa Areefa Ghaisani | 103122400028 |

> 🏆 **Presentasi Terbaik** — Case Based Searching, Kecerdasan Buatan 2025/2026

---

<div align="center">
  <sub>Dibuat dengan ❤️ untuk mata kuliah yang diampu oleh ibu dosen tercingtah - Bu Gita - Kecerdasan Buatan · Telkom University Purwokerto</sub>
</div>
