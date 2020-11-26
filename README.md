# Лабораторная работа 0

## ЗАДАНИЕ
<b> Задача </b>: реализовать алгоритм перемножения матриц <br>
<b> Язык </b>: C++ или Python <br>
<b> Входные данные </b>: 2 матрицы размером от 100х100 до 2000х2000. <br>
<b> Выходные данные </b>: проверка корректности перемножения + время вычисления <br>
<b> Реализация должна содержать 2 функции перемножения матриц </b>: на CPU и на GPU с применением CUDA. <br>
Отчет о проделанной лабораторной работе - это git-репозиторий с исходным кодом реализации + описание проделанной работы там же в readme. <br>
Необходимо описать реализацию, объяснив, что конкретно было распараллелено и почему.  <br>
<b>Провести эксперименты </b>: перемножить матрицы разных размеров, посчитать ускорение. Результаты привести в виде таблицы. <br>

## СРЕДА
<b>Язык программирования</b>: Python, v = 3.x.x <br>
<b>IDE</b>: google colab <br>
<b>GPU</b>: NVIDIA Tesla T4 <br>

## ОПИСАНИЕ
В ходе работы был написан код для двух экспериментов - сравнения ускорения для умножения матриц при помощи GPU, в том числе и с разделяемой памятью, относительно параллельного алгоритма перемножения матриц. Алгоритм с разделяемой памятью реализует блочный алгоритм перемножения матриц, когда данные загружаются в разделяемую память частями, после чего промежуточные результаты суммируются. Второй алгоритм с более простой реализацией производит многократное копирование данных для нити и перемножает матрицы стандартным способом.
<br><br>
Для эксперимента были использованы матрицы, размерности которых кратны 32, чтобы загрузка блоков GPU была максимальной.

## РЕЗУЛЬТАТЫ ВЫЧИСЛЕНИЙ

|     | Время на CPU | Время на GPU |    Разница |
|----:|-------------:|-------------:|-----------:|
| 128 |     2.354504 |     0.920238 |   1.434266 |
| 256 |    19.088963 |     0.272060 |  18.816903 |
| 512 |   153.401633 |     0.297645 | 153.103988 |

# Лабораторная работа 1. PiCalc

## СРЕДА
<b>Язык программирования</b>: Python, v = 3.x.x <br>
<b>IDE</b>: google colab <br>
<b>GPU</b>: NVIDIA Tesla T4 <br>

## ОПИСАНИЕ

Реализованы функции на GPU/CPU
* GPU: используется pycuda -> SourceModule содержит реализацию алгоритма на GPU:
   > Есть две последовательности: x, y ; -> 
   > Считаем temp = x^2 + y^2; -> 
   > Если temp < 1 , то возвращаем 1 (к счётчику прибавляем 1), иначе возвращаем 0 (оставляем значение счётчика без изменений); -> 
   > Выполняется атомарная операция сложения; ->
   > Домножение на 4/n будет осуществлено позже.
   > Размер блока: (128,1,1), т.к. массивы одномерные;
   > Размер грида: (int(n/(128 * block[0])), 1)
* CPU: алгоритм такой же, как у GPU, домножение осуществляется в цикле.

## РЕЗУЛЬТАТЫ ВЫЧИСЛЕНИЙ

* Произведены замеры времени и сравнение с числом пи, предоставленым библиотекой numpy:

|      n      |       GPU время      |      CPU время     |      Ускорение     |    Полученное Pi   | Сравнение с Pi из NumPy |
|:-----------:|:--------------------:|:------------------:|:------------------:|:------------------:|:-----------------------:|
| 65536*16    | 0.012803792953491211 | 1.2068207263946533 |  94.25493920265163 | 3.1400413513183594 |    0.001551302271433741 |
| 1048576*16  |  0.06619715690612793 | 18.788211345672607 |  283.8220283737498 | 3.1419284343719482 |   0.0003357807821551262 |
| 16777216*16 |   0.9365365505218506 | 310.75218868255615 | 331.80999557294473 | 3.1415703296661377 |  2.2323923655420685e-05 |


# Лабораторная работа 2. Bilateral filtering

## СРЕДА
<b>Язык программирования</b>: Python, v = 3.x.x <br>
<b>IDE</b>: google colab <br>
<b>GPU</b>: NVIDIA Tesla T4 <br>

## ОПИСАНИЕ

* Лабораторная написана на языке Python 3 с использованием библиотеки 'Numba'
* Для получения массива чисел, характеризующих цвет пикселя использовалась библиотека 'Pillow'
* Файлы с изображениями смотреть в папке [Lab2_Bilateral](https://github.com/Russia163Samara/CUDA-labs/tree/main/Lab2_Bilateral), они подписаны

![japan](https://github.com/Russia163Samara/CUDA-labs/blob/main/Lab2_Bilateral/Orig.jpg)

|     Время CPU     |      Время GPU     | Множитель ускорения |
|:-----------------:|:------------------:|:-------------------:|
| 32.63468098640442 | 0.4001603126525879 | 81.5540171139792    |
