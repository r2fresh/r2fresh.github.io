---
title: prettier에서 quote Props 설정 방법
date: 2020-04-07 09:30:28 -0400
categories: jekyll update
---

## Sass
Sass는 처음 프로젝트 생성 시 Manually로 선택하면 설치가 된다.

```bash
// manually 선택
? Please pick a preset:  
  default (babel, eslint) 
❯ Manually select features 

// manually로 선택후 CSS Pre-processors 체크
? Please pick a preset: Manually select features
? Check the features needed for your project: 
 ◉ Babel
 ◯ TypeScript
 ◯ Progressive Web App (PWA) Support
 ◯ Router
 ◯ Vuex
❯◉ CSS Pre-processors
 ◉ Linter / Formatter
 ◯ Unit Testing
 ◯ E2E Testing

// dart-sass를 선택
 ? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use arrow keys)
❯ Sass/SCSS (with dart-sass) 
  Sass/SCSS (with node-sass) 
  Less 
  Stylus 
```


## PostCSS
위의 Sass의 설치 부분 중 마지막으로 보면 PostCSS, Autoprefixer, CSS Modules은 기본으로 제공 된다.   

아래의 문서 링크에서 PostCSS는 기본으로 제공 된다고 쓰여 있다.

[https://cli.vuejs.org/guide/css.html#postcss](https://cli.vuejs.org/guide/css.html#postcss)

## CSS loader
Vue cli는 기본적으로 Webpack을 사용한다.   

Webpack의 css loader를 수정하려 아래와 같이 [`css.loaderOptions`](https://cli.vuejs.org/config/#css-loaderoptions)을 사용해야 한다.   

`vue.config.js` 파일안에 추가한다.

``` javascript
module.exports = {
  css: {
    loaderOptions: {
      css: {
        // options here will be passed to css-loader
      },
      postcss: {
        // options here will be passed to postcss-loader
      }
    }
  }
}
```

## postcss option
`vue.config.js`에서 css-loader 관련 option은 변경사항이 없어서 삭제한다.
postcss-loader 관련 기본 옵션을 `vue.config.js`에 추가한다.

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

``` javascript
module.exports = {
  plugins: [],
};
```

## tailwindcss setting

### install
postcss option까지 설정이 끝나면 postcss의 plugin 형태로 tailwindcss를 추가해 준다.

우선 모듈을 먼저 다운로드 한다. [install Tailwind via npm](https://tailwindcss.com/docs/installation#1-install-tailwind-via-npm)

``` bash
$> npm install tailwindcss --save-dev
//or npm i -D tailwindcss
```

tailwindcss의 config파일도 만들어 준다. [create your tailwind config file](https://tailwindcss.com/docs/installation#3-create-your-tailwind-config-file-optional)

``` bash
$> npx tailwindcss init
```

`tailwind.config.js` 파일이 생기고 안에는 아래와 같이 기본으로 설정된다.

``` javascript
module.exports = {
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
};
```

### Add postcss plugin
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
