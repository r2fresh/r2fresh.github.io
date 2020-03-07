---
title: Form Element 속성 required
date: 2020-03-08 09:30:28 -0400
categories: jekyll update
---

Required는 Form Element(input, select, checkbox, radio, textarea..)등에 기본적으로 입력값이 입력되지 않았을 경우 체크하여 Form의 스타일을 변경하여 알려준다.

하지만 Required를 사용하고 싶지 않은 경우도 있다.

기본적은 에러메세지 표현이 아닌, 사용자가 만든 에러 메세지 표현과 중복이 될수 있다.

자신이 모든 validation을 체크하고자 한다면 required를 사용하지 않는 것이 좋다.

## required사용하지 않는 방법

너무나도 간단하다 아래와 같이 하면 된다.

```javascript
Node.required = false;
```

위의 같이 하면 기본적인 에러 알림표시가 꺼지게 된다.

현재는 Firefox에서만 기본 에러메세지 표현이 실행되는 것을 확인 하였다.

혹시 모르니 항상 Firefox, safari,등의 브라우저도 테스틑를 하자.

