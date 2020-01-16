---
title: getElementById vs querySelector
date: 2020-01-16 10:05:28 -0400
categories: jekyll update
---

# getElementById vs querySelector 성능 차이

`getElementById`의 경우 Element의 property인 id의 값만을 이용하여 Element를 찾는 방법이다.

`querySelector`의 경우는 id, tag, classname을 사용하여 Element를 가지고 온다.

두가지 방법은 사용법은 아래와 같다.

``` javascript
var el = document.getElementById('testElement');

var el = document.querySelector('#testElement');
```

성능의 benchmark의 사이트를 통해 측정을 해본다.

https://www.measurethat.net/

두개를 직접 측정해 보면 두배 정도로 해서 `getElementById`가 성능상으로 빠르다.

아래는 측정한 결과값이다.

관련 내용은 아래의 링크를 클릭해도 확인 할 수 있다.

[https://www.measurethat.net/Benchmarks/ShowResult/89560](https://www.measurethat.net/Benchmarks/ShowResult/89560)

그렇기 때문에 element에 id가 있다면 `getElementById`를 사용하여 element를 가지고 오는 것이 쫗다.