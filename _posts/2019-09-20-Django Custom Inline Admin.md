## admin.TabularInline 에서 Model Data List에서 특정 객체만 보이는 방법

**변경 전 소스 코드**
``` python
class ModuleVersionInline(admin.TabularInline):
    model = ModuleVersion
    extra = 0
    fk_name = 'module'
```
**변경 후 소스 코드**
``` python
class ModuleVersionInline(admin.TabularInline):
    model = ModuleVersion
    extra = 0
    fk_name = 'module'

    # 모듈 버전만 보임
    fieldsets = [
        (None, {
            'fields': [
                'module_version'
            ],
        }),
    ]
```

## "created_at", "updated_at"과 fieldsets에 추가 방법

`created_at`과 `updated_at`과 같이 auto로 입력 되는 부분을 fieldsets에 추가하기 위해서는 readonly_fields 리스트에 추가 해야 한다. 아래의 소스코드와 같이 변경 하면 된다.

``` python
class ModuleVersionInline(admin.TabularInline):
    model = ModuleVersion
    extra = 0
    fk_name = 'module'

    fieldsets = [
        (None, {
            'fields': [
                'module_version',
                'created_at',
                'updated_at'
            ],
        }),
    ]

    readonly_fields = [
        'module_version', 
        'created_at',
        'updated_at'
    ]
```

## Module Admin에서 바로 module version을 삭제

version의 삭제는 보다 신중하게 하기 위해 Module Admin에서는 삭제가 안 되도록 아래와 같이 소스코드를 변경

``` python
class ModuleVersionInline(admin.TabularInline):
    model = ModuleVersion
    extra = 0
    fk_name = 'module'

    fieldsets = [
        (None, {
            'fields': [
                'module_version',
                'created_at',
                'updated_at'
            ],
        }),
    ]

    readonly_fields = [
        'module_version', 
        'created_at',
        'updated_at'
    ]
    
    # version delete 체크박스 삭제
    def has_delete_permission(self, request, obj=None):
        return False
```

## Module Admin에서 Module version 추가 버튼 삭제

"Add another Module version" 버튼은 클릭 Module Admin에서 바로 version이 추가가 된다.
바로 추가 되는 것을 막기 위해서 아래의 코드를 추가 한다.

``` python
class ModuleVersionInline(admin.TabularInline):
    model = ModuleVersion
    extra = 0
    fk_name = 'module'

    fieldsets = [
        (None, {
            'fields': [
                'module_version',
                'created_at',
                'updated_at'
            ],
        }),
    ]

    readonly_fields = [
        'module_version', 
        'created_at',
        'updated_at'
    ]

    # version delete 체크박스 삭제
    def has_delete_permission(self, request, obj=None):
        return False

    # + Add another Module version 삭제
    def has_add_permission(self, request):
        return False
```

## Module Version 수정 버튼 추가

version의 내용을 수정하기 위한 페이지로 연결하는 버튼 추가
"show_change_link" True를 해서 사용해도 되지만 UI 부분에서 깔끔하지 않아서 직접 UI를 생성해서 추가한다.

``` python
class ModuleVersionInline(admin.TabularInline):
    model = ModuleVersion
    extra = 0
    fk_name = 'module'

    fieldsets = [
        (None, {
            'fields': [
                'module_version',
                'created_at',
                'updated_at',
                'edit_version'
            ],
        }),
    ]

    readonly_fields = [
        'module_version', 
        'created_at',
        'updated_at',
        'edit_version'
    ]

    # version delete 체크박스 삭제
    def has_delete_permission(self, request, obj=None):
        return False

    # + Add another Module version 삭제
    def has_add_permission(self, request):
        return False

    def edit_version(self, obj=None):
        url = resolve_url('/admin/toppings_modules/moduleversion/%s/change/' % (obj.version_no))
        return '<div style="margin-left:20px;"><a href="{url}" target="_black" class="btn" style="{style}">Edit</a></div>'.format(
            url=url,
            style='color:#fff!important;background-color:#0bf!important;border: 1px solid #0bf!important;'
        )
    edit_version.short_description = ("Module Version Edit")
    edit_version.allow_tags=True
```

## new version 생성 페이지 링크 버튼 추가

기존의 template를 수정하기 위해서는 기존 template을 기반으로 해서 수정하고자 하는 부분의 block 부분을 수정 하면 된다. inlines로 추가된 부분이 많을 경우 verbose_name를 사용하여 특정 inline일 경우에만 실행하도록 한다.

``` python
class ModuleAdmin(admin.ModelAdmin):
    verbose_name = 'modules admin'

    fieldsets = [
        (None, {
            'fields': [
                'module_name',
                'module_slug',
                'module_status',
                'module_type'
            ],
        }),
    ]

    inlines = [ModuleVersionInline, ModuleLanguageInline]
    list_display = (
        'module_name',
        'module_slug',
        'module_status',
        'module_type',
        'created_at',
        'updated_at'
    )
    search_fields = ['module_name', 'module_type']
    
    # new version 버튼 추가 위해 template 수정
    change_form_template = "modules/change_form.html"
```

``` python
# change_form.html template 부분 생성 및 수정
{% extends "admin/change_form.html" %}
{% load i18n admin_urls static admin_modify %}
{% block inline_field_sets %}
{% for inline_admin_formset in inline_admin_formsets %}
    {% include inline_admin_formset.opts.template %}
    {% if inline_admin_formset.opts.verbose_name == 'module version'%}
        <div style="margin:0px 0px 20px 30px;text-align:right;">
            <a href="../../../../admin/toppings_modules/moduleversion/add/" class="btn" style="color:#fff!important;background-color:#0bf!important;border: 1px solid #0bf!important;">Add another Module Version</a>
        </div>
    {% endif %}
{% endfor %}
{% endblock %}

```