---
title: Understanding the Delete Operator in JavaScript
date: 2019-11-19 15:42:28 -0400
categories: jekyll update
---

## The delete operator

ECMA 스펙에 따라서 

Delete(O,P)

이것은 객체에서 특정 속성을 제거하는 데 사용됩니다. 
만약 속성이 구성되지 않았으면 제외됩니다.

`O`는 오브젝트 `P`는 특성 키 입니다.

`delete` 연산자는 객체의 속성을 제거하거나 삭제 하는데 사용됩니다.

``` javascript
const l = console.log
let obj = {
    "d": 88
}
l(obj.d)
delete obj.d
l(obj.d)
```
결과
``` javascript
88
undefined
```

delete로 삭제하면 값만 삭제 되고 변수는 그대로 있습니다.

그렇게 되면 변수는 정의되고 아직 값이 할당되지 않은 undefined가 됩니다.

## Tip: Share and reuse utility and UI components(유틸리티 및 UI 구성요소 공유 및 재사용)

[Bit](https://bit.dev/)를 사용하여 서로 다른 JavaScript프로젝트에서 작은 개별 모듈을 쉽게 게시 설치 및 업데이트 하십시오

전체 라이브러리를 설치하지 말고 코드를 복사하여 붙여 넣지 마세요.

빌드 할때 작은 모듈을 사용하면 빠르게 할 수 있습니다.

## Non-configurable properties and delete

delete는 Object의 속성에서 `configurable`의 값이 `true`일 경우에만 삭제가 가능합니다.
`configurable`의 속성이 `false`이면 객체에서 해당 키에 해당하는 값을 삭제 할 수 없습니다.

아래의 코드를 보시면 `defineProperty`를 사용하여 `d`의 `configurable`의 값을 `false`로 만들었습니다.

이렇게 하면 삭제가 되지 않고 로그에 찍히는 것을 볼 수 있습니다.

`Object`의 `configurable`의 기본 속성은 `true`입니다.

``` javascript
let obj = {
    "d": 88
}
Object.defineProperty(obj, 'd', { configurable: false });
l(obj.d)
delete obj.d
l(obj.d)
```
결과 로그
``` javascript
88
88 
```

아래는 `getOwnPropertyDescriptor`를 사용하여 `d`의 기본 Property를 찍어 보았습니다.

``` javascript
let obj = {
    "d": 88
}
l(Object.getOwnPropertyDescriptor(obj, "d"))
$ node delete
{ value: 88,
  writable: true,
  enumerable: true,
  configurable: true }
```

## var, let, const and delete

`var`, `let`, `const`는 기본적으로 `configurable` property 값이 `false`입니다. 

그렇기 때문에 `delete`로 삭제가 불가능합니다.

아래는 관련된 예제입니다.

``` javascript
const l = console.log
var f = 90
let g = 88
const h = 77
l("before delete f: " + f)
l("before delete g: " + g)
l("before delete h: " + h)
function func() {
    let funh = 990
    delete funh
    l(funh)
    delete f
    delete g
    delete h
    l("inside func f:" + f)
    l("inside func g:" + g)
    l("inside func h:" + h)
}
func()
delete f
delete g
delete h
l("after delete f: " + f)
l("after delete g: " + g)
l("after delete h: " + h)
```
결과 로그
``` javascript
before delete f: 90
before delete g: 88
before delete h: 77
990
inside func f:90
inside func g:88
inside func h:77
after delete f: 90
after delete g: 88
after delete h: 77
```

## functions and delete

`delete`연산자로는 함수는 삭제가 불가능합니다.

``` javascript
function func() {
    l("inside func")
}
delete func
func()
```
결과
``` javascript
inside func
```

하지만 객체의 속성으로서 정의 된 함수는 삭제할 수 있습니다.
``` javascript
var obj = {
    d: function() {
        l("inside obj d function")
    }
}
obj.d()
delete obj.d
obj.d()
```
결과
``` javascript
inside obj d function
delete.js:14
obj.d()
    ^
TypeError: obj.d is not a function
```
마찬가지로 `configurable`의 속성이 `false`이면 삭제가 불가능합니다.

``` javascript
const obj = {
    d: function() {
        l("inside obj d function")
    }
}
obj.d()
Object.defineProperty(obj, "d", { configurable: false })
delete obj.d
obj.d()
```
결과
``` javascript
inside obj d function
inside obj d function
```

## delete and the global scope

`var`, `let`, `const`로 정의되지 않고 글로벌로 정의 된 변수는 어떨까요?

``` javascript
f = 90
```
이 변수의 `configurable`는 확인해 볼까요?
``` javascript
f = 90
l(Object.getOwnPropertyDescriptor(global, "f"))
```
결과
``` javascript
{ value: 90,
  writable: true,
  enumerable: true,
  configurable: true }
```
`configurable`이 `true`이기 때문에 삭제가 가능합니다.
``` javascript
f = 90
l(f)
delete f
l(f)
```
결과
``` javascript
$ node delete
90
l(f)
  ^
ReferenceError: f is not defined
```
삭제 했더니 ReferenceError가 발생합니다.

## delete and local scope

`delete`는 local scopes와 function scopes에는 영향을 미치지 않습니다.

``` javascript
{
    var g = 88
    delete g
}
l(g)
function func() {
    var funch = 90
    delete funch
    l(funch)
}
func()
```
결과
``` javascript
88
90
```

## delete and non-existent properties

`Object`에 존재하지 않는 속성을 삭제하더라도 오류는 발생하지 않으며, `true`를 리턴합니다.

``` javascript
var obj = {
    "d": 90
}
l(delete obj.f)
```
결과
``` javascript
true
```