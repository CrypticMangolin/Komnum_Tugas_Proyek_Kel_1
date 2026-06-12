<a href="https://colab.research.google.com/github/CrypticMangolin/super-duper-engine/blob/main/Komnum_Tugas_Proyek.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

# Proyek Komputasi Numerik: Integrasi Trapesium Majemuk (Composite Trapezoidal Rule)

Repositori ini berisi implementasi **Metode Trapesium Majemuk (Composite Trapezoidal Rule)** berbasis Python yang dijalankan menggunakan Jupyter Notebook / Google Colab. Proyek ini memadukan komputasi numerik menggunakan `NumPy` dengan komputasi simbolik matematika menggunakan `SymPy` untuk menghitung pendekatan nilai integral tentu, membandingkannya dengan nilai eksak, serta menghitung galat (error) secara dinamis.

## 👥 Anggota Kelompok
Berikut adalah daftar anggota kelompok yang berkontribusi dalam proyek ini:

* **HIDAYATULLAH** (5025201031)
* **Abyan Zhafran Trisnanto** (5025211218)
* **Dzuhrillah Hendraines** (5025221107)
* **Ahmad Fadhilah Mappisara** (5025221195)

### Trapezoidal Rule Formula

General Formula:
$$L = \frac{h}{2} \left[ f(x_0) + 2 \sum_{i=1}^{n-1} f(x_i) + f(x_n) \right]$$

Segment Width:
$$h = \frac{b - a}{n}$$

Interior Points:
$$x_i = x_0 + i \cdot h$$

**Args:**
*   `f`: The function to integrate.
*   `a`: Lower bound (starting point, $x_0$).
*   `b`: Upper bound (end point, $x_n$).
*   `n`: Number of segments.

### Fitur Program
1. **Parsing Fungsi Fleksibel:** Mengubah input string matematika dari pengguna (seperti `x^2+5`, `sin(x)`, `e^x`) menjadi ekspresi matematika simbolik melalui pustaka `SymPy`.
2. **Perhitungan Integrasi Eksak:** Menghitung nilai analitis (antiturunan) sejati sebagai referensi pembanding nilai numerik.
3. **Simulasi Dinamis & Iterasi:** Menampilkan tabel hasil integrasi numerik ($L$), lebar segmen ($h$), dan persentase galat relatif sejati ($E_t$) secara berulang hingga tingkat kesalahan memenuhi batas toleransi yang ditentukan (misalnya $E \le 1\%$).

---

## 🚀 Cara Penggunaan

### 1. Menjalankan di Google Colab (Rekomendasi)
Anda dapat langsung menjalankan berkas `.ipynb` ini di lingkungan Google Colab:
1. Klik *badge* **"Open In Colab"** yang ada di bagian atas berkas README ini.
2. Pastikan runtime telah terhubung (Python 3).
3. Jalankan blok kode (cell) dari atas ke bawah secara berurutan dengan menekan tombol *Run* atau `Shift + Enter`.

```python
import numpy as np
def integrasi_trapesium(f, a, b, n):
    h = (b - a) / n
    x = np.linspace(a, b, n+1)     # equal to x = [a + i*h for i in range(n+1)]
    L = h * (0.5 * f(x[0]) + np.sum(f(x[1:n])) + 0.5 * f(x[n]))
    return L, h
```

# Simulasi Dinamis: Tabel Hasil Integrasi & Error

