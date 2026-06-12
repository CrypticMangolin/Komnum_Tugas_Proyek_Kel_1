<a href="https://colab.research.google.com/github/CrypticMangolin/super-duper-engine/blob/main/Komnum_Tugas_Proyek.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

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


```python
def integrasi_trapesium(f, a, b, n):
    h = (b - a) / n
    xi_sum = sum(f(a + i * h) for i in range(1, n))
    L = h * (0.5 * f(a) + xi_sum + 0.5 * f(b))
    return L
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
a, b = 0, 2
# f_str = input("Enter function (example 3x^5+2x^2+5): ")
# f_str = "x**3+2x+5"
# f_str = "0.2+25x-200x^2+675x^3-900x^4+400x^5"
f_str = "x^2+5"
# f_str = "(x+1)/(x-2)"
# f_str = "sin(x)"
# f_str = "e^x"

f = parse_function(f_str)
show(f)
```


$\displaystyle x^{2} + 5$


Pada tahap ini (di atas) ditentukan batas bawah dan batas atas integral, yaitu *a = 0* dan *b = 2*. Selanjutnya fungsi yang akan diintegralkan didefinisikan dalam bentuk string. Program menyediakan beberapa contoh fungsi seperti fungsi polinomial, rasional, trigonometri, dan eksponensial. Fungsi yang dipilih kemudian diproses menggunakan `parse_function()` untuk diubah menjadi ekspresi simbolik SymPy. Setelah itu, fungsi ditampilkan dalam format LaTeX menggunakan `show()` sehingga pengguna dapat memverifikasi bahwa fungsi yang akan digunakan dalam proses integrasi telah sesuai.


```python
F = sp.integrate(f, x)
show(sp.Integral(f, x),"=", F,"+C")
```


$\displaystyle \int \left(x^{2} + 5\right) dx=\frac{x^{3}}{3} + 5 x+C$


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


$\displaystyle \int\limits_{0}^{2} f{\left(x \right)} dx=\frac{38}{3}=12.67$



```python
n=5
f_num = sp.lambdify(x, f, 'numpy')
if exact_value.is_real:
    show(f"L = {integrasi_trapesium(f_num, a, b, n):.2f}")
else:
    print(f"{exact_value} is not Real/Undefined")
```


$\displaystyle L = 12.72$

