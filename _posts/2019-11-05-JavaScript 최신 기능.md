---
title: Javascript의 최신 기능
date: 2019-11-05 11:37:28 -0400
categories: jekyll update
---

# Modern JavaScript features you may have missed

[http://www.breck-mckye.com/blog/2019/10/modern-javascript-features-you-may-have-missed/](http://www.breck-mckye.com/blog/2019/10/modern-javascript-features-you-may-have-missed/)

개발에만 집중하게 되면 놓칠 수 있는 자바스크립트 최신 기능

### ES2015

**Binary and octal literals**

2진수 변경 `0b`

``` javascript
const binaryZero = 0b0;
const binaryOne  = 0b1;
const binary255  = 0b11111111;
const binaryLong = 0b111101011101101;
```

8진수 변경 `0o`

``` javascript
// Pizza toppings
const olives    = 0b0001;
const ham       = 0b0010;
const pineapple = 0b0100;
const artechoke = 0b1000;

const pizza_ham_pineapple = pineapple | ham;
const pizza_four_seasons  = olives | ham | artechoke;
```

**Number.isNaN()**

isNaN()

``` javascript
isNaN(NaN)              === true
isNaN(null)             === false
isNaN(undefined)        === true
isNaN({})               === true
isNaN('0/0')            === true
isNaN('hello')          === true
```

Number.isNaN() 

``` javascript
Number.isNaN(NaN)       === true
Number.isNaN(null)      === false
Number.isNaN(undefined) === false
Number.isNaN({})        === false
Number.isNaN('0/0')     === false
Number.isNaN('hello')   === false
```

### ES2016

**Exponent (power) operator**

Transpilers : [@babel/plugin-transform-exponentiation-operator](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-exponentiation-operator)

``` javascript
2**2 === 4
3**2 === 9
3**3 === 27
```

**Array.prototype.includes()**

Transpilers : [babel-plugin-array-includes](https://github.com/stoeffel/babel-plugin-array-includes)

``` javascript
[1, 2, 3].includes(2)    === true
[1, 2, 3].includes(true) === false
```

``` javascript
const object1 = {};
const object2 = {};

const array = [object1, 78, NaN];

array.includes(object1) === true
array.includes(object2) === false
array.includes(NaN)     === true
```

``` javascript
// positions   0  1  2  3  4
const array = [1, 1, 1, 2, 2];

array.includes(1, 2) === true
array.includes(1, 3) === false
```

### ES2017

**Shared Array Buffers & Atomics**

브라우저 프로세스간에 메모리를 직접적으로 공유하고, 경쟁 조건을 피하도록 할수 있는 기능
이것의 기능은 아래의 URL에서 참고 하도록 하자

[https://www.sitepen.com/blog/the-return-of-sharedarraybuffers-and-atomics/](https://www.sitepen.com/blog/the-return-of-sharedarraybuffers-and-atomics/)

### ES2018

Regex bonanza

**ES2018의 새로운 정규 표현식의 기능**

Lookbehind matches (match on previous chars)

``` javascript
const regex = /(?<=\$)\d+/;
const text  = 'This cost $400';
text.match(regex) === ['400']
```

``` javascript
Look ahead:  (?=abc)
Look behind: (?<=abc)

Look ahead negative:  (?!abc)
Look behind negative: (?<!abc)
```

You can name capture groups

``` javascript
const getNameParts  = /(\w+)\s+(\w+)/g;
const name          = "Weyland Smithers";
const subMatches    = getNameParts.exec(name);

subMatches[1]     === 'Weyland'
subMatches[2]     === 'Smithers'
```
``` javascript
const getNameParts  = /(?<first>\w+)\s(?<last>\w+)/g;
const name          = "Weyland Smithers";
const subMatches    = getNameParts.exec(name);

const {first, last} = subMatches.groups
first             === 'Weyland'
last              === 'Smithers'
```

### ES2019

**Array.prototype.flat() & flatMap()**

``` javascript
const multiDimensional = [
	[1, 2, 3],
    [4, 5, 6],
    [7,[8,9]]
];

multiDimensional.flat(2) === [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

``` javascript
const texts = ["Hello,", "today I", "will", "use FlatMap"];

// with a plain map
const mapped = texts.map(text => text.split(' '));
mapped === ['Hello', ['today', 'I'], 'will', ['use', 'FlatMap']];

// with flatmap
const flatMapped = texts.flatMap(text => text.split(' '));
flatMapped === ['Hello', 'today', 'I', 'will', 'use', 'FlatMap'];
```

**Unbound catches**

``` javascript
try {
  // something throws
} catch {
  // don't have to do catch(e)
}
```

**String trim methods**

``` javascript
const padded         = '          Hello world   ';
padded.trimStart() === 'Hello world   ';
padded.trimEnd()   === '          Hello world';
```