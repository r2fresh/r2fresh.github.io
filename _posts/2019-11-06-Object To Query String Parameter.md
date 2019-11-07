---
title: Object To Query String Parameter
date: 2019-11-06 11:37:28 -0400
categories: jekyll update
---

# Object To Query String Parameter

## Using map and join

### ES6

arrow function을 지원하는 브라우저에서 사용

``` javascript
var queryString = Object.keys(params).map(key => key + '=' + params[key]).join('&');
```

### ES5

일반 함수를 사용

``` javascript
var queryString = Object.keys(params).map(function(key) {
    return key + '=' + params[key]
}).join('&');
```

## Using jQuery

jquery를 사용

``` javascript
var queryString = $.param(params);
```

## Using the querystring module in node

node에서 querystring을 사용

``` javascript
const querystring = require('querystring');

let queryString = querystring.stringify(params);
```

## Parameter encoding

encoding이 필요한 key와 value에서 사용

``` javascript
var queryString = Object.keys(params).map((key) => {
    return encodeURIComponent(key) + '=' + encodeURIComponent(params[key])
}).join('&');
```