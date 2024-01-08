# README

## Занятие 08.01.2024

### Тема: Комплексное повторение пройденного. Функции, колбэки. Некоторые методы массивов.

Репозитарий содержит материалы по занятию группы 220823, посвященному повторению пройденного ранее.

### Цели занятия
- повторение и закрепление знаний;
- решение задач.

### Функции.
> Функция  - это именованный блок кода. Если мы хотим выполнять какой-то блок кода в разных местах - то мы вынесем его в функцию, и сможем вызывать его по имени.

Таким образом, мы пишем функции, чтобы не дублировать код.
> Функция может принимать какие-то аргументы (параметры), и возвращать значение. Аргументы мы передаем при вызове, через запятую. А возвращаемое значение можем указать в самой функции, с помощью ключевого слова return

Что имеется в виду: что мы можем передать в функцию нечто, что нужно этой функции для ее работы. Рассмотрим на примере из реального мира - на кофемашине. Мы передаем ей кофейные зерна, и воду (это параметры), и получаем кофе (это возвращаемое значение).
```
function makeCoffee(coffeeBeans, milk) {
    // реализация функции кофемашины
}

const myCoffee = makeCoffee('Ambassador arabica', 'Evian'); // мы получаем результат и записываем его в переменную
```
Функцию можно задать с помощью ключевого слова `function`. А можно задать по-другому, с помощью стрелочной нотации. В примере ниже, записи функций эквивалентны (то есть делают одно и то же).
```
function makeCoffee(coffeeBeans, water) {
    // реализация
}

const makeCoffee = (cofeeBeans, water) => {
    // реализация
}
```

Небольшая тренировка. Пишем реализацию функции `makeCoffee`. Она принимает 2 аргумента: сорт кофейных зерен (строка), и название воды (строка). Результатом функции также должна быть строка "Ваш кофе готов: вода такая-то, зерна такие-то. Объем 200мл".
Пример:
```
const result = makeCofee('Арабика', 'Боржом');
// ожидаемый результат: в константу result записана строка "Ваш кофе готов: вода Боржом, зерна Арабика. Объем 200мл"
```
Перепишите эту функцию как стрелочную - для закрепления понимания.

Усложним немного нашу задачу. Теперь наша кофемашина принимает третий аргумент, тип напитка (`espresso`, `double espresso`, `americano`). В зависимости от этого, мы должны наливать определенный объем кофе. В случае, если нам заказали эспрессо, мы наливаем 200мл, в случае если американо - 300мл, в случае двойного эспрессо - 400мл.

И еще усложним. Мы улучшаем нашу кофемашину, и хоти научить ее делать латте. Для этого, нам понадобится молоко. Что нам предстоит сделать:
 - добавить еще один аргумент в нашу функцию (чтобы передавать кофемашине молоко).
 - в реализации функции, рассматривать тип напита - `latte`
 - если нам заказали латте, то надо проверить, что нам передали в аргументах молоко (что значение этого аргумента не null, не undefined и не пустая строка)
 - объем наливаемого латте - 500мл.

### Области видимости
Теперь вспомним об областях видимости. Как следует из названия, область видимости - это некая область, где что-то видно (или не видно). Применительно к функциям, это следует трактовать так: видит ли функция переменную, или не видит.
Пример:
```
let outer = 'Outer';

function getVisibility() {
    let inner = 'Inner';
    console.log(inner); // Функция видит эту переменную
    console.log(outer); // Эту переменную функция тоже видит
}

console.log(inner); // Здесь мы получим ошибку - переменная inner объявлена внутри функции getVisibility, и видна только внутри этой функции
console.log(outer); // Здесь все в порядке - переменная outer объявлена в этой области видимости (глобальной)
```

Закрепим результат на примере с кофемашиной. Кофемашина работает от сети. Нам нужно объявить вне функции переменную `power`, в которой будем содержать булеан-значение: есть ли ток или нет. И в функции кофемашине, нам нужно проверить, есть ли ток. Если тока нет (`power = false`) - то возвращаем строку "Несудьба вам попить кофе". Иначе - работаем штатно.

