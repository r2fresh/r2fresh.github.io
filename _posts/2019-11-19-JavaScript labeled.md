---
title: JavaScript labeled statements
date: 2019-11-19 09:30:28 -0400
categories: jekyll update
---

# labeled statements

JavaScript에서 가끔 사용 되지만 알고 있으면 유용한 기능 Labeled statements

> 원본 : [https://flaviocopes.com/javascript-labeled-statements/](https://flaviocopes.com/javascript-labeled-statements/)

JavaScript에는 label을 지정할 수 있는 비교적 알려지지 않은 기능이 있습니다.
최근에 Svelte에서 강력한 reactive declarations을 사용하는데 이 기능을 보았다.
아래의 선언문은 변수가 변경될때 다시 계산이 됩니다.

``` javascript
$: console.log(variable)
```

당신은 statement 정의를 할수 있는 블록을 정의하는 자바스크립트의 다른 기능 statement block 사용을 또한 허락한다.

``` javascript
$: {
  console.log(variable)
  console.log('another thing')
  //...
}
```

이상하게 보일수도 있지만 올바른 JavaScript 입니다.
이 명령문은 $ 레이블에 지정됩니다.

Svelte 컴파일러는 내부적으로 이를 사용하여 반응형 선언을 강화합니다.
다른 곳에 이 기능을 사용한 적은 없지만, 아직, loop와 switch에서 중단을 할때 사용을 합니다.
아래는 하나의 예제입니다.

``` javascript
for (let y = 0; y < 3; y++) {
  switch (y) {
    case 0:
      console.log(0)
      break
    case 1:
      console.log(1)
      break
    case 2:
      console.log(2)
      break
  }
}
```
위는 `0 1 2`가 console.log에 찍힙니다.

switch case문에서 break를 원할때는 아래와 같이 합니다.

``` javascript
loop: for (let y = 0; y < 3; y++) {
  switch (y) {
    case 0:
      console.log(0)
      break
    case 1:
      console.log(1)
      break loop
    case 2:
      console.log(2)
      break
  }
}
```

위는 `0 1`이 찍힙니다.

관련 된 소스코드 [링크](https://github.com/flaviocopes/website-content/blob/content/post/javascript/javascript-labeled-statements/index.md)입니다.