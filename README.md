# Praktikum-2-Komnum

# **Praktikum Komputasi Numerik _(Numerical Computation Lab Work)_**
- **Anggkota Kelompok 22 :**

|    NRP     |      Name      |
| :--------: | :------------: |
| 5025241201 | Nasyita Larashati Ertyanada |
| 5025241222 | Rahma Sakinah |

### Laporan Resmi Praktikum Komputasi Numerik _(Numerical Computation Lab Work Report)_

Laporan ini disusun sebagai bagian dari pelaksanaan Praktikum Komputasi Numerik (Numerical Computation Lab Work) pada semester genap. Tujuan dari praktikum ini adalah untuk memahami dan menerapkan metode numerik yang telah dipelajari dalam perkuliahan ke dalam bentuk implementasi program komputer.

----

## **Praktikum 2**

Salah satu kelemahan dari metode Trapezoidal adalah kita harus menggunakan jumlah interval yang besar untuk memperoleh akurasi yang diharapkan. Buatlah sebuah program komputer untuk menjelaskan bagaimana metode Integrasi Romberg dapat mengatasi kelemahan tersebut.

#### **Answer:**

- **Code:**

```python
import numpy as np

# Fungsi yang ingin diintegrasi
def fungsi(x):
    return np.sin(x)  # Ganti sesuai kebutuhan

# Implementasi metode Romberg
def integrasi_romberg(fungsi, batas_awal, batas_akhir, maks_iterasi=5):
    tabel = np.zeros((maks_iterasi, maks_iterasi))
    h = batas_akhir - batas_awal

    # Trapezoidal awal (iterasi pertama)
    tabel[0][0] = 0.5 * h * (fungsi(batas_awal) + fungsi(batas_akhir))
    print(f"R[0][0] = {tabel[0][0]:.10f}")

    for i in range(1, maks_iterasi):
        h /= 2 # step-size dibagi 2 setiap iterasi

        jumlah = 0
        for k in range(1, 2**(i - 1) + 1):
            titik = batas_awal + (2 * k - 1) * h
            jumlah += fungsi(titik)

        # Trapezoidal tahap berikutnya (Mengisi tabel[i][0])
        tabel[i][0] = 0.5 * tabel[i - 1][0] + h * jumlah
        print(f"R[{i}][0] = {tabel[i][0]:.10f}")

        # Richardson Extrapolation:
        # meningkatkan akurasi tanpa tambah titik evaluasi
        for j in range(1, i + 1):
            atas = 4 * j * tabel[i][j - 1] - tabel[i - 1][j - 1]
            bawah = 4 * j - 1
            tabel[i][j] = atas / bawah
            print(f"R[{i}][{j}] = {tabel[i][j]:.10f}")

    # Cetak hasil akhir tabel Romberg (menurun ke bawah)
    print("\nTabel Romberg:")
    for i in range(maks_iterasi):
        for j in range(i + 1):
            print(f"{tabel[i][j]:.10f}", end=" ")
        print()

    return tabel[maks_iterasi - 1][maks_iterasi - 1]

# Contoh penggunaan
awal = 0
akhir = np.pi
hasil = integrasi_romberg(fungsi, awal, akhir, maks_iterasi=5)

print(f"\nEstimasi nilai integral = {hasil:.10f}")
```

- **Explanation:**

```python
def fungsi(x):
    return np.sin(x)
```
Fungsi ini adalah f(x) yang akan diintegralkan. Bisa diganti sesuai kebutuhan. Dalam pengerjaan ini kami menggunakan sin(x).

```python
tabel = np.zeros((maks_iterasi, maks_iterasi))
```
matriks kosong untuk menyimpan nilai hasil integrasi dalam iterasi.

```python
tabel[0][0] = 0.5 * h * (fungsi(batas_awal) + fungsi(batas_akhir))
```
Rumus metode trapezoid untuk menghitung estimasi awal integral.

```python
    for i in range(1, maks_iterasi):
        h /= 2 # step-size dibagi 2 setiap iterasi

        jumlah = 0
        for k in range(1, 2**(i - 1) + 1):
            titik = batas_awal + (2 * k - 1) * h
            jumlah += fungsi(titik)

        # Trapezoidal tahap berikutnya (Mengisi tabel[i][0])
        tabel[i][0] = 0.5 * tabel[i - 1][0] + h * jumlah
        print(f"R[{i}][0] = {tabel[i][0]:.10f}")
```
Setiap kali nilai h dibagi dua, kami menambahkan titik-titik tengah baru yang sebelumnya belum dihitung. Dengan cara ini, hasil integrasi menjadi semakin akurat dan mendekati nilai sebenarnya, tanpa perlu menambah terlalu banyak titik evaluasi secara langsung.

```python
        for j in range(1, i + 1):
            atas = 4 * j * tabel[i][j - 1] - tabel[i - 1][j - 1]
            bawah = 4 * j - 1
            tabel[i][j] = atas / bawah
            print(f"R[{i}][{j}] = {tabel[i][j]:.10f}")
```
Program ini menjelaskan bahwa Integrasi Romberg mampu mengatasi kelemahan metode Trapezoidal yang membutuhkan banyak interval untuk akurat. Dengan menggunakan Richardson Extrapolation, hasil pendekatan diperbaiki secara bertahap dari nilai sebelumnya, sehingga akurasi meningkat lebih cepat tanpa menambah terlalu banyak titik perhitungan. Ini membuat proses integrasi lebih efisien dan tetap presisi.

```python
    # Cetak hasil akhir tabel Romberg (menurun ke bawah)
    print("\nTabel Romberg:")
    for i in range(maks_iterasi):
        for j in range(i + 1):
            print(f"{tabel[i][j]:.10f}", end=" ")
        print()
```
Bagian ini mencetak isi tabel Romberg dari baris ke baris dengan format mendatar. Tujuannya supaya lebih mudah melihat perkembangan nilai hasil integral di setiap iterasi dan bagaimana nilainya semakin mendekati hasil yang diharapkan.

```python
return tabel[maks_iterasi - 1][maks_iterasi - 1]
```
Bagian ini mengembalikan hasil akhir integrasi, yaitu nilai di pojok kanan bawah tabel Romberg, yang merupakan estimasi dengan tingkat akurasi paling tinggi dari seluruh iterasi yang sudah dilakukan.

```python
awal = 0
akhir = np.pi
hasil = integrasi_romberg(fungsi, awal, akhir, maks_iterasi=5)

print(f"\nEstimasi nilai integral = {hasil:.10f}")
```
Bagian ini menjalankan fungsi integrasi_romberg dengan batas bawah 0 dan batas atas π, lalu mencetak hasil akhirnya. Nilai integral yang ditampilkan merupakan estimasi terbaik berdasarkan metode Romberg dengan 5 iterasi.

- **Screenshot:**
![output](https://i.imgur.com/p7Nx7wO.png)
 menunjukkan tabel hasil integrasi Romberg dalam format segitiga. Setiap baris merepresentasikan iterasi, dan nilai-nilai dalam baris tersebut menunjukkan hasil pendekatan integral dengan akurasi yang semakin tinggi. Nilai di kanan bawah merupakan estimasi akhir yang paling akurat.

----