### Колбэки
> Callback, колбэк (обратный вызов) - обычно это функция, которую мы передаем как аргумент в другую функцию. И эта "принимающая" функция сделает вызов нашей переданной функции.

Пример:
```
function acceptCallback(callbackFunc) {
    // ... do something ...
    callbackFunc();
    // ... do something ...
}
```
То есть, мы говорим, что функция `acceptCallback` примет первым аргументом какую-то другую функцию, и мы ее вызовем внутри `acceptCallback` тогда, когда сочтем нужным. Чаще всего это применяется в методах массивов, таймаутах и интервалах, обработчиках событий.

Пример:
```
// Определяем функцию, которую потом планируем использовать как колбэк
function simpleCallback() {
    console.log('Hello from callback!');
}

// Определяем функцию, которая будет принимать и вызывать колбэк
function acceptCallback(callbackFn) {
    console.log('Start execution of acceptCallback function');
    // вызываем колбэк
    callbackFn();
    console.log('Finish');
}

// вызываем нашу функцию, и передаем ей в аргументах ССЫЛКУ на функцию-колбэк
acceptCallback(simpleCallback);
// Ожидаемый результат в консоли:
// Start execution of acceptCallback function
// Hello from callback!
// Finish
```

Теперь пора закрепить это на практике. Написать функцию `doHomework`. Она принимает 2 аргумента: название предмета, по которому выполняем домашку, и функцию, которая решает ДЗ по предмету. Задача функции - вывести в консоль сообщение вида "starting {subject} homework", и вызвать функцию-колбэк. В аргументах функции-колбэка передаем предмет.
```
// Колбэк-функция
function homeworkCallback(subject) {
  console.log(`I finished my ${subject} homework!`);
}

// Функция, которая ожидает колбэк
function doHomework(subject, callback) {
  // Add implementation here
}

doHomework('math', homeworkCallback);
// Expected output:
//  - starting math homework;
//  - I finished my math homework!
```

### Некоторые методы массивов
Теперь вспомним методы массивов - то есть те методы, которые нам доступны "из коробки".
> push, pop, shift, unshift - методы вставки и удаления. Это МУТИРУЮЩИЕ методы, то есть они меняют массив.

- push - добавить элемент в конец массива
- pop - удалить элемент из конца массива
- unshift - добавить элемент в начало массива
- shift - удалить элемент из начала массива

Закрепим на практике. У нас есть массив `numbers`. Нужно решить такие задачи:
1. Добавить в конец массива число 100.
2. Удалить из начала массива первое число, и вывести его в консоль.
3. Удалить последнее число из массива и вывести его в консоль.
4. Добавить в начало массива число 1, и вывести в консоль новую длину массива.
5. Добавить в конец массива числа 1, 2, 3.
6. Удалить из массива последние 2 числа.

> includes, indexOf, lastOndexOf - методы поиска в массиве.

- includes - проверяет наличие элемента в массиве
- indexOf - ищет индекс первого вхождения элемента в массиве. Если есть - возвращает индекс, если такого элемента нет - вернет -1.
- lastIndexOf - ищет индекс последнего вхождения элемента в массиве. Если есть - возвращает индекс, если такого элемента нет - вернет -1.

Тоже закрепим.
1. Проверить наличие в массиве цифры 9.
2. Вывести индекс первого вхождения числа 5.

> forEach - метод перебора массива. For Each - для каждого. То есть, этот метод проходит по массиву, и для каждого элемента выполняет какую-то логику. Логику определяем мы, в колбэк-функции.

Пример: вывести в консоль все элементы массива array.
```
const array = ['simple', 'array'];
function writeElem(elem) {
    console.log(elem);
}

array.forEach(writeElem);
// Ожидаемый результат:
// simple
// array
```
То есть, мы передаем колбэк, который выполняется для каждого элемента массива.
Закрепим практикой.
1. Переписать пример выше с использованием стрелочных функций.
2. Пройтись по массиву `numbers`, вывести в консоль квадрат каждого элемента массива.
3. Посчитать сумму элементов массива.
4. Вывести в консоль только нечетные элементы массива `numbers`.
5. Пройтись по массиву `strings`, вывести в консоль только те элементы, у которых кол-во символов меньше 5.
6. Написать свою реализацию метода `forEach`.

