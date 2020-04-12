---
title: prettier에서 quote Props 설정 방법
date: 2020-04-07 09:30:28 -0400
categories: jekyll update
---

## prettier에서 quote Props 설정 방법

vscode의 설정에서 `"editor.formatOnSave": true`를 하고 아래와 같이 prettier를 설정했을 경우,
코드가 어떻게 바뀌는지 설명

### Quote Props

> 객체 안에서 key의 따옴표 관련하여 코드 스타일을 정의
> 
> 여기서 작은 따옴표와 큰 따옴표은 하지 않는다
> 
> 작은 따옴표와 큰따옴표 설정은 "quotes"에서 설정

#### Options

**"as-needed"**

Object의 property에서 key의 name에 quote를 감싸지 않으면 안되는 문자("-")등이 있으면, 그 문자가 들어있는 key의 name만 빼고 모두 quote를 제거한다.

적용 전
``` javascript
plugins: {
    'a-a-a': {},
    'bbb': {},
    ccc: {},
},
```
적용 후
``` javascript
plugins: {
    'a-a-a': {},
    bbb: {},
    ccc: {},
},
```

아래와 같이 quote로 감싸지 않아도 되는 문자로 된 것은, 모두 quote를 제거한다.

적용 전
``` javascript
plugins: {
    'aaa': {},
    'bbb': {},
    ccc: {},
},
```
적용 후
``` javascript
plugins: {
    aaa: {},
    bbb: {},
    ccc: {},
},
```

**"consistent"**

"as-needed"와 비슷하지만 "consistent"는 Object의 property에서 key의 name에 quote를 감싸지 않으면 안되는 문자("-")등이 있으면, 
Object안의 모든 property의 key의 name에 quote를 적용한다.

적용 전
``` javascript
plugins: {
    'a-a-a': {},
    'bbb': {},
    ccc: {},
},
```
적용 후
``` javascript
plugins: {
    'a-a-a': {},
    'bbb': {},
    'ccc': {},
},
```

아래와 같이 quote로 감싸지 않아도 되는 문자로 된 것은, "as-needed"와 같이 모두 quote를 제거 한다.

적용 전
``` javascript
plugins: {
    'aaa': {},
    'bbb': {},
    ccc: {},
},
```
적용 후
``` javascript
plugins: {
    aaa: {},
    bbb: {},
    ccc: {},
},
```

**"preserve"**

"preserve"는 Object의 Syntax에만 맞다면 개발자가 입력한 대로 그대로 사용하게 된다. quote를 없애거나, 넣거나 하지 않는다.
아래처럼 quote를 꼭 써야 하는 경우에도 Syntax만 맞다면 그대로 변경이 없다.

적용 전
``` javascript
plugins: {
    'a-a-a': {},
    'bbb': {},
    ccc: {},
},
```
적용 후
``` javascript
plugins: {
    'a-a-a': {},
    'bbb': {},
    ccc: {},
},
```
또한 아래와 같아도 변화가 없다.

적용 전
``` javascript
plugins: {
    'aaa': {},
    'bbb': {},
    ccc: {},
},
```
적용 후
``` javascript
plugins: {
    'aaa': {},
    'bbb': {},
    ccc: {},
},
```