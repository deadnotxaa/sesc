# Задание 16 — Вычисление рекуррентных выражений
Примерное время выполнения задания — *5 минут (по спецификатору)*
<br><br>
### Относительно способа решения задания можно разделить на 2 основные группы:
1. Можно решить на **Python** (за адекватный временной промежуток)
2. Можно решить на **C++** (если на питоне слишком долго)

К первому типу относятся все задания из открытого банка ФИПИ (и вероятно любые, которые попадутся на ЕГЭ)
<br><br>

### 1 тип. Задание из открытого банка ФИПИ (№*7C0639*)
*«Задания из открытого банка халява»* — прим. ред. <br>
> Алгоритм вычисления значения функции F(n), где n –
> целое неотрицательное число, задан следующими соотношениями:
> 
> F(n) = 0 при n ≤ 1;  
> F(n) = (n + 1) / 2 + F(n − 1), если n > 1 и при этом n нечётно;  
> F(n) = 2 × F(n − 1) + 1, если n > 1 и при этом n чётно.  
> 
> Чему равно значение функции **F(33)**?  
> Примечание. При вычислении значения F(n) используется операция целочисленного деления.
<br>

```python
def f(n):
    if n <= 1:
        return 0
    elif n % 2:
        return (n + 1) // 2 + f(n - 1)
    else:
        return 2 * f(n - 1) + 1

# Комментриев не будет, слишком легко 
print(f(33))
```

**Ответ: 262124** 
<br><br>
### 2 тип. Задание с сайта Полякова (№6075)
> Обозначим частное от деления натурального числа a на натуральное число b как a // b,
> а остаток как a % b. Алгоритм вычисления функции F(n),
> где n – неотрицательное число, задан следующими соотношениями:  
> 
> F(n) = 0, если n = 0  
> F(n) = F(n // 10) + (n % 10).   
> 
> Найдите количество таких чисел в диапазоне от 865 432 015, 1 585 342 628, для которых **F(n) > F(n + 1)**.
<br>
Попробуем решить задачу, написав код на Python:  
<br><br>
  
```python
# Импортируем кэш 
from functools import lru_cache

# перед рекурсивной функцией пишем декоратор
# в качестве аргумента передаем None,
# чтобы не было ограничений по кол-ву используемой памяти
@lru_cache(None)
def f(n):
    if not n:
        return 0
    return f(n // 10) + (n % 10)


xnt = 0 # здесь подсчитываем кол-во удовлетворяющих значений
for i in range(865_432_015, 1_585_342_628):
    if f(i) > f(i + 1):
        xnt += 1
print(xnt)
```  

К сожалению, проработав 5 минут и использовав 4.5Гб оперативной памяти, программа завершается с ошибкой **MemoryError**  
  
Напишем аналогичный код на **C++**  

```cpp
// подключаем волшебную библиотеку, которой пользуются олимпиадники
// а волшебная она из-за того, что подключив её вы автоматически подключите основные библиотеки такие как iostream, cmath и т.д.
#include <bits/stdc++.h>

// тоже волшебная строчка, которая влияет на работу компилятора
// её используют не все, но в некоторых случаях она позволяет ускорить работу программы (этот код с ней работает в 6 раз быстрее)
#pragma GCC optimize("Ofast")
using namespace std;

// рекурсивная функция, описанная в условии задачи
int f(int n) {
    // !n аналог not n в python
    // всё подчиняется закону: ноль это ноль, а всё, что не ноль — это единица (а 0 и 1 эквивалент false и true)
    if (!n) 
        return 0;
    // в плюсах оператор / работает отвечает как за обычное деление, так и за целочисленное
    // чтобы использовать целочисленное деление нужно проводить его над целочисленными переменными (short, int, long, etc.)
    // операция деления даст дробный результат при проведении над переменными с плавающей точкой (float and double)
    return f(n / 10) + (n % 10);  
}

int main() {
    int cnt = 0; // счётчик
    for (int i = 865432015; i <= 1585342628; ++i) {
        if (f(i) > f(i + 1))
            cnt++;
    }
    cout << cnt << '\n'; // вывод результата
}
```
Спустя 30 секунд получим ответ **71991061**

<br><br>
