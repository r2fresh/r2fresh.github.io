---
title: 'JavaScript Promise'
date: 2019-11-04 11:37:28 -0400
categories: jekyll update
---

> 원본 블로그 : [https://medium.com/dailyjs/javascript-promises-zero-to-hero-plus-cheat-sheet-64d75051cffa](https://medium.com/dailyjs/javascript-promises-zero-to-hero-plus-cheat-sheet-64d75051cffa)

# JavaScript Promises: Zero To Hero Plus Cheat Sheet

async / await는 비동기 JavaScript롤 새롭게 도입한 것이지만 이것은 Promises이 기반으로 되어 있습니다.

그렇기 때문에 Promises를 이해하는 것이 중요 합니다.

혹시 Async/Awaitdp eogo 자세히 알려는 분은 아래의 링크를 참고 하시기 바랍니다.

[https://medium.com/dailyjs/javascript-async-await-zero-to-hero-plus-cheat-sheet-4b064401e29a](https://medium.com/dailyjs/javascript-async-await-zero-to-hero-plus-cheat-sheet-4b064401e29a)

## What The Heck Is A Promise

Promise는 비동기로 실행이되도록 되어있지만 아직 실행되지 않은 것이라 말 할 수 있다.

아래는 기본 스펙 코드 입니다.

``` javascript
const flipACoin = new Promise((resolve, reject) => {    

});
```

Promise는 단일 인수(executor 함수)를 가진 클래스 입니다. executor function는 resolve와 reject함수라는 두개의 인수를 가지며 executor내부 코드가 실행되고 문제가 없으면 resolve가 실행되고 문제가 발생하면 rejecf함수가 실행된다.

아래는 위의 구조에 executor가 들어간 코드 입니다.

``` javascript
const flipACoin = new Promise((resolve, reject) => {    
  const flipResult = flip(); //let's say flip() takes a few seconds
  
  if(flipResult) {
    resolve();
  } else {
    reject();
  }
});
```

위의 코드에서 flipACoin을 완료하는데는 약간의 시간이 걸립니다. Coin을 flip()는 것은 몇초가 걸립니다.
flip이 완료되서 성공하면 flipResult는 true를 하고 resolve()함수가 실행됩니다.
만약에 flipResult가 Undefined이면 reject()함수가 실행됩니다.

이게 Promise의 기본입니다.

그렇다면 executor function는 내부에서 언제 실행이 되는 것일까요 그것은 바로이면 나중에 실행되는 것은 나중에 서명하겠습니다.

## Interacting With Surrounding Code

Promise의 기본 실해 방법은 보았으니 주변 Context를 살펴 보겠습니다. Promise를 사용하는 가장 중요한 측면 중 하나는 Promise를 만들더라도 주변 코드가 계속 실행된다는 것입니다.

예를 들어 아래의 코드를 본다면..

```javascript
4console.log("I'm about to flip a coin!");

const flipACoin = new Promise((resolve, reject) => {
  console.log("I'm flipping the coin!");
  
  const flipResult = flip(); //let's say flip() takes a few seconds
  
  if(flipResult) {
    console.log("Here is the coin flip result!", flipResult);
    resolve();
  } else {
    reject();
  }
});

console.log("I have flipped the coin.")
```

console에는 아래와 같이 찍힙니다.

1. “I’m about to flip a coin!”
2. “I’m flipping the coin!”
3. “I have flipped the coin.”
4. “Here is the coin flip result! Heads”

위와 같이 되는 이유는 Promise의 비동기 작업이 되어 있으면 완료 되기까지 기다리지 않고 주변 context의 코드가 실행이 됩니다.

그럼 우리는 resolved가 되고 주변이 실행되도록하려면 어떻게 해야 할까요?

## Waiting For A Promise To Finish

Promise의 완료에 대한 대답에서 코드를 실행하기 위해 Promise에서는 다양한 함수를 호출 할 수 있습니다. 
그 중에 첫번째로 `.then()`입니다.

```javascript
onsole.log("I'm about to flip a coin!");

const flipACoin = new Promise((resolve, reject) => {
  console.log("I'm flipping the coin!");
  
  const flipResult = flip(); //let's say flip() takes a few seconds
  
  if(flipResult) {
    console.log("Here is the coin flip result!", flipResult);
    resolve();
  } else {
    reject();
  }
}).then(() => {
  console.log("I have flipped the coin.")
});
```
`.then()`는 resolve()함수가 호출되고 실행이 되고 나면 실행이 됩니다.
아래와 같이 console에 로그가 찍힙니다.

1. “I’m about to flip a coin!”
2. “I’m flipping the coin!”
3. “Here is the coin flip result! Heads”
4. “I have flipped the coin.”

이제는 resolve()가 끝날때까지 기다리고 있습니다.

아래는 사용할 수 있는 모든 Promise의 함수 입니다.

```javascript
const flipACoin = new Promise((resolve, reject) => {
  resolve();
}).then(() => {
  //use `.then()` to do something after `resolve()` has been called
}).catch(() => {
  //use `.catch()` to do something after `reject()` has been called
}).finally(() => {
  //use `.finally()` to do something either way
});
```

## Why Promises? Why Not Callbacks?

callback 지옥이라는 말 들어 봤을 겁니다. 비동기 결과에 따라 계속적으로 비동기 함수를 호출하게 되면 아래와 같은 코드가 만들어 집니다.

``` javascript
callUserApiEndpoint(userId, (data) => {
  const authorId = data.authorId;
  
  callPostsEndpoint(authorId, (data) => {
    const postsByUser = data.posts;
    const topPost = postsByUser[0];
    
    callCommentsEndpoint(topPost.id, (data) => {
      const comments = data.comments;
      const topComment = comments[0];
      
      callResponsesEndpoint(topComment.id, (data) => {
        const responses = data.responses;
        //...
      });
    });
  })
});
```

나쁘지 않지만, 오류를 찾기 힘들고, 가독성도 안좋으며 유지 관리가 어렵습니다.

## Returning Values Out Of Promises

Promise는 함수가 실행되서 결과가 오는 것을 기다려서 resolve함수를 실행하고 완료 후 .then()를 실행하는 것이 유융합니다.
하지만 이것보다는 주변에 Promise의 값이 무엇인지를 reolve와 .then()를 사용하여 알리는 것이 더 중요합니다.
그러기 위해서는 아래의 두가지 해야 합니다.

1. resolve() 함수를 실행시 값을 인수로 가져와야 합니다.
2. .then() 함수를 실행 시 단일 인수(타입이 함수)에 매개변수로 가져와야 합니다.

``` javascript
const flipACoin = new Promise((resolve, reject) => {
  const flipResult = flip(); //let's say flip() takes a few seconds
  
  if(flipResult) {
    resolve(flipResult); // 인수로 가져오는 것이 중요
  } else {
    reject();
  }
}).then((flipResult) => { // 매개변수로 가져오는 것이 중요
  console.log(`The result was ${flipResult}`);
});
```

## Using A Promise Later

Promise를 만들때 `.then()`과 `.catch()`를 모두 추가해야 한다고 생각할 수 있지만, 나중에 추가도 할 수 있습니다.
코드는 아래와 같습니다.

``` javascript
const flipACoin = new Promise((resolve, reject) => {
  const flipResult = flip(); //let's say flip() takes a few seconds
  
  if(flipResult) {
    resolve(flipResult);
  } else {
    reject();
  }
});

//...
//do other things
//...

console.log("Wait did I flip the coin?");

flipACoin.then((flipResult) => {
  console.log("Oh I did!");
});

console.log("Double checking...");

flipACoin.then((flipResult) => {
  console.log("Okay I definitely did!");
});
```

Promise를 만들어 변수에 저장한 후에도 내부코드가 오래전에 완료 된 후에도 `.then()` 및 `.catch()` 호출을 계속 추가 할 수 있습니다.

## Chaining Promises
 
Promise의 특징은 아래와 같이 chain을 할 수 있다는 것입니다.

``` javascript
const flipACoin = new Promise((resolve, reject) => {
  resolve();
}).then(() => {
  doSomething();
}).then(() => {
  doSomethingElse();
}).then(() => {
  doAnotherThing();
});
```

여기서는 의존할 것을 계속 추가 할 수 있기 때문에 유지 관리가 쉽고 가독성도 좋습니다.

그러나 여기서 doSomethingElse()는 doSomething()이 완료 되기를 기다리는 것이 아닙니다.

chain의 모든 `.then()`은 이전의 `.then()`이 아닌 마지막 Promise를 기다립니다.

doSomething()가 실행에 오래걸리면, doSomethingElse가 먼저 실행 할수도 있습니다.

이렇게 마지막 Promise의 완료를 기다리는 것을 고치느 쉬운 방법이 있습니다.

``` javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve();
  }, 2000);
}).then((result) => {
  document.getElementById("then1").innerHTML = "Done!";
}).then((result) => {
  setTimeout(() => {
        document.getElementById("then2").innerHTML = "Done!";
  }, 5000);
}).then(() => {
  document.getElementById("then3").innerHTML = "Done!";
});
```

`.then()` `.catch()` `.finally`는 호출 체인의 마지막 규칙이므로 `.then()`을 대기시키기 위해 아래오 같은 작업을 수행해야 합니다.

``` javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve();
  }, 2000);
}).then((result) => {
  document.getElementById("then1").innerHTML = "Done!";
}).then((result) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      document.getElementById("then2").innerHTML = "Done!";
      resolve();
    }, 3000);
  });
}).then(() => {
  document.getElementById("then3").innerHTML = "Done!";
});
```
`.then` 블록에 Promise를 반환하면 .then()은 Promise 대신 대기 합니다.

## Making A Promise Start Later

때때로 Promise를 변수에 넣고 만들지만 코드는 실제로 실행되지 않기를 바랄 수 도 있습니다.

이럴때는 helper 함수를 사용하면 됩니다.

``` javascript
const doThisLater = () => {
  return new Promise((resolve, reject) => {
    console.log("Done!");
    resolve();
  });
};

console.log("After promise-creating function");

doThisLater();
```

위의 코드를 실행하면 아래와 같은 순서로 console에 로그가 남습니다,

1. “After promise-creating function”
2. “Done!”

helper 함수를 사용하여 Promise를 리턴해서 사용은 할 수 있지만 다른 시간의 코드까지는 실행 할 수 없습니다.

``` javascript
const timeout = (millisecondsToWait, onTimeoutComplete) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      onTimeoutComplete();
      resolve();
    }, millisecondsToWait)
  });
};

timeout(3000, () => {
  console.log("Timeout finished!");
}).then(() => {
  console.log("Promise resolved!");
});
```

이 helper 함수의 기능을 조금 변경하여 사용하면 Promise의 매개변수를 넘겨 사용할 수 도 있습니다.
이 부분은 잘 사용하면 좋은 부분입니다.

## Static Helpers

마지막으로 Promise 클래스에는 몇 가지 편리한 정적 함수가 제공됩니다. 
아래는 gist에서 발취한 내용이며, 사용방법은 cheat sheet를 사용할 것입니다.

``` javascript
const promise1 = new Promise((resolve, reject) => resolve("Resolved promise1!"));
const promise2 = new Promise((resolve, reject) => reject("Rejected promise2!"));
const promise3 = new Promise((resolve, reject) => resolve("Resolved promise3!"));
const allPromises = [promise1, promise2, promise3];

Promise.all(allPromises).then((results) => {
  // if all promises in the collection resolves, `results` is an array of [promise1 result, promise2 result,promise3 result]
  // 컬렉션의 모든 Promise가 해결되면, results는 [promise1 result, promise2 result,promise3 result]가 됩니다.
}).catch((error) => {
  // if any promise is rejected, `error` will have the rejected value of the promise that failed
  // 어떤 Promise가 거부되면 error는 실패한 Promise의 value를 거부할 것입니다.
  // if multiple failed, `error` will be the error of the first one that failed
  // 여러번 실패하면 `error`는 첫번째 실패한 오류입니다.
});

Promise.allSettled(allPromises).then((results) => {
  //waits until all promises either resolved or rejected
  // 모드 Promise가 resolve되거나 reject되기를 기다립니다.
  //`results` will be an array of [promise1 result, promise2 result, promise3 result]
  //'results'는 [promise1 result, promise2 result, promise3 result] 배열로 나옵니다.
});

Promise.race(allPromises).then((value) => {
  //waits until first promise in array is resolved
  //배열의 첫번째 Promise가 resolved 될때까지 기다립니다.
  //`value` will be the resolved value of the first promise resolved
  //`value`는 첫번째 Promise의 resolved의 값일 것 입니다.
}).catch((value) => {
  //waits until first promise in array is rejected
  // 배열의 첫번째 promise가 rejected 될때 까지 기다립니다.
  //`value` will be the rejected value of the first promise rejected
  // `value`는 첫번째 proimse의 rejected의 값일 것입니다.
});

// Promise를 즉시 resolve
Promise.resolve(); //makes a promise that instantly resolves

// Promise를 즉시 reject
Promise.reject(); //makes a promise that instantly rejects
```

Promise.all()과 Promise.allSettled()가 가장 많이 사용됩니다.
Promise.allSettled()는 지원하는 브라우저가 적습니다.

## Codepen Cheat Sheet

위에 사용된 기술을 보여는 예제입니다.

#### Then, Catch, And Finally
[https://codepen.io/jksaunders/pen/XWrweqd](https://codepen.io/jksaunders/pen/XWrweqd)

#### Passing Values Through Chains
[https://codepen.io/jksaunders/pen/QWLRqVX](https://codepen.io/jksaunders/pen/QWLRqVX)

#### Waiting For Promises Mid-Chain
[https://codepen.io/jksaunders/pen/RwbmLvJ](https://codepen.io/jksaunders/pen/RwbmLvJ)

#### Managing Multiple Promises
[https://codepen.io/jksaunders/pen/Rwbmjaq](https://codepen.io/jksaunders/pen/Rwbmjaq)

#### Data Fetching Example
[https://codepen.io/jksaunders/pen/dyybZaJ](https://codepen.io/jksaunders/pen/dyybZaJ)

#### React Example
[https://codepen.io/jksaunders/pen/aboMZoK](https://codepen.io/jksaunders/pen/aboMZoK)

## Async/Await

JavaScript Promise를 어느 정도 봤다면, Async/Await를 도면 된다.
Promise를 이해 했다면 80%는 이해했다고 생각해도 좋다

[https://medium.com/dailyjs/javascript-async-await-zero-to-hero-plus-cheat-sheet-4b064401e29a](https://medium.com/dailyjs/javascript-async-await-zero-to-hero-plus-cheat-sheet-4b064401e29a)
