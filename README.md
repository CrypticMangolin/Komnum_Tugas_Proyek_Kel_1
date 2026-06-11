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


```python
from IPython.display import display, Math
import sympy as sp
from sympy.parsing.sympy_parser import parse_expr, standard_transformations, convert_xor, implicit_multiplication_application

x = sp.symbols('x')

def parse_function(f_str):
    transformations = standard_transformations + (convert_xor, implicit_multiplication_application)
    f = parse_expr(f_str, transformations=transformations)
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


```python
a=0
b=2
# f_str = input("Enter function (example 3x^5+2x^2+5): ")
# f_str = "5x^5+8x^3-3x^2+3"
f_str = "x**2+5"
# f_str = "sin(x)"

f = parse_function(f_str)
show(f)
```


$\displaystyle x^{2} + 5$



```python
F = sp.integrate(f, x)
show(sp.Integral(f, x),"=", F,"+C")

exact_integral = sp.integrate(f, (x, a, b))
exact_value = float(exact_integral) # Hasil True
f_x_str = sp.Integral(sp.Function('f')(x), (x, a, b)) # f(x)
show(f_x_str,"=", exact_integral, "=", f"{exact_value:.2f}")
```


$\displaystyle \int \left(x^{2} + 5\right)\, dx=\frac{x^{3}}{3} + 5 x+C$



$\displaystyle \int\limits_{0}^{2} f{\left(x \right)}\, dx=\frac{38}{3}=12.67$



```python
f_num = sp.lambdify(x, f, 'numpy')
print(f"{integrasi_trapesium(f_num, a, b, 5):.2f}")
```

    12.72
    
