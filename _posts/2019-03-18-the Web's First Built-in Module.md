---
title: "KV Storage: the Web's First Built-in Module[한글번역]"
date: 2019-03-18 08:26:28 -0400
categories: jekyll update
---

Browser Vendor와 Web performance experts는 지난 10년가 localstorage는 느리며, Web developer는 그것을 사용하지 말아야 한다고 말해오고 있습니다.

공평하게 보면 이렇게 말하는 사람들 잘못 아니다. Localstorage는 메인 스레드를 차단하는 방식인 synchronous API(동기식)로, 
매번 그것에 엑세스 할때 마다 잠재적으로 interactive하고 페이지를 할 수 없습니다.

문제는 localstorage API가 매우 간단하고, localstorage의 비동기적인 대안이 indexDB라는 점입니다. 그것은 편의성과 환영을 받지 못하는 API입니다.

개발자는 사용하기 어려운 것과 성능이 나븐 것 중 하나를 선택해야 한다. 

실제로 비동기 스토리지 API에 localstorage API이 단순하믈 사용할 수 있도록 하는 라이브러리가 있지만, 그것을 사용하면 Size도 커지고 추가적인 비용과 예산이 들어가게 된다. 

그러나 만약 파이리 필요없고 localstorage API의 단순함을 사용할 수 있다면 어떨까?

곧 나오겠지만, 다양한 Chrome 내장 모듈 중에 도 우선 비동기식 키/값 저장 모듈인 kV Storage에 대해 알아보자

## 내장 모듈이란?

브라우저에서 제공되기 때문에 따로 다운로드를 할 필요가 없는 것 빼고는 JavaScript module과 같다.

예전과 마찬가지로 웹 API는 표준화 과장을 거쳐야 한다. 

웹 API와 달리 기본 제공 모듈은 전역 범위에 노출되지 않으며 import를 통해서만 사용할 수 있습니다.

내장모듈을 전역적으로 노출시키지 않으면 많은 이점이 있습니다. 새로운 런타임 컨텍스트를 시작하는데 오버 헤드를 추가하지 않으며 메모리를 사용하지 않으며, CPU가 실제로 import를 하지 않는 한 코드에 정의 된 다른 변수와 충돌이 날 염려도 없습니다.

내장 모듈을 가져오려면, 접두어 std:뒤에 내장모듈 식별자를 사용하십시오. 지원되는 브라우저에서는 다음과 같이 해서 import 할 수 있고,
지원되지 않는 브라우저는 [how to use a KV Storage polyfill in unsupported browsers]('https://developers.google.com/web/updates/2019/03/kv-storage#what_if_a_browser_doesnt_support_a_built-in_module') 를 통해 사용할 수 있습니다.

``` javascript
import {storage, StorageArea} from 'std:kv-storage';
```