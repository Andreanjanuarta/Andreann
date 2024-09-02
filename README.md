# Import Library yang Diperlukan
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Mengatur agar grafik ditampilkan di notebook
%matplotlib inline

# Mengatur Style untuk Seaborn
sns.set(style="whitegrid")

# Membaca Dataset
file_path = r'C:\Users\User\Downloads\archive.zip'
df = pd.read_csv(file_path)

# 1. Eksplorasi Awal
print("===== 5 Baris Pertama =====")
display(df.head())

print("\n===== 5 Baris Terakhir =====")
display(df.tail())

print("\n===== Informasi Dataset =====")
df.info()

print("\n===== Statistik Deskriptif (Kolom Numerik) =====")
display(df.describe())

print("\n===== Jumlah Nilai Missing =====")
display(df.isnull().sum())

# 2. Identifikasi Outliers
print("\n===== Boxplot untuk Identifikasi Outliers =====")
numeric_columns = df.select_dtypes(include=[np.number]).columns
for column in numeric_columns:
    plt.figure(figsize=(8, 4))
    sns.boxplot(x=df[column])
    plt.title(f'Boxplot of {column}')
    plt.show()

# 3. Statistik Deskriptif Tambahan
print("\n===== Skewness dan Kurtosis =====")
skewness = df[numeric_columns].skew()
kurtosis = df[numeric_columns].kurtosis()

print("\nSkewness:")
display(skewness)

print("\nKurtosis:")
display(kurtosis)

# 4. Korelasi antar Variabel Numerik
print("\n===== Heatmap Korelasi =====")
plt.figure(figsize=(12, 10))
correlation_matrix = df[numeric_columns].corr()
sns.heatmap(correlation_matrix, annot=True, cmap="coolwarm", fmt=".2f")
plt.title('Correlation Heatmap')
plt.xticks(rotation=45)
plt.yticks(rotation=0)
plt.show()

# 5. Visualisasi Data

## 5.1. Histogram
print("\n===== Histogram untuk Setiap Variabel Numerik =====")
df[numeric_columns].hist(bins=15, figsize=(15, 10), layout=(len(numeric_columns)//3 + 1, 3))
plt.tight_layout()
plt.show()

## 5.2. Boxplot
print("\n===== Boxplot untuk Setiap Variabel Numerik =====")
for column in numeric_columns:
    plt.figure(figsize=(8, 4))
    sns.boxplot(y=df[column])
    plt.title(f'Boxplot of {column}')
    plt.show()

## 5.3. Scatter Plot untuk Pasangan Variabel dengan Korelasi Tinggi
print("\n===== Scatter Plot untuk Pasangan Variabel dengan Korelasi Tinggi =====")
# Menentukan threshold korelasi tinggi (misalnya > 0.7 atau < -0.7)
high_corr = correlation_matrix.abs().unstack().sort_values(ascending=False)
high_corr = high_corr[high_corr < 1]  # Menghapus korelasi diri sendiri
high_corr = high_corr[high_corr > 0.7].drop_duplicates()

for (column1, column2) in high_corr.index:
    plt.figure(figsize=(8, 6))
    sns.scatterplot(data=df, x=column1, y=column2, hue='HeartDisease', palette='viridis')
    plt.title(f'Scatter Plot of {column1} vs {column2}')
    plt.show()

LAPORAN ANALISIS DATA HEART DISEASE UCI

PENGANTAR
Dataset Heart Disease UCI berisi informasi medis dari berbagai pasien dan digunakan untuk
memprediksi kemungkinan seseorang menderita penyakit jantung. Data ini memiliki beberapa
fitur seperti usia, jenis kelamin, tekanan darah, kadar kolesterol, dan beberapa hasil tes medis
lainnya

EKSPLORASI AWAL DATA
Dataset terdiri dari beberapa kolom numerik dan kategorikal. Setelah memuat data, langkah
pertama adalah menampilkan beberapa baris pertama dan terakhir untuk memahami
strukturnya:
Informasi tentang dataset menunjukkan jumlah entri dan kolom serta tipe data dari masingmasing kolom. Ditemukan bahwa dataset tidak memiliki nilai yang hilang di sebagian besar kolom,
namun beberapa kolom mungkin memiliki nilai yang tidak terduga.

STATISTIK DESKRIPTIF
Statistik deskriptif memberikan wawasan tentang distribusi data pada kolom numerik:
• Mean, Median, Mode, Variance, dan Standar Deviasi: Dihitung untuk setiap variabel
numerik untuk memberikan informasi tentang pusat distribusi dan penyebarannya.
• Skewness dan Kurtosis: Mengukur kemiringan dan puncak distribusi data. Beberapa
variabel menunjukkan distribusi yang tidak normal, dengan skewness yang tinggi
menunjukkan distribusi miring.
• Korelasi Antar Variabel: Korelasi menunjukkan hubungan linier antara variabel. Variabel
seperti chol (kolesterol) dan age memiliki korelasi yang cukup tinggi dengan risiko penyakit
jantung.

VISUALISASI DATA
Berbagai visualisasi digunakan untuk menggambarkan distribusi dan hubungan antar variabel:
1. Histogram: Menampilkan distribusi nilai untuk setiap variabel numerik. Beberapa
histogram menunjukkan distribusi yang tidak normal.
2. Box Plot: Mengidentifikasi adanya outliers pada variabel numerik. Beberapa variabel
seperti chol dan thalach menunjukkan outliers yang signifikan.
3. Scatter Plot: Menunjukkan hubungan antar variabel dengan korelasi tinggi. Misalnya,
terdapat korelasi yang kuat antara age dan risiko penyakit jantung.

KESIMPULAN
• Distribusi Data: Beberapa variabel menunjukkan distribusi yang tidak normal, dengan
skewness yang tinggi dan kurtosis yang menonjol.
• Korelasi: Terdapat korelasi yang signifikan antara beberapa variabel, seperti age dan chol,
yang menunjukkan hubungan yang kuat dengan risiko penyakit jantung.
• Outliers: Outliers ditemukan pada beberapa variabel numerik, yang mungkin
mempengaruhi hasil analisis statistik.
• Nilai yang Hilang: Tidak ada nilai yang hilang dalam dataset, sehingga analisis dapat
dilakukan tanpa memerlukan imputasi nilai.


