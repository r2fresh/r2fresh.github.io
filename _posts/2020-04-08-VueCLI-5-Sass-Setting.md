---
title: Sass 기본 설정 및 확인
date: 2020-04-08 09:30:28 -0400
categories: jekyll update
---

### Sass 설치
Sass는 처음 프로젝트 생성 시 Manually로 선택하여 설치 (VueCLI-1-Create-Project를 참고)

### Sass 실행 확인

1. vue 프로젝트 안의 `src/components`안의 `HelloWorld.vue` 안의 style 태그 안에 아래의 코드 추가

``` html
<style scoped lang="scss">

$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

2. `HelloWorld.vue`를 저장 후 `npm run serve`를 실행

3. 화면의 inspector(개발자도구)를 열고 style을 확인해서 아래와 같이 나오면 Sass는 잘 작동

``` html
<style type="text/css">
<!-- data-v-469af010는 서버 실행시 마다 다름 -->
body[data-v-469af010] {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
</style>
```
