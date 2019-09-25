---
title: Django Admin TextArea 강제 width값 해제 방법
date: 2019-09-25 11:37:28 -0400
categories: jekyll update
---

## Djangocms Admin TextArea 강제 width값 해제 방법

Django Admin에서 TextArea의 width값이 !important로 설정되어 변경이 width값이 auto에서 아무리 해도 다른 것으로 변경이 안된다.

``` css
/* djangocms-admin.css */
.inline-group .module:not(.aligned) .form-row input, 
.inline-group .module:not(.aligned) .form-row textarea:not(.cke_source) {
    width: auto !important;
}
```

이것을 변경하려면 `textarea:not(.cke_source)`를 사용하여 class name에 'cke_source'를 추가하면 CSS가 적용이 되지 않는다.
적용방법은 아래와 같이 적용하면 된다.

``` python
class ModuleContentInline(admin.StackedInline):
    ...
    formfield_overrides = {
        models.TextField: {'widget': Textarea( attrs={'class':'cke_source'} )},
    }
    ...
        
```