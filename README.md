# Learn C++

## [1.4. Ветвления и циклы](https://academy.yandex.ru/handbook/cpp/article/branches-and-loops)

### Конспект по уроку

#### Сравнение чисел с плавающей точкой

Поскольку операции над числами с `плавающей` точкой могут содержать погрешность, обычное сравнение через `==` некорректно. Поэтому правильнее сравнивать `модуль` разности величин с некой допустимой для нас `погрешностью`. Модуль дробного числа можно получить с помощью функции `std::abs` из заголовочного файла `cmath`

```c++
#include <cmath>
#include <iostream>
 
int main() {
    double delta = 0.000001;
 
    double x = 0.1, y = 0.2;
    double sum = x + y;
 
    if (std::abs(sum - 0.3) < delta) {
        std::cout << "EQUAL ";
    } else {
        std::cout << "NOT EQUAL ";
    }
 
    std::cout << sum << "\n";
}
```
Программа выведет `EQUAL 0.3`.

#### Оператор switch

Выражения внутри скобок оператора `switch` и в блоках `case` должны быть `простого целочисленного` или `символьного типа`. Использование `сложных` типов (например, строк) приведёт к `ошибке` компиляции.

#### Оператор goto

Есть шутка, что оператор безусловного перехода goto — это ругательное слово из четырёх букв. Доказано, что любую программу можно переписать без оператора goto. Поэтому во многих современных языках goto отсутствует.

Однако в C++ этот оператор есть по следующим причинам:
> - обратная совместимость с языком С;
> - удобный выход из вложенных циклов;
> - применение в автосгенерированном коде (например, коде конечного автомата), не предназначенном для чтения человеком.

```c++
int main() {
    again:  // метка — это произвольное имя с двоеточием
 
    std::cout << "How old are you?\n";
    int age;
    std::cin >> age;
 
    if (age < 0 || age >= 128) {
        std::cout << "Wrong age...\n";
        goto again;  // безусловный прыжок в место, помеченное меткой
    }
 
    std::cout << "Your age is " << age << ".\n";
 
    // ...
}
```
Здесь вводится метка `again`, на которую осуществляется переход, если возраст введён некорректно.

#### Цикл while
Цикл `while` — это цикл с предусловием. Перед очередной итерацией `проверяется` условие, и если оно `истинно`, то цикл `продолжается`.

`\t` — знак табуляции. При итеративном выводе позволяет сформировать выровненные колонки с фиксированной шириной.

#### Цикл do-while

Цикл `do-while` — это цикл с постусловием. Отличие от цикла `while` заключается в том, что `первая итерация` всегда выполняется `безусловно`. Только после её завершения проверяется условие цикла. Если оно `истинно`, то цикл `продолжается`.

```c++
#include <iostream>
 
int main() {
    int n = 1;
    do {
        std::cout << n << "\t" << n * n << "\n";
        ++n;
    } while (n <= 10);
}
```
Без особых причин пользоваться этим видом циклов `не стоит`, старайтесь использовать циклы `while` или `for`.

#### Цикл for

Цикл for — самый гибкий. Он записывается так:

```c++
for (initialization; condition; action) {
    // тело цикла
}
```
Начальное значение такого параметра можно задать в разделе `initialization`, условие — в `condition`, а действие над параметром, выполняющееся после каждой итерации, — в `action`.

#### Цикл range-based for

Этот цикл применим к `контейнерам` разной природы (`массивам`, `векторам`, `спискам` и т. д.). Пока рассмотрим его на примере строк. Цикл позволяет удобно проитерироваться по `символам` строки, не используя `индексов`. В этом примере мы `считываем` `строку` и печатаем отдельно все `символы` строки и их `ASCII`-коды:

```c++
#include <iostream>
#include <string>
 
int main() {
    std::string line;
    std::getline(std::cin, line);
    for (char symbol : line) {
        std::cout << symbol << "\t" << static_cast<int>(symbol) << "\n";
    }
}
```
Здесь оператор `static_cast` преобразует символ к числовому типу `int`, чтобы `получить` его `код`. Результат для строки `Hello, world!` выглядит так:

```
H	72
e	101
l	108
l	108
o	111
,	44
 	32
w	119
o	111
r	114
l	108
d	100
!	33
```

Обратите внимание, что `std::string` хранит внутри байты. Если вы вводите символы русского алфавита и у вас используется кодировка `UTF-8`, ставшая де-факто стандартом, то эти символы будут кодироваться парами байтов. И при такой итерации вы увидите отдельные байты, а не символы.

#### Операторы break и continue

Оператор `break` досрочно `заканчивает` текущий цикл. Оператор `continue` прыгает в самый `конец` тела цикла и, если условие цикла позволяет, переходит на `следующую` итерацию.

Напишем программу, которая считывает числа с клавиатуры до тех пор, пока пользователь не введёт ноль, а затем печатает их сумму:
```c++
#include <iostream>
 
int main() {
    int sum = 0;
    while (true) {
        int x;
        std::cin >> x;
        if (x == 0) {
            break;
        }
        sum += x;
    }
    std::cout << sum << "\n";
}
```

#### Считывание до конца ввода

В предыдущем примере мы считывали числа до тех пор, пока на вход не поступит ноль. Это не очень удобно. Предположим, что вместо ввода с клавиатуры мы читали бы эти числа из файла. Это легко сделать, перенаправив файл на стандартный ввод при запуске программы из консоли:
```
./a.out < input.txt
```
Следующий цикл `считывает` числа до тех пор, `пока` поступающие данные `не закончатся`:

```c++
#include <iostream>
 
int main() {
    int sum = 0;
    int x;
    while (std::cin >> x) {
        sum += x;
    }
    std::cout << sum << "\n";
}
```
> `При вводе` данных не из файла, а с клавиатуры можно `сымитировать конец ввода` комбинацией клавиш `Ctrl+D` в `Linux` и `macOS` или `Ctrl+Z` в `Windows`.

Аналогично можно прочитать строки до конца ввода с помощью `std::getline`:

```c++
#include <iostream>
#include <string>
 
int main() {
    std::string name;
    while (std::getline(std::cin, name)) {
        std::cout << "Hello, " << name << "!\n";
    }
}
```

---
---

## Задача 1
***Ход ферзя***

Шахматный ферзь ходит по диагонали, горизонтали или вертикали. Даны две различные клетки на шахматной доске без фигур. Определите, может ли ферзь попасть с первой клетки на вторую одним ходом.

Формат ввода
Программа получает на вход четыре целых числа от 1 до 8. Первая пара чисел задаёт номер столбца и номер строки для первой клетки. Вторая пара чисел аналогично задаёт вторую клетку.

Формат вывода
Программа должна вывести YES, если из первой клетки ходом ферзя можно попасть во вторую, или NO в противном случае.

*Пример 1*

Ввод:
```
1
1
2
2
```

Вывод:
```
YES
```

*Пример 2*

Ввод:
```
1
1
2
3
```

Вывод:
```
NO
```

*Пример 3*

Ввод:
```
5
6
3
3
```
Вывод:
```
NO
```

*Решение:*

---

# Задача 2


### [На главную](https://github.com/syrovezhko/learn-c-plus-plus)
#### [1.3. Типы данных](https://github.com/syrovezhko/learn-c-plus-plus/tree/data-types) | [1.5. Векторы и строки](https://github.com/syrovezhko/learn-c-plus-plus/tree/vectors-and-strings)
#### [Мое резюме](https://github.com/syrovezhko/syrovezhko/blob/main/READMEru.md)