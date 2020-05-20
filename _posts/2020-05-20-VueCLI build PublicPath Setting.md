---
title: Vue-CLI에서 Build시 PublicPath 설정
date: 2020-05-20 02:40:28 -0400
categories: jekyll update
---

## 변경해야 할 경우

Vue로 개발을 할 경우 build를 히게되면 `/dist`폴더안에 파일이 생성 된다.

/css, /js, /img, index.html이 생성된다.

여기서 python을 사용하거나, 다른 언어를 사용하여 static파일을 간단히 웹서버로 올리면 에러가 발생하지 않는다.

하지만 배포를 할 경우 특정 폴더을 메인으로 할 경우 상대경로에 문제가 생겨 에러가 발생하고 페이지가 열리지 않는다.

이러한 문제를 해결하기 위해서는 아래와 같이 PublicPath를 변경 해야 한다.

Vue Cli의 경우에는 vue.config.js 파일에 추가하면 된다.

```javascript
module.exports = {
  publicPath: process.env.NODE_ENV === "production" ? "./" : "",
};
```

## 참고

- https://cli.vuejs.org/config/#publicpath
