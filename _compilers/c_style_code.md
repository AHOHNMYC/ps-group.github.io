---
title: Особенности языка C и C-style кода
---

Язык C++ — это не просто "C с классами", а совершенно другой язык. Хотя практически любой код на C является одновременно и кодом на C++, общепринятые решения в двух языках сильно различаются, и писать на C++ "в стиле C" не принято.

## Препроцессор

>В языке C принято широко использовать препроцессор, в C++ принято использовать только `#include` и некоторые `#pragma`

В языках C и C++ доступен препроцессор, позволяющий определять макросы для подмены одних строк в исходном коде на другие. Препроцессор запускается до полноценного разбора файла и полноценной компиляции, он принимает на вход файл и создаёт виртуальный файл, в котором произведены замены строк и включения сторонних файлов.

Несколько примеров использования препроцессора:

```cpp
// Перед компиляцией препроцессор включит содержимое header.h в виртуальный образ текущего файла
#include <header.h>

// При использовании кавычек вместо '<' поиск заголовка будет начат в текущем каталоге,
//  иначе поиск будет идти только в системных путях поиска заголовков,
//  где лежат, например, заголовки стандартной библиотеки
#include "header.h"

// ! В C++ так делать не принято !
#define TRUE 1
// Теперь любое отдельно стоящее слово TRUE будет заменено на 1, и компилятор увидит 1 вместо "TRUE"

// Компилятор увидит `const int g_trueValue = 1;`
const int g_trueValue = TRUE;

// ! В C++ так делать не принято !
#define ADD(a, b) ((a) + (b))
// Теперь любая строка вида `ADD(3, 24)` будет заменена строкой `((3) + (27))`
// Лишние скобки нужны, чтобы избежать влияния контекста: разверните мысленно `ADD(a, b) * c`

// Компилятор увидит `const int g_intValue = ((42) + (1));`
const int g_intValue = ADD(42, TRUE);

#undef TRUE
// Теперь подмены TRUE на 1 происходить не будет
```

В языке C часто используют макросы для объявления констант. В C++ использование макросов для объявления констант считается плохим поступком, т.к. макросы игнорируют пространства имён и засоряют доступный программисту набор имён.

```c
// ! В C++ так делать не принято !
#define ITERATIONS_COUNT 10

// В C работать не будет, но в C++ принято делать именно так
constexpr int ITERATIONS_COUNT = 10;

// Альтернативный путь: связанные по смыслу константы можно объявить как значения enum
enum TokenId
{
    TokenPlus = 10,
    TokenMinus, // = 11, т.е. предыдущая константа плюс 1
    TokenNumber, // = 12, т.е. предыдущая константа плюс 1
    TokenString, // = 13, т.е. предыдущая константа плюс 1
};
```

## Префикс struct/enum/union в именах типов

>В C при объявлении структуры ключевое слово struct становится частью имени файла, а в C++ нет

В заголовках и в коде на языке C можно встретить такой код:

```c
// ! В C++ так делать не принято !
// Объявляем структуру `struct StudentInfo_tag` и её синоним StudentInfo
typedef struct StudentInfo_tag {
    int age;
} StudentInfo;

void foo()
{
    struct StudentInfo_tag student1;
    StudentInfo student2;
    // student1 и student2 имеют одинаковый тип.
}
```

В C++ имя структуры является именем типа само по себе, без дополнительного слова `struct`, и можно писать код проще:

```cpp
struct StudentInfo
{

};
```
