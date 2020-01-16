---
title: getElementsByClassName vs querySelectorAll
date: 2020-01-16 14:49:28 -0400
categories: jekyll update
---

## getElementsByClassName vs querySelectorAll 성능 차이

`getElementsByClassName`는 className에 해당하는 Elements들을 반환하는 메소드 이다.

`querySelectorAll`의 경우는 id, tag, classname에 해당하는 Element들을 반환한는 메소드 이다.

두가지 방법은 사용법은 아래와 같다.

``` javascript
var el = document.getElementsByClassName('testElement');

var el = document.querySelectorAll('#testElement');
```

성능의 benchmark의 사이트를 통해 측정을 해본다.

https://www.measurethat.net/

두개를 직접 측정해 보면 두배 정도로 해서 `getElementsByClassName`가 성능상으로 빠르다.

아래는 측정한 결과값이다.

![getElementsByClassName vs querySelectorAll](https://raw.githubusercontent.com/r2fresh/r2fresh.github.io/master/_img/querySelectorAll_vs_%20getElementsByClassName.png)

관련 내용은 아래의 링크를 클릭해도 확인 할 수 있다.

[https://www.measurethat.net/Benchmarks/ShowResult/89762](https://www.measurethat.net/Benchmarks/ShowResult/89762)

그렇기 때문에 element에 id가 있다면 `getElementById`를 사용하여 element를 가지고 오는 것이 쫗다.

