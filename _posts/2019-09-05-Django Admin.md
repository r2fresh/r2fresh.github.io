# django.admin

ModelAdmin을 사용하지 않고 기본으로 사용하려면

``` python
from django.contrib import admin
from myproject.myapp.models import Author

admin.site.register(Author)
```

The register decorator

'@' decorator를 사용하여 model을 등록 할 수 있다.

``` python
from django.contrib import admin
from .models import Author

@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    pass
```

## ModelAdmin.list_display

model의 데이터를 입력하는 폼의 순서를 정하는 변수

``` python
class PersonAdmin(admin.ModelAdmin):
    list_display = ('first_name', 'last_name')
```

## ModelAdmin.search_fields
Model에서 검색으로 할 객체를 선
``` python
search_fields = ['foreign_key__related_fieldname']
```
