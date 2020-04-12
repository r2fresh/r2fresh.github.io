---
title: Vue 프로젝트 생성
date: 2020-04-08 09:30:28 -0400
categories: jekyll update
---

## VueCLI Install

### 1. npm을 사용하여 설치

``` bash
$> npm install -g @vue/cli
// OR
$> yarn global add @vue/cli
```

### 2. vue 프로젝트를 생성

``` bash
$> vue create helloworld
```

### 3. Manual로 선택

``` bash
? Please pick a preset: 
  ken-fe (node-sass, babel, typescript, router, vuex, eslint, unit-mocha, e2e-nightwatch) 
  default (babel, eslint) 
❯ Manually select features 
```

### 4. `PWA`만을 제외하고 아랜를 모두 체크

``` bash
? Check the features needed for your project: 
 ◉ Babel
 ◉ TypeScript
 ◯ Progressive Web App (PWA) Support
 ◉ Router
 ◉ Vuex
 ◉ CSS Pre-processors
 ◉ Linter / Formatter
 ◉ Unit Testing
❯◉ E2E Testing
```

### 5. class-style component 사용 여부 `Y`

``` bash
Use class-style component syntax? Yes
```

### 6. TypeScript와 함께 Babel 사용 여부 `Y`

``` bash
Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? Yes
```

### 7. Router 사용 여부 `Y`

``` bash
Use history mode for router? (Requires proper server setup for index fallback in production) Yes
```

### 8. CSS 전처리기 선택 `Sass/SCSS (with dart-sass)`

``` bash
Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): Sass/SCSS (with dart-sass)
```

### 9. Lint와 Fommater 선택 `ESLint + Prettier`

``` bash
Pick a linter / formatter config: Prettier
```

### 10. 추가 lint 기능 선택 `Lint on save`

``` bash
Pick additional lint features: Lint on save
```

### 11. Unit testing Solution 선택 `Mocha + Chai`

``` bash
Pick a unit testing solution: Mocha
```

### 12. E2E testing Solution 선택 `Nightwatch (WebDriver-based)`

``` bash
Pick an E2E testing solution: Nightwatch
```

### 13. test browser 선택 `Chrome & Firefox`

``` bash
Pick browsers to run end-to-end test on Chrome, Firefox
```

### 14. 설정파일 생성 타입 여부 `In dedicated config files`

``` bash
Where do you prefer placing config for Babel, ESLint, etc.? In dedicated config files
```

### 15. 사전 설정을 저장하시겠습니까? `Y`

``` bash
Save this as a preset for future projects? Yes
```

### 16. 사전 설정으로 지정한 이름 입력 `hello`

``` bash
Save preset as: hello
```