![Image](https://i.imgur.com/DjEOUAZ.png)


```python
SimulasiDinamis()
```


$\displaystyle \text{Indefinite Integral:}\quad\int f{\left(x \right)} dx=\frac{x^{3}}{3} + 5 x+C$



$\displaystyle \text{Exact Integral:}\quad\int\limits_{0}^{2} f{\left(x \right)} dx≈\frac{38}{3}≈12.67$


    
    Hasil integral menggunakan integrasi trapesium untuk



$\displaystyle f(x) = x^{2} + 5$


    Hasil integral menggunakan integral trapesium untuk dari a = 0 sampai b = 2 dapat dilihat pada tabel berikut:
    



| $n$ | $h$ | $L$ | $E \%$ |
|:---:|:---:|:---------------------:|:---------------------:|
| 1 | 2.00 | 14.00 | 10.53% |
| 2 | 1.00 | 13.00 | 2.63% |
| 4 | 0.50 | 12.75 | 0.66% |
| 8 | 0.25 | 12.69 | 0.16% |
| 16 | 0.12 | 12.67 | 0.04% |



    
    Error(0.04%) < 0.05%


# Penjelasan


```python
import numpy as np
def integrasi_trapesium(f, a, b, n):
    h = (b - a) / n
    x = np.linspace(a, b, n+1)     # equal to x = [a + i*h for i in range(n+1)]
    L = h * (0.5 * f(x[0]) + np.sum(f(x[1:n])) + 0.5 * f(x[n]))
    return L, h
```

Fungsi `integrasi_trapesium()` digunakan untuk menghitung pendekatan nilai integral tentu menggunakan metode trapesium majemuk (Composite Trapezoidal Rule). Pertama, program menghitung lebar setiap subinterval *h=(b−a)/n*. Selanjutnya, nilai fungsi pada seluruh titik interior dijumlahkan menggunakan perulangan. Nilai fungsi pada batas bawah dan batas atas diberi bobot 1/2 sesuai rumus metode trapesium. Hasil akhir diperoleh dengan mengalikan jumlah tersebut dengan lebar interval h. Implementasi ini merepresentasikan rumus:


```python
from IPython.display import display, Math
import sympy as sp
from sympy.parsing.sympy_parser import parse_expr, standard_transformations, convert_xor, implicit_multiplication_application

x = sp.symbols('x')

def parse_function(f_str):
    transformations = standard_transformations + (convert_xor, implicit_multiplication_application)
    local_dict = {'e': sp.E, 'π': sp.pi}
    f = parse_expr(f_str, transformations=transformations, local_dict=local_dict)
    return f

def show(*args):
    s = ""
    for arg in args:
        if isinstance(arg, str):
            s += arg
        else:
            s += sp.latex(arg)
    display(Math(s))
```

Blok program di atas digunakan untuk memproses input fungsi matematika yang diberikan pengguna dan menampilkannya dalam format matematis yang rapi. Fungsi `parse_function()` mengubah masukan berupa string menjadi ekspresi simbolik SymPy dengan mendukung notasi umum seperti `x^2`, `2x`, `e^x`, dan `π`. Sementara itu, `fungsi show()` digunakan untuk menampilkan ekspresi matematika dalam format LaTeX pada Google Colab sehingga rumus dan hasil perhitungan dapat dibaca dengan lebih jelas dan menyerupai penulisan matematika pada buku atau jurnal ilmiah.


```python
# Menerima input dari pengguna (kosongkan untuk menggunakan nilai default)
f_str_input = input("Masukkan fungsi f(x) [Default: x^2 + 5]: ").strip()
f_str = f_str_input if f_str_input else "x^2 + 5"

a_input = input("Masukkan batas bawah (a) [Default: 0]: ").strip()
a = float(a_input) if a_input else 0.0

b_input = input("Masukkan batas atas (b) [Default: 2]: ").strip()
b = float(b_input) if b_input else 2.0

f = parse_function(f_str)
show(f)
```


$\displaystyle 400 x^{5} - 900 x^{4} + 675 x^{3} - 200 x^{2} + 25 x + 0.2$


Pada tahap ini (di atas) ditentukan batas bawah dan batas atas integral, yaitu *a = 0* dan *b = 2*. Selanjutnya fungsi yang akan diintegralkan didefinisikan dalam bentuk string. Program menyediakan beberapa contoh fungsi seperti fungsi polinomial, rasional, trigonometri, dan eksponensial. Fungsi yang dipilih kemudian diproses menggunakan `parse_function()` untuk diubah menjadi ekspresi simbolik SymPy. Setelah itu, fungsi ditampilkan dalam format LaTeX menggunakan `show()` sehingga pengguna dapat memverifikasi bahwa fungsi yang akan digunakan dalam proses integrasi telah sesuai.


```python
F = sp.integrate(f, x)
show(sp.Integral(f, x),"=", F,"+C")
```


$\displaystyle \int \left(400 x^{5} - 900 x^{4} + 675 x^{3} - 200 x^{2} + 25 x + 0.2\right) dx=66.6666666666667 x^{6} - 180.0 x^{5} + 168.75 x^{4} - 66.6666666666667 x^{3} + 12.5 x^{2} + 0.2 x+C$


Blok program ini digunakan untuk menghitung integral tak tentu dari fungsi yang telah didefinisikan menggunakan kemampuan integrasi simbolik pada library SymPy. Fungsi `sp.integrate()` menghasilkan antiturunan *F(x)* dari fungsi *f(x)*. Selanjutnya, hasil integral ditampilkan dalam format LaTeX menggunakan fungsi `show()`, sehingga diperoleh bentuk matematis yang mudah dibaca. Meskipun metode trapesium merupakan metode numerik yang tidak memerlukan solusi analitik, hasil integral simbolik ini dapat digunakan sebagai referensi untuk memverifikasi hasil pendekatan numerik dan menghitung besar galat (error) metode yang digunakan.


```python
exact_integral = sp.integrate(f, (x, a, b))
exact_value = exact_integral.evalf(6) # Hasil True
try:
  display_str  = f"{float(exact_value):.2f}"
except:
  display_str = exact_value
f_x_str = sp.Integral(sp.Function('f')(x), (x, a, b)) # f(x)
show(f_x_str,"=", exact_integral, "=", display_str)
```


$\displaystyle \int\limits_{0.0}^{0.8} f{\left(x \right)} dx=1.64053333333333=1.64$



```python
from IPython.display import Markdown

# Menerima input untuk nilai n (bisa satu atau banyak yang dipisahkan koma)
n_input = input("Masukkan nilai n (pisahkan dengan koma jika lebih dari satu, contoh: 2, 3, 10) [Default: 5]: ").strip()
if n_input:
    # Memisahkan input koma dan mengubahnya menjadi list integer
    n_list = [int(val.strip()) for val in n_input.split(",") if val.strip().isdigit()]
else:
    n_list = [5]

f_num = sp.lambdify(x, f, 'numpy')

if exact_value.is_real:
    exact_val_float = float(exact_value)

    # Membuat header tabel Markdown
    table_md = "| $n$ | $h$ | Nilai Integrasi ($L$) | Galat/Error ($E_t$) |\n"
    table_md += "|:---:|:---:|:---------------------:|:-------------------:|\n"

    for n in n_list:
        h = (b - a) / n
        L = integrasi_trapesium(f_num, a, b, n)

        # Hitung galat relatif sejati (%)
        if exact_val_float != 0:
            error = abs((exact_val_float - L) / exact_val_float) * 100
            error_str = f"{error:.2f}%"
        else:
            error_str = "-"

        table_md += f"| {n} | {h:.2f} | {L:.4f} | {error_str} |\n"

    display(Markdown(table_md))
else:
    print(f"{exact_value} is not Real/Undefined")
```


| $n$ | $h$ | Nilai Integrasi ($L$) | Galat/Error ($E_t$) |
|:---:|:---:|:---------------------:|:-------------------:|
| 1 | 0.80 | 0.1728 | 89.47% |



### Simulasi Dinamis: Tabel Hasil Integrasi & Error (Trapesium Berganda)


```python
from IPython.display import Markdown, display
import sympy as sp

if exact_value.is_real:
    exact_val_float = float(exact_value)

    print("Hasil integral dari integral trapesium berganda untuk")
    show("f(x) = ", f)
    print(f"dari a = {a} sampai b = {b} dapat dilihat pada tabel berikut:\n")

    # Menentukan nilai awal n_start dari Cell 7
    n_start = min(n_list) if ('n_list' in globals() and n_list) else 2
    if n_start < 2:
        n_start = 2

    f_num = sp.lambdify(x, f, 'numpy')

    # Membuat tabel Markdown lengkap secara dinamis hingga galat <= 1%
    table_md = "| $n$ | $h$ | $L$ | $E \\%$ |\n"
    table_md += "|:---:|:---:|:---------------------:|:---------------------:|\n"

    n = n_start
    max_iterations = 100  # Batas keamanan untuk menghindari tabel yang terlalu panjang/hang

    for _ in range(max_iterations):
        h = (b - a) / n
        L = integrasi_trapesium(f_num, a, b, n)

        # Hitung galat error (%)
        if exact_val_float != 0:
            error = abs((exact_val_float - L) / exact_val_float) * 100
            error_str = f"{error:.1f}%"
        else:
            error = 0.0
            error_str = "-"

        table_md += f"| {n} | {h:.4f} | {L:.4f} | {error_str} |\n"

        # Kondisi berhenti jika galat error <= 1%
        if error <= 1.0:
            break

        n += 1

    display(Markdown(table_md))
else:
    print(f"{exact_value} is not Real/Undefined")
```

    Hasil integral dari integral trapesium berganda untuk



$\displaystyle f(x) = 400 x^{5} - 900 x^{4} + 675 x^{3} - 200 x^{2} + 25 x + 0.2$


    dari a = 0.0 sampai b = 0.8 dapat dilihat pada tabel berikut:
    



| $n$ | $h$ | $L$ | $E \%$ |
|:---:|:---:|:---------------------:|:---------------------:|
| 2 | 0.4000 | 1.0688 | 34.9% |
| 3 | 0.2667 | 1.3696 | 16.5% |
| 4 | 0.2000 | 1.4848 | 9.5% |
| 5 | 0.1600 | 1.5399 | 6.1% |
| 6 | 0.1333 | 1.5703 | 4.3% |
| 7 | 0.1143 | 1.5887 | 3.2% |
| 8 | 0.1000 | 1.6008 | 2.4% |
| 9 | 0.0889 | 1.6091 | 1.9% |
| 10 | 0.0800 | 1.6150 | 1.6% |
| 11 | 0.0727 | 1.6195 | 1.3% |
| 12 | 0.0667 | 1.6228 | 1.1% |
| 13 | 0.0615 | 1.6254 | 0.9% |


