---
title: What is Functional Programming
date: 2019-11-18 16:37:28 -0400
categories: jekyll update
---

> 원본 링크 : [https://www.sitepoint.com/what-is-functional-programming/](https://www.sitepoint.com/what-is-functional-programming/)

# The Core Principles of Functional Programming

## Pure functions (순수성)

Pure function는 입력으로 인수를 받아서 처리해서 반환값을 출력하는 것입니다.

다음 코드는 impure function(불순한) 이다.

```javascript
let number = 2;

function squareNumber() {
  number = number * number; // impure action: manipulating variable outside function
  console.log(number); // impure action: console log-ing values
  return number;
}

squareNumber();
```

아래는 순수한 함수입니다.
```javascript
// pure function
function squareNumber(number) {
  return number * number;
}

squareNumber(2);
```

pure function는 외부 변수를 사용하지 않고 독립적으로 함수 내에서 변수를 사용합니다.
외부의 영향을 받지 않고 동일한 입력에는 동일한 출력물이 나오도록 합니다.
아래는 동일한 출력물이 나오지 않아 pure function는 아닙니다.

```javascript
// Not referentially transparent
Math.random();
// 0.1406399143589343
Math.random();
// 0.26768924082159495ß
```

## Immutability (불변성)

함수형 프로그래밍은 불변하며 데이터를 직접수정하지 않습니다. 불변성은 예측 가능성으로 이어집니다.

JavaScript를 예를 들어 `.pop()`의 메소드는 직접 데이터를 수정하기 때문에 함수형 프로그래밍이 아닙니다.

하지만 `.splice()`의 경우 필요한 데이터를 복사해서 사용하기 때문에 원본데이터는 수정하지 않습니다.

여기서 `.splice()`는 함수형 프로그래밍 입니다.

```javascript
// We are mutating myArr directly
const myArr = [1, 2, 3];
myArr.pop();
// [1, 2]
```
```javascript
// We are copying the array without the last element and storing it to a variable
let myArr = [1, 2, 3];
let myNewArr = myArr.slice(0, 2);
// [1, 2]
console.log(myArr);
```

## First-class functions

함수형 프로그램에서 함수는 값으로도 사용이 가능합니다.

```javascript
let myFunctionArr = [() => 1 + 2, () => console.log("hi"), x => 3 * x];
myFunctionArr[2](2); // 6

const myFunction = anotherFunction => anotherFunction(20);
const secondFunction = x => x * 10;
myFunction(secondFunction); // 200
```

## Higher-order functions (고차함수)

고차함수는 함수를 하나 이상으로 매개변수로 사용하거나, 또는 함수를 반환하는 함수입니다.
배열에는 map, reduce, filter등의 함수가 있습니다.

여기서 filter는 조건에 맞는 값만 포함해서 새로운 배열을 반환합니다.

``` javascript
const myArr = [1, 2, 3, 4, 5];

const evens = myArr.filter(x => x % 2 === 0); // [2, 4]
```

map은 제공된 논리의 함수에 배열의 각 항목을 반복해서 사용하여 새로운 배열을 반환합니다.

``` javascript
const myArr = [1, 2, 3, 4, 5];

const doubled = myArr.map(i => i * 2); // [2, 4, 6, 8, 10]
```

reduce는 배열을 주어진 논리를 적용하여 다음 배열의 항목으로 하여 다시 저장하는 것을 하는 함수 입니다.

``` javascript
const myArr = [1, 2, 3, 4, 5];

const sum = myArr.reduce((i, runningSum) => i + runningSum); // 15
```

fiter 함수를 다음과 같이 만들수도 있습니다.
``` javascript
const filter = (arr, condition) => {
  const filteredArr = [];

  for (let i = 0; i < arr.length; i++) {
    if (condition(arr[i])) {
      filteredArr.push(arr[i]);
    }
  }

  return filteredArr;
};
```

고차함수의 두번째 유형인 함수를 리턴하는 함수 입니다.
자주 사용하는 패턴입니다.

``` javascript
const createGreeting = greeting => person => `${greeting} ${person}`

const sayHi = createGreeting("Hi")
console.log(sayHi("Ali")) // "Hi Ali"

const sayHello = createGreeting("Hello")
console.log(sayHi("Ali")) // "Hello Ali"
```
두번째 유형은 [Currying](https://www.sitepoint.com/currying-in-functional-javascript/)에서 많이 사용하는 패턴입니다.

## Function composition

더 복잡한 함수를 만들기 위해 함수를 작은 단위로 나누어서 사용하는 것입니다.

``` javascript 
const sum = arr => arr.reduce((i, runningSum) => i + runningSum);
const average = (sum, count) => sum / count;
const averageArr = arr => average(sum(arr), arr.length);
```

## Benefits

함수를 작은 모듈 단위로 나누면, 오류도 찾기 쉽고, 가독성도 높으며, 모듈끼리 사용도도 높일 수 있습니다.

## How Can You Use Functional Programming?

함수형 프로그래밍으로 모든것을 개발 할 필요는 없다. 객체형 프로그램과 조화를 이루면서 개발을 해야 한다.