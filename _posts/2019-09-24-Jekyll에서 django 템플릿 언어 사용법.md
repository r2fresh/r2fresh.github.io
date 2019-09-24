## Markdown에서 Django 템플릿을 사용시 발생하는 에러 해결 방법

아래와 같은 템플릿을 markdown의 코드를 넣는 곳에 넣으면 다음과 같은 에러가 발생한다.

``` python
{% extends "admin/change_form.html" %}
{% load i18n admin_urls static admin_modify %}
{% block inline_field_sets %}

...

{% endblock %}
```

에러 내용

```
The page build failed for the `master` branch with the following error:

The tag `extends` on line 204 in `_posts/2019-09-20-Django` is not a recognized Liquid tag. For more information, see https://help.github.com/en/articles/page-build-failed-unknown-tag-error.

For information on troubleshooting Jekyll see:

  https://help.github.com/articles/troubleshooting-jekyll-builds

If you have any questions you can contact us by replying to this email.
```

해결방법은 아래와 같이 raw를 추가하면 된다.
``` python
{% raw %}
# change_form.html template 부분 생성 및 수정
{% extends "admin/change_form.html" %}
{% load i18n admin_urls static admin_modify %}
{% block inline_field_sets %}

...

{% endblock %}
{% endraw %}
```