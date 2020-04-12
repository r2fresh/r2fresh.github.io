---
title: TailwindCSS 설정 및 확인
date: 2020-04-08 09:30:28 -0400
categories: jekyll update
---

### TailWindCSS 설치
postcss의 plugin 형태로 tailwindcss를 추가

우선 모듈을 먼저 다운로드 한다. 

> [install Tailwind via npm](https://tailwindcss.com/docs/installation#1-install-tailwind-via-npm)

``` bash
$> npm install tailwindcss --save-dev
//or npm i -D tailwindcss
```

tailwindcss의 config파일 생성

> [create your tailwind config file](https://tailwindcss.com/docs/installation#3-create-your-tailwind-config-file-optional)

``` bash
$> npx tailwindcss init
```

`tailwind.config.js` 파일 기본 정보 확인

``` javascript
module.exports = {
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
};
```

### postcss plugin 추가
이제 postcss의 plugin에 tailwindcss를 추가한다.

**postcss.config.js**
``` javascript
module.exports = {
  plugins: [require('tailwindcss')],
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
        plugins: [require('tailwindcss')],
      },
    },
  },
};
```

### tailwindcss 실행 확인

1. vue 프로젝트 안의 `src/components`안의 `HelloWorld.vue` 안의 style 태그 안에 아래의 코드 추가

``` html
<style scoped lang="scss">
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
</style>
```

2. template의 태그에 다음과 같이 클래스를 추가

``` html
<!-- text-purple-600를 추가 -->
<div class="hello">
    <h1 class="text-purple-600">{{ msg }}</h1>
```

3. `HelloWorld.vue`를 저장 후 `npm run serve`를 실행

4. 화면에 `Welcome to Your Vue.js App`의 텍스트 색상이 purple로 나오면 성고

