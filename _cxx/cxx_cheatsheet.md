---
title: Шпаргалка по C++
preview: img/cplusplus-logo.png
subtitle: Эта статья даёт сжатый обзор основ современного C++. Шпаргалка фокусируется на языке, а не стандартной библиотеке, и учитывает особенности C++11/C++14
---

> Основано на [Phillip M. Duxbury's C++ Cheatsheet](http://www.pa.msu.edu/~duxbury/courses/phy480/Cpp_refcard.pdf), английская версия доработана Morten Nobel-Jørgensen. Дополнения для C++11 основаны на [ISOCPP.org C++11 Cheatsheet](https://isocpp.org/blog/2012/12/c11-a-cheat-sheet-alex-sinyakov). Оригинал документа на английском размещён на [github.com/mortennobel/cpp-cheatsheet](https://github.com/mortennobel/cpp-cheatsheet).

## Препроцессор

```cpp
                            // Комментарий до конца строки
                            /* Многострочный комментарий */
#include  <stdio.h>         // Вставить содержимое заголовка, поиск по стандартным путям заголовков  
#include "myfile.h"         // Вставить содержимое заголовка, поиск начиная с текущего каталога
#define X some text         // Замена X на some text
#define F(a, b) a + b       // Замена F(1, 2) на 1 + 2
#define X \
 some text                  // Многострочное определение макроса
#undef X                    // Удаляет определение макроса
#if defined(X)              // Условная компиляция (#ifdef X)
#else                       // Опциональная ветвь else (эквивалент #ifndef X или #if !defined(X))
#endif                      // Завершает блоки #if, #ifdef
```

## Литералы и переменные

```cpp
255, 0377, 0xff, 0b1011     // Целые числа (десятичные, восьмиричные, шестнадцатиричные, двоичные)
2147483647L, 0x7fffffffl    // Целые числа long (64bit на 64-битных UNIX платформах, 32bit на остальных) 
123.0, 1.23e2               // Действительные числа двойной точности (double)
'a', '\141', '\x61'         // Символы (литеральный, восьмиричный код, шестнадцатиричный код)
'\n', '\\', '\'', '\"'      // Литералы символа переноса строки, обратного слеша, одинарной кавычки, двойной кавычки
"string\n"                  // Массив символов, завершённый переносом строки и нулевым символом \0
"hello" "world"             // Конкатенация двух литералов
true, false                 // Булевы константы ИСТИНА и ЛОЖЬ
nullptr                     // Литерал указателя, указывает на нулевой (недопустимый) адрес
```

## Объявления

```cpp
int x;                      // Объявление переменной x типа int, т.е. целого числа (значение не определено)
int x = 255;                // Объявление переменной и инициализация значением 255
unsigned ing x = 255u;      // Объявление переменной типа беззнаковое целое число, инициализация значением 255.
char c = 'a';               // Объявление переменной, хранящей символ (однобайтовый, знаковый или беззнаковый)
unsigned char u = 255;      // Объявление переменной типа "беззнаковый символ"
signed char s = -1;         // Объявление переменной типа "знаковый символ"
float f;                    // Знаковое действительное число одинарной точности (обычно 4 байта)
double f;                   // Знаковое действительное число двойной точности (обычно 8 байт)
bool b = true;              // Булевы переменные инициализируются значением true либо false 
int a[10];                  // Массив из 10 целых числе (индексация начиная с нуля, от a[0] до a[9]) 
int a[] = { 0, 1, 2 };      // Инициализированный массив (эвивалентно a[3] = { 0, 1, 2 };) 
int a[2][2] = { { 1, 2 },    // Двумерный массив, т.е. массив массивов целых чисел
                { 4, 5 } };
char str[] = "hello";       // Строка в стиле языка C (6 элементов, включая '\0' в конце)
std::string str = "Hello"   // Строка в стиле C++, инициализированная значением "Hello"
std::string str = R"(Hello
World)";                    // Строка в стиле C++, инициализированная значением "Hello\nWorld"
int* ptr = nullptr;         // ptr - указатель на адрес в памяти, предназначенный для хранения int 
const char* str = "hello";  // str указывает на анонимный массив, хранящий значение "hello\0" 
void* ptr = nullptr;        // ptr хранит адрес нетипизированной области памяти
int& ref = x;               // ref - ссылка на int x (по сути синоним x)

enum Weekend                // Weekend - это тип, способный хранить WK_SATURDAY или WK_SUNDAY
{
    WK_SATURDAY,
    WK_SUNDAY,
};
Weekend day;                // day - переменная типа weekend
enum Weekend                // На уровне ассембера тип Weekend представлен целым числом,
{                           // Здесь для WK_SATURDAY и WK_SUNDAY явно назначаем значения 0 и 1
    WK_SATURDAY = 0,
    WK_SUNDAY = 1,
};
enum class Color            // Современный enum: константы не попадают в глобальную область видимости,
{                           //  обращаться к ним можно конструкциями Color::Red и Color::Blue.
    Red,
    Blue,
};
Color color = Color::Red;   // Объявляем переменную типа color, инициализируем значением Color::Red.

typedef char* ZeroedString; // Устаревшее добавление синонима типа: объявить `ZeroedString x;` значит объявить `char* x;`.
using ZeroedString = char*; // Современное добавление синонима типа.

const int c = 3;            // Константные значения инициализируются один раз и не могут быть повторно присвоены.
const int pow = GetPower(); // Инициализация константы может быть динамической.
const int* ptr = arr;       // Нельзя записывать значения в память, на которую указывает ptr (т.е. в элементы arr). 
int* const ptr = arr;       // Нельзя менять значение указателя ptr (но можно записывать в память, куда он указывает) 
const int* const ptr = arr; // Как ptr, так и память, на которую он указывает, недоступны для записи. 
const int& cref = x;        // cref не получится использовать для изменения x 
int8_t, uint8_t, int16_t,   // Целые числа с фиксированным числом бит, не зависящим от платформы.
uint16_t, int32_t, uint32_t,
int64_t, uint64_t

auto it = m.begin();        // Объявляем переменную it, тип выставляется автоматически как тип инициализатора m.begin() 
auto const param            // Объявляем переменную param, тип выставляется автоматически, но будет константным.
    = config["param"];
auto& instance              // Объявляем переменную instance, тип выставляется автоматически, но будет ссылочным
    = singleton::instance();
```

## Классы памяти

```cpp
int x;                      // Память для x выделяется автоматически (обычно на стеке) и существует только в области видимости.
static int x;               // Память для x выделяется в глобальной области (даже если x объявлена внутри функции).
extern int x;               // Переменная x объявлена, но её расположение в памяти будет указано в другом месте.
```

## Инструкции

```cpp
x = y;                      // Любое невложенное выражение (включая вызов функции, присваивание) является инструкцией.
int x = 0;                  // Объявления являются инструкциями.
;                           // Пустая инструкция.
{                           // Блок внутри {} является одной инструкцией
  int x;                    //  Области видимости x - от объявления до конца блока. 
}

if (x) a;                   // Если значение x - ИСТИНА (не 0), то выполнить инструкцию print('a'); 
else if (y) print('b');     // Если не x и y, выполняем print('b'), else if необязателен, его можно повторять;
else print('c');            // Если не x и не y, выполняем print('c') else необязателен. 

while (x)                   // Повторяем 0 или более раз, пока выражение x имеет значение ИСТИНА 
    print(x);

for (int i = 0; i < 10; ++i)// Эквивалентно `int i = 0; while (i < 10) { doSomething(); ++i; }`
    doSomething();            

int arr[] = { 5, 10, 20 };
for (auto x : arr)          // Range-based цикл for, x последовательно принимает все значения элементов arr.
    print(x);

do                          // Эквивалентно цепочке `foo(); while(x) foo()`;
{
    foo();
} while (x);

switch (x)                  // Switch выполнит прыжок на один из case/default
{                           //  x должен быть целочисленным типом (enum допускается, строки - нет)
    case X1: a;             // Если x == X1 (X1 должно быть константой времени компиляции), продолжаем выполнение отсюда
    case X2: b;             // Иначе если x == X2, продолжаем выполнение отсюда 
    default: c;             // В противном случае продолжаем выполнение отсюда (метка default опциональная)
}

switch (x)
{
    case X1:
        a;
        break;              // break прерывает выполнение после метки до конца switch,
    default:                //  если break нет, то после a будет продолжено выполнени и выполнено c, в отличии от инструкции if
        c;
}

break;                      // Выход из ветвления switch либо из цикла while, do, или for
continue;                   // Прыжок в конец цикла while, do или for
return x;                   // Завершает выполнение функции, возвращает значение x

try
{
    a;    
}
catch (const ExceptionType &ex)
{
    b;                      // Если a; бросает исключение типа ExceptionType, выполнение продолжается здесь
}
catch (...)
{
    c;                      // Если a; бросает какое-либо другое исключение, выполнение продолжается здесь
}
```

## Функции

```cpp
int sum(int a, int b);      // Объявление функции sum, принимающей два параметра int и возвращающей int
void fn();                  // fn - это процедура без параметров (псевдо-тип void означает, что возвращаемого значения нет)
void fn(int a = 12);        // можно написать fn(), и это будет эквивалентно f(12), т.е. параметр имеет значение по умолчанию

int sum(int a, int b)       // Определение функции sum: тело функции станет её реализацией.
{
    statements;
}

T operator+(T x, T y)       // В выражении вида "a + b", где a, b имеют тип T, вызывается тело функции operator+(a, b)
{
    // Реализация оператора...
}

T operator-(T x);           // Унарный оператор "минус", вызывается в выражениях вида "-a"

T operator++();             // Префиксная форма оператора a++ или a--

T operator++(int);          // Постфиксная форма оператора a++ или a-- (параметр типа int игнорируется)

extern "C"                  // Все функции, объявленные внутри блока extern "C", не будут подвержены name mangling.
{                           // Это полезно, если функции могут быть вызваны из другого языка (например, из C).
    void fn();
}
```

Типы параметров и возвращаемого значения функции могут быть любыми. Функция должна быть либо объявлена (без тела),
либо определена (с телом функции) до первого вызова. Можно сначала объявить функцию (и вызвать где-либо), а затем
определить (возможно, в другом файле). Каждая программа состоит набора файлов, каждый файл содержит набор
глобальных переменных и набор функций. Одна из функций - main - служит точной входа в программу.

```cpp
int main()
{
    statements...
}

int main(int argc, char* argv[])
{
    statements...
} 
```

- argv является массивом длины argc, хранящим строки параметров командной строки
- по общему соглашению, main возвращает 0 при успешном выполнении программы, ненулевой код после возникновения ошибки

Функции с различными параметрами могут иметь одинаковое имя (это называется перегрузка функций).

Могут быть перегружены все операторы, кроме "::", ".", ".*", "?:". Перегрузка не меняет приоритет оператора.

## Выражения

Операторы сгруппированы по приоритету, сперва наивысший приоритет. Унарные операторы и присваивание вычисляются
справа налево, все остальные слева направо. Во время выполнения программы не происходит проверок на выход за
границы массива, на недопустимые указатели и т.п. 

```cpp	
T::X                        // Доступ к символу X, объявленному в классе T
N::X                        // Доступ к символу X, объявленному в пространстве имён N
::X                         // Доступ к глобальному имени X (может помочь избежать конфликтов имён)

t.x                         // Поле или метод объекта t (структуры или класса)
ptr->x                      // Поле или метод того объекта (структуры или класса), на который указывает ptr
arr[i]                      // i-й элемент массива arr
fn(x, y)                    // Вызов фукнции fn с аргументами x и y
T(x, y)                     // Конструирование объекта класса T, в конструктор передаются значения x и y
x++                         // Увеличивает x на единицу, но возвращает старое значение (постфиксная форма)
x--                         // Вычитает единицу из x, но возвращает старое значение (постфиксная форма)
typeid(x)                   // Вычисляет значение типа std::type_info для объекта x
typeid(T)                   // Вычисляет (обычно при компиляции) значение типа std::type_info для типа T
dynamic_cast<T>(x)          // Приводит x к типу T во время выполнения, выполняет проверку с помощью
                            //  информации о виртуальных методах объекта.
static_cast<T>(x)           // Приводит x к типу T без каких-либо проверок
reinterpret_cast<T>(x)      // Интерпретирует байты объекта x как байты объекта типа T
const_cast<T>(x)            // Конвертирует x к типу T, убирая модификаторы const и volatile

sizeof x                    // Вычисляет число байт, используемое для хранения объекта x
sizeof(T)                   // Вычисляет число байт, используемое для хранения типа T
++x                         // Увеличивает x на единицу, возвращает новое значение (префиксная форма)
--x                         // Вычитает единицу из x, возвращает новое значение (префиксная форма)
~x                          // Битовая операция: вычисляет битовое дополнение x
!x                          // Возвращает true если x имеет значение ЛОЖЬ или 0, иначе false
-x                          // Унарный минус
+x                          // Унарный плюс
&x                          // Вычисление адреса x
*ptr                        // Доступ к памяти, на которую указывает ptr (т.е. разыменование, *&x эквивалентно x)
new T                       // Выделяет память для объекта типа T, возвращает его адрес
new T(x, y)                 // Выделяет память для объекта типа T, в конструктор передаёт x, y, возвращает адрес
new T[n]                    // Выделяет память для массива из n элементов типа T
delete ptr                  // Удаляет объект, на который указывает ptr, возвращает системе занятую им память
delete[] ptr                // Удаляет массив объектов, на которые указывает ptr
(T) x                       // Преобразует x к типу T (устаревшая форма, используйте static_cast или в крайнем случае reinterpret_case) 

x * y                       // Умножение 
x / y                       // Деление (для целых чисел происходит отбрасывание дробной части) 
x % y                       // Получение остатка (знак результата совпадает со знаком x) 
x + y                       // Сложение целых чисел либо целочисленного смещения и указателя
                            //  (для указателей эквивалентно выражению &x[y])
x - y                       // Вычитание целых чисел либо получение смещения от указателя y к указателю x
x << y                      // Битовая операция: смещение x на y бит влево, эквивалентно x * pow(2, y)
x >> y                      // Битовая операция: смещение x на y бит вправо, эквивалентно x / pow(2, y)

x < y                       // ИСТИНА, если x меньше чем y
x <= y                      // ИСТИНА, если x меньше или равен y
x > y                       // ИСТИНА, если x больше чем y
x >= y                      // ИСТИНА, если x больше или равен y

x & y                       // Битовая операция "И": 3 & 6 равно 2
x ^ y                       // Битовая операция "ИСКЛЮЧАЮЩЕЕ ИЛИ": 3 ^ 6 равно 5
x | y                       // Битовая операция "ИЛИ": 3 | 6 равн 7
x && y                      // Логическое "И": вычисляет x, и если x ЛОЖЬ, возвращает ЛОЖЬ, иначе
                            //  вычисляет y и возвращает его булево значение (ИСТИНА или ЛОЖЬ).
x || y                      // Логическое "ИЛИ": вычисляет x, и если x ИСТИНА, возвращает ИСТИНА,
                            //  иначе вычисляет y и возвращает его булево значение (ИСТИНА или ЛОЖЬ).
x = y                       // Присваивает значение y переменной x, возвращает новое значение x
x += y                      // Эквивалент x = x + y, также существуют -= *= /= <<= >>= &= |= ^=
x ? y : z                   // Тернарный оператор: возвращает y если x ИСТИНА, иначе z
throw x                     // Выбрасывает исключение, а если оно не поймано, аварийно завершает программу
x, y                        // Вычисляет x и y, затем возвращает y (редко используется)
```

## Анонимные функции

Анонимные функции поддерживают замыкание, т.е. захват и удержание внешних переменных.

```cpp
auto fn1 = [] {             // Простая анонимная функция типа `void()`
    /* тело функции */;
};
auto fn2 = [] {
};
static_assert(!std::is_same_v(fn1, fn2),
              "Две одинаковые с виду лямбды имеют разные типы."
              "Каждая лямбда принадлежит уникальному типу данных,"
              "имеющему operator().");

int value1 = 10;            // Захватим value1 по значению: [value]
int value2 = 10;            // Захватим value2 по ссылке: [&value2]
auto fn = [value1, &value2] {
    ++value1;
    ++value2;
};
assert(value1 == 10);       // Значение не изменилось (изменялась копия)
assert(value2 == 11);       // Значение изменилось

auto sum = [](int a, int b) -> int {
    return a + b;
};
int x = sum(10, 50);        // sum принимает 2 параметра int, возвращает int

int count = 0;              // mutable означает, что захваченное
                            //  по значению сохраняется между вызовами
auto bump = [count]() mutable {
    ++count;
    printf("count: %d", count);
};
bump();                     // печатает 'count: 1'
bump();                     // печатает 'count: 2'

auto sum = [](auto && a, auto && b) {
    return a + b;           // возвращаемый тип определится автоматически
};
double x = sum(10.2, 40);   // sum - обобщённая анонимная функция,
                            //  псевдо-тип auto служит местом
                            //  для параметра любого типа

// Условие в if constexpr вычисляется при компиляции,
//  это позволяет в одной обобщённой анонимной функции
//  обработать разные типы данных.
auto println = [](auto && value) {
    if constexpr (typeid(value) == typeid(int)) {
        printf("%d\n", value);
    }
    if constexpr (typeid(value) == typeid(std::string)) {
        printf("%s\n", value.c_str());
    }
};
```

## Классы

```cpp
class T                     // Новый тип данных T
{
private:                    // В этой секции символы доступны только для методов класса T
protected:                  // В этой секции символы доступны ещё для классов-наследников класса T
public:                     // В этой секции символы доступны для всех желающих
    int x;                  // Поле класса, располагается в памяти как часть объекта класса
    void f();               // Метод (функция, являющаяся членом класса)
    void g() { return; }    // Метод с телом, встроенным в класс
    void h() const;         // Метод не сможет модифицировать какие-либо поля класса
    int operator+(int y);   // t + y превращается в вызов метода-оператора t.operator+(y)
    int operator-();        // -t превращается в вызов метода-оператора t.operator-()
    T(): x(1) {}            // Конструктор использует списки инициализации конструктора: ": x(1)"
    T(const T& t): x(t.x) {}// Конструктор копирования
    T& operator=(const T& t)// Оператор присваивания
    {
        x = t.x;
        return *this;
    }
    ~T();                   // Деструктор (автоматически вызываемая процедура очистки)
    explicit T(int a);      // Позволяет писать t = T(3), но не t = 3
    T(float x): T((int)x) {}// Делегирующий конструктор, делегирует инициализацию в T(int)
    operator int() const
    {return x;}             // Оператор преобразования в int, допускает код x = int(t)
    static int y;           // Поле y становится единственным и глобальным для всех объектов типа T
    static void l();        // Метод становится методом класса, доступен для вызова через T::l()
    class Z {};             // Вложенный класс T::Z
    typedef int Int;        // T::Int - синоним типа int
};
void T::fn()                // Тело метода fn класса T 
{
    this->x = x;            // this - это адрес текущего объекта (здесь поле x копируется в поле x) 
}
int T::y = 2;               // Инициализация статической переменной
                            //  (должна быть вне класса для всех типов, кроме целочисленных) 
T::l();                     // Вызов статического метода (метода класса)
T t;                        // Создание объекта t типа T с неявным вызовом конструктора T()
t.f();                      // Вызов метода f объекта t
 
struct T {                  // Эквивалентно коду class T { public: 
    virtual void i();       // Реализация виртуального метода i может быть перегружена классом-наследником  
    virtual void g() = 0;   // Чисто виртуальный метод, должен быть перезаписан в классе-наследнике
};
class U : public T          // Дочерний класс U наследует все поля и методы базового класса T
{
public:
    void g(int) override;   // Перегрузка метода g
};
```

Все классы по умолчанию имеют конструктор копирования, оператор присваивания и деструктор, которые
рекурсивно вызывают соответствующие методы для базовых классов и полей. Примитивные типы, такие как
целые числа, указатели и т.п., копируются прямым копированием памяти, а их деструктор ничего не делает.
Конструкторы, операторы присваивания и дееструкторы никогда не копируются. Если у класса не указан
ни один конструктор, то у него есть конструктор по умолчанию без аргументов, который лишь вызывает
другие конструкторы.

## Шаблоны

```cpp
template <class T>          // Функция fn будет перегружена для любых типов-аргументов
T fn(T t);

template <class T>
class X                     // Класс параметризуется типом T
{
    X(T t);                 // Конструктор принимает значение типа T
};

template <class T>
X<T>::X(T t)                // Определение конструктора (метода) за пределами класса
{
}

X<int> x(3);                // Тип объекта x - это X<int>, специализация шаблонного класса X

template <class T, class U = T, int n = 0>
class A                     // Шаблон имеет типы-параметры с типами по умолчанию
{
};
```

## Пространства имён

```cpp
namespace N {class T {};}   // Помещает имя T в пространство имён N
N::T t;                     // Используем имя T из пространства имён N
using namespace N;          // Все символы из пространства имён N теперь доступны без префикса N::
using N::T;                 // Символ N::T теперь доступен без префикса N::
```

## Отладочный механизм assert

```cpp
#include <cassert>          // Включаем cassert
assert(cond);               // Если условие cond не выполняется, распечатать сообщение и аварийно
                            //  завершить программу. В Release-конфигурациях объявлен макрос
                            //  NDEBUG, который превращает assert в пустой макрос. Пустой макрос
                            //  ничего не выполняет и не вычисляет выражение cond.
```