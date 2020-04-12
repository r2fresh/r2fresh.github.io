---
title: PostCSS 기본 설정
date: 2020-04-08 09:30:28 -0400
categories: jekyll update
---

### PostCSS 설치

VueCli에서는 Sass의 설치 부분 중 마지막으로 보면 PostCSS, Autoprefixer, CSS Modules은 기본으로 제공 된다.   

아래의 문서 링크에서 PostCSS는 기본으로 제공 된다고 쓰여 있다.

[https://cli.vuejs.org/guide/css.html#postcss](https://cli.vuejs.org/guide/css.html#postcss)

### postcss option 추가

postcss-loader 관련 기본 옵션을 `vue.config.js`에 추가한다.

**vue.config.js**
``` javascript
module.exports = {
  css: {
    loaderOptions: {
      postcss: {
        ident: 'postcss',
        plugins: [],
      }
    }
  }
}
```

그리고 프로젝트 root경로에 `postcss.config.js`을 추가하고 아래와 같이 입력한다.

**postcss.config.js**
``` javascript
module.exports = {
  plugins: [],
};
```