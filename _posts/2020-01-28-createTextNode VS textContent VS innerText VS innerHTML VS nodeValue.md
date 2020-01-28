---
title: createTextNode VS textContent VS innerText VS innerHTML VS nodeValue 비교분석
date: 2020-01-28 15:08:28 -0400
categories: jekyll update
---


Node, Document, Element 등의 메소드와 속성 등을 사용하여 다양하여 화면에 텍스트를 렌더링하여 넣을 수 있다.

예전에는 innerHTML은 많이 사용하였는데, 점차 textContent, createTextNode를 사용하게 되었고, 최근에는 nodeValue를 사용한다.

각기 문자를 넣는 것은 똑같지만, 지원 되는 브라우저와 그에 따른 성능이 다르다.

성능이 좋지만, 지원되는 브라우저 적고 최신만 지원하는 것도 있으며, IE6부터 모든 브라우저에서 지원하지만 성능은 낮은 것도 있다.

아래는 문자열을 넣는 메소드와 속성의 성능과 지원하는 브라우저를 정리한 내용입니다.

## Apply Browser (지원브라우저)

### Document.createTextNode

**Syntax**

```
var text = document.createTextNode(data);
```

**Browser compatibility**

https://caniuse.com/#search=createTextNode

* IE 11 ~
* Edge 12 ~
* Firefox 72 ~
* chrome 4 ~
* safari 13 ~
* opera 64 ~

### Node.textContent

**Syntax**

```
let text = someNode.textContent;
someOtherNode.textContent = string;
```

**Browser compatibility**

https://caniuse.com/#feat=textcontent

* IE 9 ~
* Edge 12 ~
* Firefox 2 ~
* chrome 4 ~
* safari 3.2 ~
* opera 10 ~


### HTMLElement.innerText

**Syntax**

```
var renderedText = HTMLElement.innerText;
HTMLElement.innerText = string;
```

**Browser compatibility**

https://caniuse.com/#feat=innertext

* IE 6 ~
* Edge 12 ~
* Firefox 45 ~
* chrome 4 ~
* safari 3.2 ~
* opera 10 ~

### HTMLElement.innerHTML

**Syntax**

```
const content = element.innerHTML;
element.innerHTML = htmlString;
```

**Browser compatibility**

https://caniuse.com/#feat=mdn-api_element_innerhtml

* IE 6 ~
* Edge 14 ~
* Firefox 2 ~
* chrome 33 ~
* safari 9 ~
* opera 10 ~


### Node.nodeValue

**Syntax**

```
str = node.nodeValue;
node.nodeValue = str;
```

**Browser compatibility**

https://caniuse.com/#feat=mdn-api_node_nodevalue

* 지원안함
* Edge 12 ~
* Firefox 72 ~
* chrome 79 ~
* safari 13 ~
* opera 64 ~

## BenchMark (성능측정결과)

https://www.measurethat.net/Benchmarks/ShowResult/91680

## 결과

전체적으로 문자열만을 넣을 경우 IE 브라우저를 신경쓰지 않는다면, nodeValue가 가장 성능이 좋습니다.
하지만 만약 IE를 하위 버전에 포함해야 한다면 textContent를 사용하는 것이 성능상으로는 가장 좋을 것 같습니다.
또한 innerHTML과 createTextNode는 특별한 경우를 제외 하고는 사용을 배제하는 것이 좋아 보입니다.