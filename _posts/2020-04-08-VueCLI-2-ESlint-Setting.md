---
title: .eslintrc.js 추가 설정
date: 2020-04-08 09:30:28 -0400
categories: jekyll update
---

### quote-props 설정

Object Property의 key의 name에 따옴표의 포함 여부 지정

``` javascript
// quote를 필수로 입력하도록 'always'로 선택
'quote-props': ['warn', 'as-needed'],
```

### quotes 설정

quote를 single인지 double인지 설정

``` javascript
//'single'로 선택
'quotes': ['error', 'single'],
```

### array-element-newline 설정

array elemnt를 설정

``` javascript
// 배열 요소 사이에 줄 바꿈을 일관되게 사용하기 위해 consistent 선택
'array-element-newline': ['error', 'consistent'],
```

### .eslintrc.js 전체 내용
``` javascript
module.exports = {
  root: true,
  env: {
    node: true,
  },
  extends: [
    'plugin:vue/essential',
    'eslint:recommended',
    '@vue/typescript/recommended',
    '@vue/prettier',
    '@vue/prettier/@typescript-eslint',
  ],
  parserOptions: {
    ecmaVersion: 2020,
  },
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'quote-props': ['warn', 'always'],
    'quotes': ['error', 'single'],
    'array-element-newline': ['error', 'consistent'],
  },
  overrides: [
    {
      files: ['**/__tests__/*.{j,t}s?(x)', '**/tests/unit/**/*.spec.{j,t}s?(x)'],
      env: {
        mocha: true,
      },
    },
  ],
};

```