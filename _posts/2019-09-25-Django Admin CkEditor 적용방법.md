---
title: Django Admin CkEditor 적용방법
date: 2019-09-25 11:37:28 -0400
categories: jekyll update
---

## Django Admin TextArea에 CkEditor(version-4) 적용방법

**1. [Downlaod](https://ckeditor.com/ckeditor-4/download/)에서 Standard Package를 다운로드**

**2. 압축을 풀고, 버전을 표시하기 위해 폴더명을 "ckeditor_4.12.1_standard"로 변경한다.**

**3. 변경된 폴더를 setting.py에 static file 설정에 적용된 폴더에 복사한다. 아래는 예시입니다.**

``` python
# assets 폴더에 복사
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'assets'),
)
```

**4. adimn.py에서 ckeditor를 사용하려는 class에 아래와 같이 추가한다.**

``` python 
class ModuleContentInline(admin.StackedInline):

    formfield_overrides = {
            models.TextField: {'widget': Textarea( attrs={'class':'cke_source ckeditor'} )},
        }
        class Media:
            js = ("lib/ckeditor_4.12.1_standard/ckeditor.js","js/toppings_editor.js")
```

* "lib/ckeditor_4.12.1_standard/ckeditor.js" 는  assets 폴더 아래의 경로이다.
* "js/toppings_editor.js"는 ckediotr를 실행하는 javascript 소스코드가 들어간다. 경로는 assets 폴더 아래의 경로이다.
* attrs에서 class에 ckeditor를 추가한다. 
* 참고로 cke_source는 textarea의 width의 auto값을 적용하지 않기 위해 사용한 class이다. (참고링크)

**5. 마지막으로 toppings_editor.js의 소스코드를 아래와 같이 작성한다.**

``` javascript
window.onload=function(){
    CKEDITOR.replaceClass = 'ckeditor';
}
```