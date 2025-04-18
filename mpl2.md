## Слайдеры

[Документ скачать можно здесь](./Matplotlib___слайдеры__3d.pdf)

Понятия:
- Слайдеры - Штуки с помощью которых можно регулировать значение переменных.
- Фигуры (Figures) - внутри MatPlotLib - **окна**, в которых происходит рисование.
- Оси (Axes) - внутри MatPlotLib - подграфики, внутри одного окна (фигуры) может быть несколько осей, в каждом свой график.

Импортируем библиотеки:

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.widgets import Slider
```

Задаем начальные значения:

```python
x = np.linspace(0, 10, 100) # сто точек от 0 до 10

a = 0
b = 1
c = 1
d = 0

y = a + b * np.sin(c * x + d) # функция, которую будем рисовать
```

Настраиваем отображение графика:

```python
fig, ax = plt.subplots() # Создаем новую фигуру и оси
plt.subplots_adjust(bottom=0.5) # Задаем отступ снизу, оставляя место для слайдеров
(line,) = ax.plot(x, y) # Рисуем функцию и сохраняем получившуюся линию
```

Создаем слайдеры на каждый параметр:

```python
# ОтступСлева, ОтступСнизу, Ширина, Высота
ax_a = plt.axes([0.1, 0.25, 0.65, 0.03])
# Слайдер принимает: ось, название, начало, конец, начальное значение
a_slider = Slider(ax_a, 'a', 0.1, 10.0, valinit=a)

ax_b = plt.axes([0.1, 0.20, 0.65, 0.03])
b_slider = Slider(ax_b, 'b', 0.1, 10.0, valinit=b)

ax_c = plt.axes([0.1, 0.15, 0.65, 0.03])
c_slider = Slider(ax_c, 'c', 0.1, 10.0, valinit=c)

ax_d = plt.axes([0.1, 0.1, 0.65, 0.03])
d_slider = Slider(ax_d, 'd', 0.1, 10.0, valinit=d)
```

Создаем функцию, которая будет вызываться каждый раз, когда мы двигаем слайдер:

```python
def update(val):
    # Берем текущее значение каждого из слайдеров
    a = a_slider.val
    b = b_slider.val
    c = c_slider.val
    d = d_slider.val

    # Снова вычисляем функцию, но уже с новыми значениями
    data = a + b * np.sin(c * x + d)
    # Снова рисуем функцию
    line.set_ydata(data)
    # Говорим matplotlib что график изменился
    fig.canvas.draw_idle()
```

Привязываем функцию к слайдерам:

```python
a_slider.on_changed(update)
b_slider.on_changed(update)
c_slider.on_changed(update)
d_slider.on_changed(update)
```

Показываем график:

```python
plt.show()
```

### Задание

Нарисовать график функции `np.cos(a * x + b) + np.sin(c * x + d)`. Все 4 параметра (a, b, c, d) должны быть регулируемы.

## 3D 🤯

Создаем сетку для данных:

```python
X = np.linspace(-1, 1, 1000) # 1000 точек от -1 до 1
Z = np.linspace(-1, 1, 1000)
(X, Z) = np.meshgrid(X, Z) # прямоугольная сетка 1000 на 1000 точек от -1 до 1
```

Задаем функцию $y = \pm \sqrt{1 - x^2}$ (Y для плюс, Y2 для минус):

```python
r = 1 # радиус
Y = np.sqrt(r ** 2 - X ** 2)
Y2 = -Y
```

Создаем новое 3d окно:

```python
fig = plt.figure(figsize=(10, 7))
ax = fig.add_subplot(111, projection='3d')
```

Рисуем **поверхности** (в 2д были линии, здесь - поверхности) в два подхода, сначала для $y \geq 0$, затем для $y \leq 0$:

```python
surf = ax.plot_surface(X, Y, Z, cmap='viridis')
surf = ax.plot_surface(X, Y2, Z, cmap='viridis')
```

Задаем названия осей:

```python
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
```

Показываем график:

```python
plt.show()
```

### Задание

Нарисовать любую другую функцию в трехмерном пространстве, которая покажется вам интересной.