```
function customForEach(array, callback) {
  // ваша реализация forEach
}
```

> map - метод массива, который создает новый массив на основе элементов старого. Map - значит "отображать". То есть, этот метод построит массив "отображений" элементов старого массива. Метод возвращает НОВЫЙ массив, не изменяя старый!

Пример:
```
const array = [1,2,3,4,5];
const squares = array.map( (item) => {
   return item * item;
});
console.log(squares); // 1, 4, 9, 16, 25
```
То есть, мы идем по старому массиву (`array`), и дла каждого элемента применяем функцию-колбэк, которую передали в аргументе `map`. Результатом колбэка будет элемент нового (результирующего) массива.

Закрепим практикой.
1. Из массива `numbers` получить новый массив, который будет состоять из удвоенных элементов оригинального массива.
2. Из массива строк получить массив чисел, который будет содержать длины строк.
3. Из массива строк получить новый массив строк, который будет содержать строки элемены оригинального массива в верхнем регистре.
4. Напишем свою реализацию метода `map`.

> filter - поиск элементов в массиве. Он "фильтрует" массив, и возвращает НОВЫЙ массив, в который попадут отфильтрованные элементы первого массива.

Пример:
```
const arr = [1, 2, 3, 4, 5];
const even = arr.filter( (item) => {
    return item % 2 === 0;
});
console.log(even); // [2, 4]
```

И также закрепляем практикой.
1. Получить из массива `numbers` массив чисел, кратных 3.
2. Получить из массива `strings` массив строк с длиной больше 4 символов.
3. Получить из массива `strings` массив тех строк, которые содержат букву 'a'.
4. Написать свою реализацию `filter`.

> sort - сортировка массива. Это МУТИРУЮЩИЙ метод, то есть он сортирует элементы "на месте". Принимает колбэк-функцию, которая принимает два аргумента (элементы массива), и определяет, в каком порядке они должны быть. Если `a` должен быть перед `b` - то функция должна вернуть положительное число (например, 1), иначе должна вернуть отрицательное (например -1). Если возвращает 0 - то `a` идет перед `b`. Если не передать в `sort` колбэк, то элементы массива будут приведены к строкам, и отсортированы в алфавитном порядке.

Пример:
```
const arr = [1, 6, 3, 2, 10, 5];
arr.sort( (a, b) => {
  return a - b;
});
// arr === [1, 2, 3, 5, 6, 10];
```

Задачи:
1. Отстортировать массив чисел по убыванию.
2. Отсортировать массив строк по возрастанию кол-ва символов в строке.
3. Отстортировать массив чисел по возрастанию остатка от деления на 5.

> reduce - популярный метод "сворачивания" массива. Он делает из массива результирующий элемент, некую "агрегацию" массива. Этот результат может быть чем угодно: числом, строкой, объектом, другим массивом - чем угодно. Принимает 2 аргумента: првый - это функция-колбэк, второй - начальное значение (чем мы инициализируем наш результат). Колбэк принимает первым аргументом "аккумулятор" (результат предыдущей итерации), а вторым - текущий элемент массива. Задача колбэка - произвести какие-то действия над текущим элементом, возможно добавить его в аккумулятор, и вернуть аккумулятор. На первой итерации, аккумулятор инициализируется начальным значением (второй аргумент `reduce`)

Пример: посчитаем сумму элементов массива методом `reduce`
```
const arr = [1,2,3,4,5];
const result = arr.reduce( (accum, value) => {
  return accum + value; // на каждой итерации, мы прибавляем текущий элемент к результату предыдущей итерации.
}, 0); // инициализируем наш аккумулятор начальным значением - нулем
```

Задачи:
1. Посчитать общее количество символов в строках массива `strings`, с использованием метода `reduce`.
2. Посчитать среднее арифметическое элементов массива `numbers`
3. Реализовать метод `map` через `reduce`.