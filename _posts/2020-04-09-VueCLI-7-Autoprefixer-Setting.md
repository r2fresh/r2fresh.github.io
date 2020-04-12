---
title: Autoprefixer Plugin 설정
date: 2020-04-09 09:30:28 -0400
categories: jekyll update
---
### Autoprefixer 정의

autoprefixer는 브라우저 벤더별로 접두사를 자동으로 붙여주는 플러그인

### Autoprefixer 설치

VueCli에서는 Sass의 설치 부분 중 마지막으로 보면 PostCSS, Autoprefixer, CSS Modules은 기본으로 제공 된다.   

아래의 문서 링크에서 PostCSS는 기본으로 제공 된다고 쓰여 있다.

[https://cli.vuejs.org/guide/css.html#postcss](https://cli.vuejs.org/guide/css.html#postcss)

### postcss plugin 추가

이제 postcss의 plugin에 tailwindcss를 추가한다.

**postcss.config.js**
``` javascript
module.exports = {
  plugins: [require('tailwindcss'), require('autoprefixer')],
};
```

**vue.config.js**
``` javascript
module.exports = {
  chainWebpack,
  css: {
    loaderOptions: {
      postcss: {
        ident: 'postcss',
        plugins: [require('tailwindcss'), require('autoprefixer')],
      },
    },
  },
};
```

### autoprefixer 실행 확인

> `autoprefixer`되는지 확인 하는 것으로  `tailwindcss`는 주석 처리

1. vue 프로젝트 안의 `src/components`안의 `HelloWorld.vue` 안의 style 태그 안에 아래의 코드 추가

``` html
<style scoped lang="scss">
// @import "tailwindcss/base";
// @import "tailwindcss/components";
// @import "tailwindcss/utilities";

::placeholder {
  color: gray;
}
</style>
```

2. `HelloWorld.vue`를 저장 후 `npm run serve`를 실행

3. 화면의 inspector(개발자도구)를 열고 style을 확인해서 아래와 같이 나오면 Autoprefixer는 잘 작동

``` html
<style type="text/css">
<!-- data-v-469af010는 서버 실행시 마다 다름 -->
[data-v-469af010]::-webkit-input-placeholder {
  color: gray;
}
[data-v-469af010]::-moz-placeholder {
  color: gray;
}
[data-v-469af010]:-ms-input-placeholder {
  color: gray;
}
[data-v-469af010]::-ms-input-placeholder {
  color: gray;
}
[data-v-469af010]::placeholder {
  color: gray;
}
</style>
```