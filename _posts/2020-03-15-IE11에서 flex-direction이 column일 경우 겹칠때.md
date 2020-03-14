---
title: IE11에서 flex-direction이 column일 경우 겹칠때
date: 2020-03-15 09:30:28 -0400
categories: jekyll update
---

flex-direction을 column으로 했는데 적용이 되지 않고 Element가 겹칠 때가 있다.
그럴경우 아래와 같은 페이크 기술을 사용하여 겹치지 않도록 할 수 있다.

```html
<div class="parent">
    <div class="child">child_1</div>
    <div class="child">child_2</div>
    <div class="child">child_3</div>  
</div>
```

```css
/* 요약에서 질문, 답변, 댓글을 보기위해 flex적용 */
.parent{
    display: flex;
    flex-direction: column;
}

/* 요약에서 질문, 답변, 댓글 가로로 30%으로 나누어서 보여주기 위해 스타일 적용 */
.child{
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: auto;
}
```

결국 중요한 것은 `flex-basis:auto`로 하는 것이다.