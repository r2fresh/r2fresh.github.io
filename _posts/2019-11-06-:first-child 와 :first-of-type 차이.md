---
title: first-child 와 :first-of-type 차이
date: 2019-11-06 11:37:28 -0400
categories: jekyll update
---

# first-child 와 :first-of-type 차이

## first-child

자식의 요소 중에 가장 첫번째 요소를 반환하는 가상클래스

``` css
div p:first-child { color: red; }
```

## first-of-type

자식의 요소 중 특정 타입 엘리먼트 대서해만 카운트를 하는 가상클래스

``` css
div p:first-of-type { color: blue; }
```