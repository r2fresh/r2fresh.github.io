---
title: "KV Storage: the Web's First Built-in Module[한글번역]"
date: 2019-03-18 08:26:28 -0400
categories: jekyll update
---

**원본 : https://developers.google.com/web/updates/2019/03/kv-storage**

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

## The KV Storage module

KV Storage module는 localStorage API와 비슷하지만, API의 모양은 JavaScript Map 객체와 같다. getItem(), setItem(), removeItem() 대신에 get(), set(), delete()를 가진다. 또한 localStorage에서는 가능 하지 않았던 map과 같은 메소드인 keys(), values(), entries() 있다. Key가 문자열일 필요는 없습니다.. [structured-serializable type](https://html.spec.whatwg.org/multipage/structured-data.html#serializable-objects)이라 할 수 있다.

Map과 달리 KV Storage methods는 promises 또는 async iterators중에 하나를 return 합니다.
전체 API의 자세한 것은 [specification](https://wicg.github.io/kv-storage/#storagearea)에서 볼 수 있다.

예제 코드에서 주목 할 점은, KV Storage module은 storage와 storageArea 두개를 export 한다는 점이다.

storage는 storageArea 클래스의 인스턴스이며, default라는 이름을 사용하여 개발자가 응용 프로그램 코드에서 가장 자주 사용하게 됩니다 StorageArea 클래스는 addtional isolation이 필요할 경우 제공 됩니다.(데이터를 저장하고 기본 storage인스턴스를 통해 저장된 데이터와의 충돌을 피하기 위한 third part library)

StorageArea data는 kv-storage:${name}의 이름으로 indexedDB database에 저장된다. 여기서 name은 StorageArea instance의 이름이다.

아래는 KV Storage를 사용하는 예제이다.

```javascript
import {storage} from 'std:kv-storage';

const main = async () => {
  const oldPreferences = await storage.get('preferences');

  document.querySelector('form').addEventListener('submit', async () => {
    const newPreferences = Object.assign({}, oldPreferences, {
      // Updated preferences go here...
    });

    await storage.set('preferences', newPreferences);
  });
};

main();
```

> Note : KV Storage module은 chrome 74에서 사용이 가능하다. 만약에 미리 사용하고 싶다면 아래의 url을 입력하고 enabled로 변경하면 사용이 가능하다.


## 만약에 browser에서 built-in module을 지원하지 않는다면?
브라우저에서 native JavaScript modules를 사용하는 것에 익숙하다, URL 아닌 다른 것을 가져오기 하는 것는 적어도 지금까지는 오류가 발생한다는 것을 알고 있을 것입니다. std:kv-storage는 유효한 URL이 아닙니다.

그래서 우리는 코드에서 사용하기 전에 모든 브라우저에서 built-in module을 제공할때까지 기다려야 할까요? 고맙게도 답은 아니오 입니다.

당신은 built-in modules를 곧 오직 하나의 브라우저에만 제공되도록 [import maps](https://github.com/WICG/import-maps)라 불리는 [expermenting](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/sEwWEF80T4s/Nss9VxM3BAAJ)의 다른 기술의 도움을 받아 고맙게도 실제 사용할 수 있다.

### Import maps

[import maps](https://github.com/WICG/import-maps)는 개발자가 하나 이상의 대체 식별자에 import identifiers의 별칭을 지정할 수 있는 본질적인 메카니즘이다.

브라우저가 당신의 어플리케이션에서 특정한 import identifier를 (런타임) 어떻게 해결하는지 변경할 수 있는 방법을 제공하기 때문에 강력합니다.

built-in modules의 경우 당신의 어플리케이션에 모듈의 polyfill을 참조할 수 있지만, built-in module을 지원하는 브라우저에서는 해당버전을 로드 해야 합니다.

아래는 KV Storage 모듈을 import map을 선언하여 사용하는 방법입니다.

```javascript
<!-- The import map is inlined into your page -->
<script type="importmap">
{
  "imports": {
    "/path/to/kv-storage-polyfill.mjs": [
      "std:kv-storage",
      "/path/to/kv-storage-polyfill.mjs"
    ]
  }
}
</script>

<!-- Then any module scripts with import statements use the above map -->
<script type="module">
  import {storage} from '/path/to/kv-storage-polyfill.mjs';

  // Use `storage` ...
</script>
```

위의 코드에서 중요한 점은 URL(/path/to/kv-storage-polyfill.mjs)가 std:kv-storage와 원래 URL /path/to/kv-storage-polyfill.mjs 두가지 리소스에 매핑 된다는 것입니다.

따라서 브라우저가 해당 URL(/path/to/kv-storage-polyfill.mjs)을 참조하는 import문을 발견하면 먼저 std:kv-storage를 로드하려고 시도하고, 할수 없는 경우 /path/to/kv-storage-polyfill.mjs 를 로드 할 것입니다.

여기서도 마술은 브라우저가 가져오기 명령에 전달되는 URL이 polyfill의 URL이기 때문에 브라우저가 가져 오기 맵이나 이 기술을 위한 내장 모듈을 지원할 필요가 없다는 것입니다. 폴리필은 실제로 fallback이 아니며 기본 값입니다. built-in module은 점진적은 향상입니다.

## 모듈 전혀 지원하지 않는 브라우저일 경우?

import maps을 사용하여 built-in module을 로드하려면 import문을 사용해야 합니다. 이는 [module script](https://developers.google.com/web/fundamentals/primers/modules#module-vs-script)(ex: script type="module" )를 사용해야 한다는 것을 의미합니다.

현재, 브라우저의 80% 이상이 modules를 지원하고, 그리고 브라우저에서 지원하지 않는 경우 [module/nomodule technique](https://philipwalton.com/articles/deploying-es2015-code-in-production-today/)를 사용하여 구형 브라우저에 기존 번들로 제공 할 수 있습니다.
nomodule 빌드를 생성할 때 모듈을 지원하지 않는 브라우저가 내장 모듈을 지원하지 않는다는 것을 알고 있기 때문에 모든 폴리필을 포함해야 합니다.

## KV Storage demo
구 브라우저를 저원을 하면서 built-in modules를 사용이 가능하다는 것을 설명하기 위해 설명된 모든 기술을 통합하고 모든 브라우저에서 돌아가는 것을 작성했습니다.

* modules, import maps, built-in module을 지원하는 브라우저는 불필요한 코드를 로드하지 않습니다.
* modules, import maps를 지원하지만 built-in module를 지원하지 않는 브라우저는 [KV Storage polyfill](https://github.com/GoogleChromeLabs/kv-storage-polyfill)을 로드 합니다.(브라우저의 모듈로더를 통해)
* module은 지원하지만 import maps또한 지원하지 않는 브라우저는 [KV Storage polyfill](https://github.com/GoogleChromeLabs/kv-storage-polyfill)를 통해 로드합니다.(브라우저의 모듈로더를 통해)
* modules를 전혀 지원하지 않는 브라우저는 legacy bundle`<script nomodule>`에 [KV Storage polyfill](https://github.com/GoogleChromeLabs/kv-storage-polyfill) 파일을 로드합니다.

데모는 Glitch에 올려져 있다. 그래서 [실행 화면과 소스코드](https://glitch.com/edit/#!/rollup-built-in-modules)를 볼 수 있다.
또한 README에 구현에 대한 자세한 설명이 있다. 빌드가 어떻게 되었는지 보고 싶은 호기심이 있다면 자유롭게 보면서 느끼세요

> Note:데모는 application code와 polyfill의 bundle로 [Rollup](https://rollupjs.org/guide/en)을 사용하였고,Cross-browser에서 작동할 수 있도록 요구되는 다양한 버전을 생성하고 있습니다. webpack을 사용하여 비슷한 데모를 빌드하고 싶었지만, webpack은 현재 output format module을 지원하고 있지않다. 그래서 아직 불가능 하다.

> webpack에 built-in modules를 제공에 대한 기술 요구를 제출했다. 만약 webpack에서 지원되는 것을 보기를 원한다면, 당신의 목소리를 issue로 지원하기 바랍니다.

native built-in module 동작을 실제로 보기를 위해서는, experimental web platform features flag가 켜져있는 Chrome 74에서 데모를 로드 해야 합니다(chrome://flags/#enable-experimental-web-platform-features).

built-in module은 Devtools의 source panel에서는 polyfill script로 볼수 없기 때문에,

Devtools의 source panel에 polyfill script가 표시되지 않기 때문에, built-in module이 로드 되고 있는 확인 할 수 있다; 대신에 built-in module version을 볼수 있을 것이다.
(재미있는 것: modules's 의 source code를 실제로 inspect로 보거나 또는 breakpoint로 넣을 수 있다.)

## 기능에 대한 의견을 제공 할 수 있는 GitHub

* [KV Storage](https://github.com/WICG/kv-storage)
* [KV Storage Polyfill](https://github.com/GoogleChromeLabs/kv-storage-polyfill)
* [Built-in modules](https://github.com/tc39/proposal-javascript-standard-library)
* [Import Maps](https://github.com/WICG/import-maps)










































