---
title: VueCLI Stylelintrc Setting
date: 2020-04-16 09:30:28 -0400
categories: jekyll update
---
### 설치 이유?

Vscode에서 VueCLI+Sass+PostCSS+TailwindCSS를 사용시 아래와 같은 경고 발생

* at-rule or selector expected css(css-ruleorselectorexpected)
* semi-colon expected css(css-semicolonexpected)
* Unknown at rule @tailwind css(unknownAtRules)

이러한 경고를 없애기 위해 아래와 같이 설치를 진행

### Vscode Extension를 사용하여 stylelint 설치

Vscode Extension에서 `publisher:"stylelint"`를 검색하여 설치

![스크린샷_2020-04-15_오후_10.38.46](uploads/138f58923c7db9a53cd25d83e1d2dfe3/스크린샷_2020-04-15_오후_10.38.46.png)

### Stylelint Config File 추가

현재 Project root에 `.stylelintrc.js`파일을 생성하고 아래와 같이 입력

``` bash
ken-fe $> touch .stylelintrc.js 
```

``` javascript
module.exports = {
  rules: {
    'at-rule-no-unknown': [
      true,
      {
        ignoreAtRules: ['tailwind', 'apply', 'variants', 'responsive', 'screen'],
      },
    ],
    'declaration-block-trailing-semicolon': null,
    'no-descending-specificity': null,
  },
};
```

### Vscode의 CSS Default Validate를 비활성화

`.vscode/settings.json`파일 안에 비활성화를 추가

``` javascript
{
  "editor.tabSize": 2,
  // r2fresh add
  "editor.formatOnSave": true,
  // r2fresh add
  "editor.detectIndentation": false,
  // vscode css validatate
  "css.validate": false
}
```

### Vscode를 재실행

### 참고

[https://www.meidev.co/blog/visual-studio-code-css-linting-with-tailwind/](https://www.meidev.co/blog/visual-studio-code-css-linting-with-tailwind/)
