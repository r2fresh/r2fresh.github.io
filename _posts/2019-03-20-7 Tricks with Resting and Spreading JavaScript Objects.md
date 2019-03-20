---
title: "7 Tricks with Resting and Spreading JavaScript Objects[한글번역]"
date: 2019-03-18 08:26:28 -0400
categories: jekyll update
---

**원본 : [https://blog.bitsrc.io/6-tricks-with-resting-and-spreading-javascript-objects-68d585bdc83](https://blog.bitsrc.io/6-tricks-with-resting-and-spreading-javascript-objects-68d585bdc83)**

## Adding Properties

다음은 resting을 사용하여 객체의 원소를 다른 객체의 원소에 복사하는 예제이다.
shallow clone으로 user의 원소가 추가 되거나 삭제 되도 userWithPass의 원소는 변하지 않는다.

``` javascript
const user = { id: 100, name: 'Howard Moon'}
const userWithPass = { ...user, password: 'Password!' }

user //=> { id: 100, name: 'Howard Moon' }
userWithPass //=> { id: 100, name: 'Howard Moon', password: 'Password!' }
```

## Merge Objects
resting을 이용하여 두개의 객체를 merge하여 새로운 객체를 만드는 예제이다.
원소의 key가 같은 것은 마지막 객체의 값으로 지정된다.

``` javascript
const part1 = { id: 100, name: 'Howard Moon' }
const part2 = { id: 100, password: 'Password!' }

const user1 = { ...part1, ...part2 }
//=> { id: 100, name: 'Howard Moon', password: 'Password!' }
```
아래의 방법으로도 가능하다.
```javascript
const partial = { id: 100, name: 'Howard Moon' }
const user = { ...partial, id: 100, password: 'Password!' }

user //=> { id: 100, name: 'Howard Moon', password: 'Password!' }
```

## Exclude Object Properties

rest를 사용하면 입력 된 객체에서 rest 이전에 지정된 key의 이름으로 된 것 것을 입력 객체에서 제거하여 rest의 객체에 입력할 수 있다.

```javascript
const noPassword = ({ password, ...rest }) => rest
const user = {
  id: 100,
  name: 'Howard Moon',
  password: 'Password!'
}

noPassword(user) //=> { id: 100, name: 'Howard moon' }
```

## Dynamically Exclude Properties

computed object property names를 사용하여 객체의 특정 원소를 제외 할 수 있는 복사본을 만들수 있다.

```javascript
const user1 = {
  id: 100,
  name: 'Howard Moon',
  password: 'Password!'
}
const removeProperty = prop => ({ [prop]: _, ...rest }) => rest
//                     ----       ------
//                          \   /
//                dynamic destructuring

const removePassword = removeProperty('password')
const removeId = removeProperty('id')

removePassword(user1) //=> { id: 100, name: 'Howard Moon' }
removeId(user1) //=> { name: 'Howard Moon', password: 'Password!' }
```

## Organize Properties

가끔 속성들은 필요한 순서대로 되지 않습니다. 몇 가지 트릭을 사용하여 속성을 목록 맨 위로 밀어 넣거나 맨 아래로 이동 할 수 있습니다.
아래의 예제는 `id`를 첫번째 위치로 이동하기 위해 `id:undefined`를 사용한 예제입니다.

```javascript
const user3 = {
  password: 'Password!',
  name: 'Naboo',
  id: 300
}

const organize = object => ({ id: undefined, ...object })
//                            -------------
//                          /
//  move id to the first property

organize(user3)
//=> { id: 300, password: 'Password!', name: 'Naboo' }
```

다음으로 `password`를 맨 마지막으로 이동하는 방법은 우선 `password`를 삭제하고 다시 `password`를 추가하는 방법을 사용하면 됩니다.

``` javascript
const user3 = {
  password: 'Password!',
  name: 'Naboo',
  id: 300
}

const organize = ({ password, ...object }) => ({ ...object, password })
//              --------
//             /
// move password to last property

organize(user3)
//=> { name: 'Naboo', id: 300, password: 'Password!' }
```

## Default Properties

기본 속성은 원래 개체에 포함되지 않은 경우에만 설정되는 값입니다.

아래의 예제에서는 `user2`는 `quotes`가 포함되지 않습니다. `setDefaults` 함수는 모든 객체에 `quetes`가 설정 되도록 합니다.
그렇지 않으면 `[]`로 설정 됩니다.

`setDefaults(user2)` 호출 할 때 반환 값에는 `quotes:[]'가 포함 됩니다.
`setDefaults(user4)` 호출 하면 `user4`는 이미 quotes가 있기 때문에 해당 속성은 수정되지 않습니다.

``` javascript
const user2 = {
  id: 200,
  name: 'Vince Noir'
}

const user4 = {
  id: 400,
  name: 'Bollo',
  quotes: ["I've got a bad feeling about this..."]
}

const setDefaults = ({ quotes = [], ...object}) =>
  ({ ...object, quotes })

setDefaults(user2)
//=> { id: 200, name: 'Vince Noir', quotes: [] }

setDefaults(user4)
//=> {
//=>   id: 400,
//=>   name: 'Bollo',
//=>   quotes: ["I've got a bad feeling about this..."]
//=> }
```

기본 값을 마지막이 아닌 첫번째로 나오게 기본으로 정하기 위해서는 아래와 같이 코드를 변경하면 된다.
``` javascript
const setDefaults = ({ ...object}) => ({ quotes: [], ...object })
```

## Renaming Properties

위의 기술을 결합하여 속성의 key명을 바꾸는 함수를 만들 수 있습니다.
아래는 `ID`의 key 이름을 소문자 `id`로 변경하는 방법에 대한 예제입니다.

``` javascript
const renamed = ({ ID, ...object }) => ({ id: ID, ...object })

const user = {
  ID: 500,
  name: "Bob Fossil"
}

renamed(user) //=> { id: 500, name: 'Bob Fossil' }
```

## Bonus~ Add conditional properties
아래는 조건부를 사용하여 속성에 해당하는 값이 있을 때에만 속성을 추가하는 예제입니다.

``` javascript
const user = { id: 100, name: 'Howard Moon' }
const password = 'Password!'
const userWithPassword = {
  ...user,
  id: 100,
  ...(password && { password })
}

userWithPassword //=> { id: 100, name: 'Howard Moon', password: 'Password!' }
```

## Summary
혹시 알려지지 않은 spread와 resting을 사용하는 방법이 있다면 공유하자