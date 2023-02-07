# Learn C++

## [1.2. Первые шаги](https://academy.yandex.ru/handbook/cpp/article/first-steps)

### Конспект по уроку

Получим исполняемый файл с именем `a.out` и запустим его с помощью команды:

```
$ clang++ hello.cpp && ./a.out
Hello, world!
```
---

```std::cin >> ...;``` читает поток данных до ближайшего пробельного разделителя (пробела, табуляции, перевода строки или просто конца файла). Чтобы считать в строковую переменную всю строчку целиком (не включая завершающий символ перевода строки), нужно использовать функцию ```std::getline```  из заголовочного файла `string`

---
---

## Задача 1

Печать текста
Напишите программу, печатающую на экране первые строчки со страницы Бьярне Страуструпа про C++:

> C++ is a general-purpose programming language with a bias towards systems programming that
> - is a better C
> - supports data abstraction
> - supports object-oriented programming
> - supports generic programming.

Не потеряйте парные пробелы в начале строк и переносы в конце строк.

Решение:

```c++

```