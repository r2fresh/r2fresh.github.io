## InlineModleAdmin objects

**class InlineModelAdmin**

**class TabularInline[source]¶**

**class StackedInline[source]¶**

관리자 인터페이스는 상위 모델과 동일한 페이지에서 모델을 편집 할 수 있습니다. 이것을 인라인이라고합니다. 다음 두 모델이 있다고 가정하십시오.

``` python
from django.db import models

class Author(models.Model):
   name = models.CharField(max_length=100)

class Book(models.Model):
   author = models.ForeignKey(Author, on_delete=models.CASCADE)
   title = models.CharField(max_length=100)
```
저자 페이지에서 저자가 저술 한 책을 편집 할 수 있습니다. ModelAdmin.inlines에서 모델을 지정하여 모델에 인라인을 추가합니다.

``` python
from django.contrib import admin

class BookInline(admin.TabularInline):
    model = Book

class AuthorAdmin(admin.ModelAdmin):
    inlines = [
        BookInline,
    ]
```
Django는 InlineModelAdmin의 두 가지 하위 클래스를 제공하며 다음과 같습니다.

* [TabularInline](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.TabularInline)
* [StackedInline](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.StackedInline)

이 둘의 차이점은 단지 그것들을 렌더링하는 데 사용되는 템플릿입니다.

### InlineModelAdmin options¶

InlineModelAdmin은 ModelAdmin과 동일한 기능을 많이 공유하고 자체 기능을 추가합니다 (공유 기능은 실제로 BaseModelAdmin 수퍼 클래스에 정의되어 있음). 공유 기능은 다음과 같습니다.

* [form](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.InlineModelAdmin.form)
* [fieldsets](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fieldsets)
* [fields](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fields)
* [formfield_overrides](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.formfield_overrides)
* [exclude](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.exclude)
* [filter_horizontal](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.filter_horizontal)
* [filter_vertical](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.filter_vertical)
* [ordering](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.ordering)
* [prepopulated_fields](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.prepopulated_fields)
* [get_fieldsets()](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.get_fieldsets)
* [get_queryset()](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.get_queryset)
* [radio_fields](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.radio_fields)
* [readonly_fields]([readonly_fields](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.readonly_fields))
* [raw_id_fields](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.InlineModelAdmin.raw_id_fields)
* [formfield_for_choice_field()](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.formfield_for_choice_field)
* [formfield_for_foreignkey()](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.formfield_for_foreignkey)
* [formfield_for_manytomany()](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.formfield_for_manytomany)
* [has_module_permission()](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.has_module_permission)

InlineModelAdmin 클래스는 다음을 추가하거나 사용자 정의합니다.

**InlineModelAdmin.model**

인라인이 사용중인 모델입니다. 필수입니다.

**InlineModelAdmin.fk_name**

모델의 외래 키 이름입니다. 대부분의 경우 이것은 자동으로 처리되지만 동일한 상위 모델에 대한 외래 키가 두 개 이상인 경우 fk_name을 명시 적으로 지정해야합니다.

**InlineModelAdmin.formset**

기본값은 [BaseInlineFormSet](https://docs.djangoproject.com/en/2.2/topics/forms/modelforms/#django.forms.models.BaseInlineFormSet)입니다. 자체 양식 세트를 사용하면 많은 사용자 정의가 가능합니다. 인라인은 [model formsets](https://docs.djangoproject.com/en/2.2/topics/forms/modelforms/#model-formsets)을 중심으로 구축됩니다.

**InlineModelAdmin.form**

양식 값은 기본적으로 ModelForm입니다. 이것이이 인라인의 폼셋을 생성 할 때 inlineformset_factory ()로 전달되는 것입니다.

> `!` InlineModelAdmin 양식에 대한 사용자 정의 유효성 검증을 작성할 때 상위 모델의 기능에 의존하는 유효성 검증을 작성하는 데주의하십시오. 상위 모델의 유효성 검증에 실패하면 [Validation on a ModelForm](https://docs.djangoproject.com/en/2.2/topics/forms/modelforms/#validation-on-modelform)의 경고에 설명 된대로 불일치 상태로 남아있을 수 있습니다.

**InlineModelAdmin.classes**

인라인에 렌더링되는 필드 세트에 적용 할 추가 CSS 클래스를 포함하는 목록 또는 튜플입니다. 기본값은 없음입니다. 필드 세트에 구성된 클래스와 마찬가지로 축소 클래스가있는 인라인은 처음에 축소되고 헤더에는 작은 "표시"링크가 있습니다.

**InlineModelAdmin.extra**

이는 초기 양식 외에 양식 세트에 표시되는 추가 양식의 수를 제어합니다. 자세한 내용은 양식 세트 설명서를 참조하십시오.

JavaScript 가능 브라우저를 사용하는 사용자의 경우 추가 인수의 결과로 제공되는 인라인 외에 추가 인라인을 추가 할 수 있도록 "다른 추가"링크가 제공됩니다.

현재 표시된 양식 수가 max_num을 초과하거나 사용자가 JavaScript를 사용할 수없는 경우 동적 링크가 나타나지 않습니다.

InlineModelAdmin.get_extra ()를 사용하면 추가 양식 수를 사용자 정의 할 수도 있습니다.

**InlineModelAdmin.max_num**

인라인에 표시 할 최대 양식 수를 제어합니다. 이는 개체 수와 직접 관련이 없지만 값이 충분히 작은 경우 가능합니다. 자세한 내용은 편집 가능한 객체 수 제한을 참조하십시오.

InlineModelAdmin.get_max_num ()을 사용하면 최대 추가 양식 수를 사용자 정의 할 수도 있습니다.

**InlineModelAdmin.min_num**

인라인에 표시 할 최소 양식 수를 제어합니다. 자세한 정보는 modelformset_factory ()를 참조하십시오.

[InlineModelAdmin.get_min_num()](https://docs.djangoproject.com/en/2.2/ref/contrib/admin/#django.contrib.admin.InlineModelAdmin.get_min_num)을 사용하면 표시되는 최소 양식 수를 사용자 정의 할 수도 있습니다.

**InlineModelAdmin.raw_id_fields**

기본적으로 Django의 관리자는 ForeignKey 인 필드에 선택 상자 인터페이스 (`<select>`)를 사용합니다. 드롭 다운에 표시 할 모든 관련 인스턴스를 선택해야하는 오버 헤드를 원하지 않는 경우가 있습니다.

raw_id_fields는 ForeignKey 또는 ManyToManyField에 대한 입력 위젯으로 변경하려는 필드 목록입니다.

``` python
class BookInline(admin.TabularInline):
    model = Book
    raw_id_fields = ("pages",)
```

**InlineModelAdmin.template**

페이지에서 인라인을 렌더링하는 데 사용되는 템플릿입니다.

**InlineModelAdmin.verbose_name**

모델의 내부 메타 클래스에있는 verbose_name을 재정의합니다.

**InlineModelAdmin.verbose_name_plural**

모델의 내부 메타 클래스에있는 verbose_name_plural에 대한 재정의

**InlineModelAdmin.can_delete**

인라인에서 인라인 객체를 삭제할 수 있는지 여부를 지정합니다. 기본값은 True입니다.

**InlineModelAdmin.show_change_link**

관리자에서 변경할 수있는 인라인 오브젝트에 변경 양식에 대한 링크가 있는지 여부를 지정합니다. 기본값은 False입니다.