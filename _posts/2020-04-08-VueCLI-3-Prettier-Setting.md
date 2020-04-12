---
title: .prettier 추가 설정
date: 2020-04-08 09:30:28 -0400
categories: jekyll update
---

### singleQuote 설정

>https://prettier.io/docs/en/options.html#quotes

quote를 single로 사용할지 double로 사용할지 설정

``` javascript
// true로 설정
"singleQuote": true
```

### semi 설정

> https://prettier.io/docs/en/options.html#semicolons

명령문 끝에 semicolns를 붙이는 여부 설정

``` javascript
// true로 설정
"semi": true
```

### tailingComma 설정

> https://prettier.io/docs/en/options.html#trailing-commas

Comma를 마지막에 붙이는지 아닌지 설정

``` javascript
// true로 설정
"trailingComma": "es5"
```

### tabWidth 설정

> https://prettier.io/docs/en/options.html#tab-width

들여쓰기 정도를 설정

```javascript
// 2로 설정
"tabWidth": 2
```

### quoteProps 설정

> https://prettier.io/docs/en/options.html#quote-props

Object안에 quote를 사용할 것인지 여부

```javascript
// as-needed로 설정
"quoteProps": "as-needed",
```

### vueIndentScriptAndStyle 설정

> https://prettier.io/docs/en/options.html#vue-files-script-and-style-tags-indentation

vue 파일 안의 `script`와 `style` 태그 안에서 처음 들여쓰기 여부

``` javascript
// false로 설정
"vueIndentScriptAndStyle" : false
```

### printWidth 설정

> https://prettier.io/docs/en/options.html#print-width

코딩에서 단락을 할 가로 길이 설정

``` javascript
// 120으로 설정
"printWidth": 120
```

### htmlWhitespaceSensitivity 설정

> https://prettier.io/docs/en/options.html#html-whitespace-sensitivity

html에서는 공백 설정

``` javascript
// 120으로 설정
"htmlWhitespaceSensitivity": "css"
```

### prettier, eslint 전체 내용

**.prettierrc**

``` javascript
{
  "singleQuote": true,
  "semi": true,
  "trailingComma": "es5",
  "tabWidth": 2,
  "quoteProps": "as-needed",
  "vueIndentScriptAndStyle": false,
  "printWidth": 120,
  "htmlWhitespaceSensitivity": "css",
}
```

**.eslintrc.js**
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
    'quote-props': ['warn', 'as-needed'],
    'quotes': ['error', 'single'],
    'array-element-newline': ['error', 'consistent'],
    'prettier/prettier': [
      'warn',
      {
        singleQuote: true,
        semi: true,
        trailingComma: 'es5',
        tabWidth: 2,
        quoteProps: 'as-needed',
        vueIndentScriptAndStyle: false,
        printWidth: 120,
        htmlWhitespaceSensitivity: 'css',
      },
    ],
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
