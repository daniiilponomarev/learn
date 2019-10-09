---
title: Функции в JavaScript
templateKey: 'article-page'
order: 1
tags: js, JavaScript, function, closure, scope
---

# Функции в JavaScript

## Определение функции

**Функция** – это блок программного кода, который̆ определяется один раз и может выполняться, или вызываться, многократно.

Функция в JS – объект, который может быть вызван как подпрограмма.

В дополнение к свои свойствам, функция содержит исполняемый код и определенное состояние, которые определяют поведение функции при вызове.

Функция в JS - **функциональный объект**!

## Синтаксис

Классическое объявление функции называется **Function Declaration**. Пример:

```javascript
function sum(a, b) {
  return a + b;
}
```

Другой подход к объявлению функций - **Function Expression**. Пример:

```javascript
var sum = function(a, b) {
  return a + b;
};
```

Основное отличие между ними: функции, объявленные как Function Declaration, создаются интерпретатором до выполнения кода, поэтому их можно вызвать до объявления.

[См. о функциональных выражениях](https://learn.javascript.ru/function-declaration-expression).

## Параметры и аргументы функции

**Параметры функции** - список идентификаторов, заданный в момент объявления, т.е. в примерах параметрами являются a и b.

При вызове функции ей передается список значений, называемых **аргументами функции**. Например, 1 и 2 являются аргументами функции sum при вызове:

```javascript
sum(1, 2);
```

В JS нет требования, чтобы количество переданных аргументов совпадало с количеством параметров.

Если передано больше аргументов, чем объявлено параметров, то лишние переданные значения никак не повлияют на выполнение функции.

Если передано меньше аргументов, чем объявлено параметров, параметры для которых не было передано аргументов получат значение undefined. Пример:

```javascript
var sum = function(a, b) {
  console.log(a, b);
};

sum(1); // 1 undefined
```

У каждой функции есть read-only свойство length, которое определяется в момент создания функции и содержит число параметров функции.

```javascript
var sum = function(a, b, c) {
  return a + b + c;
};

sum.length; // 3
```

**Арность** (arity) – количество параметров у функции

Для доступа к аргементам может также использоваться псевдомассив (объект, у которого числовые имена(ключи) свойств и есть свойство `length`) аргументов "arguments".

Он содержит список аргументов по номерам: `arguments[0]`, `arguments[1]` и т.д., а также свойство `length`. [Подробнее](https://learn.javascript.ru/arguments-pseudoarray).

## Функция и процедура

И процедура, и функция – набор инструкций (подпрограмма), который может быть выполнен в определенном порядке. Отличие заключается в том, что функция всегда должна возвращать значение (более строгое определение требует также чтобы у функции всегда были входные данные).

Все функциональные объекты в JS – это именно функции.

По умолчание возвращаемое значение - `undefined`.

```javascript
var sum = function(a, b) {
  a + b;
};
console.log(sum(1, 2)); // undefined
```

Для явного указания значение, которое должно вернуться из функции используется инструкция `return`. Если выражение после ключевого слова `return` не указано, то функция возвращает `undefined`.

При первой встреченной инструкции `return` функция прекращает свое выполнение и возвращает указанное значение.

```javascript
var sum = function(a, b) {
  return a + b;
  console.log(100); // не вызовется никогда
};
```

В JS все функции являются **объектами первого класса** (first-class citizen or object) – они могут быть переданы как аргумент, возвращены из функции и присвоены переменной.

Пример:

```javascript
var sum = function(a, b) {
  return function(c) {
    return a + c;
  };
};
var newSum = sum(2);
newSum(2); // 4
```

## Область видимости

Чтобы однозначно определить с какой переменной связан идентификатор и какое значение он будет возвращать при обращении к нему в языках программирования существуют области видимости переменных (scope).

Концепция области видимости дает нам возможность в одной и той же программе использовать повторяющиеся идентификаторы(имена) переменных и при этом не создавать конфликты.

Область видимости – область, где переменная ассоциируется с определенным значением.

```javascript
var a = 10;

var f = function() {
  var a = 100;
  console.log(a); // ?
};

f(); // 100
```

Важно отметить, что при использовании `var` блоки ( {...код…} ) в JS не создают области видимости:

```javascript
var x = 10;

if (true) {
  var x = 20;
  console.log(x); // 20
}

console.log(x); // 20
```

При использовании объявления переменной через ключевое слово `var` в JS область видимости создают только функции!

```javascript
var a = 10;

var f = function() {
  var b = 100;
  console.log(b);
};

f(); // 100
console.log(a); // 10
console.log(b); // Uncaught ReferenceError: b is not defined
```

В примере переменная `a` – глобальная, доступна(видна) в любом месте программы, а переменная `b` - локальная для функции f и доступна(видна) только внутри функции и недоступна снаружи функции.

В JS функция имеет доступ к области видимости, в которой она была **создана**.

Переменная ищется сначала в собственной области видимости, а затем, если не была найден ищется во внешней области видимости. Этот процесс называется разрешением идентификатора - поиск переменной по областям видимости.

Глубина вложенности областей видимости может быть произвольной и поиск переменной будет продолжаться до самого верха.

```javascript
var a = 10;

var f = function() {
  var b = 100;
  var g = function() {
    console.log(b);
    console.log(a);
  };
  g();
};
f(); // 100 10
```

В JS область видимости является лексической (статической) – lexical scope. Доступ к внешним переменным определяется в **момент создания функции**.

```javascript
var a = 10;
var f = function() {
  console.log(a);
};
var g = function() {
  var a = 100;
  f();
};
g(); // 10
```

```javascript
var a = 10;

var f = function() {
  var b = 100;
  g();
};

var g = function() {
  console.log(b);
  console.log(a);
};

f(); // ReferenceError: b is not defined
```

## Замыкание

```javascript
var a = 10;

var f = function() {
  console.log(a);
};
f(); // 10
```

Переменная `a` внутри функции `f` называется нелокальной или свободной, так как не находится в области видимости самой функции, а берется из внешней области видимости.

**Замыкание** (closure) - это комбинация функции и всех лексически сохраненных родительских областей видимости. При этом, посредством этих сохраненных объектов областей видимости, функция может использовать свободные переменные.

**Замыкание** – это функция вместе со всеми внешними переменными, которые ей доступны.

Теоретически, все функции в JS являются замыканиями.

При **создании** функция получает скрытое свойство `[[Environment]]`, которое ссылается на область видимости, в котором она была создана. При вызове функции, куда бы её ни передали в коде – она будет искать переменные сначала у себя, а затем во внешних областях видимости, которые берутся из `[[Environment]]`.

```javascript
var f = function() {
  var a = 10;
  return function() {
    console.log(a);
  };
};
var g = f();
g(); // 10
```

[См. подробнее о замыканиях](https://learn.javascript.ru/functions-closures).