---
title: Django Document Application
date: 2019-09-07 11:37:28 -0400
categories: jekyll update
---

# Applications

Django에는 구성을 저장하고 내부 검사를 제공하는 설치된 응용 프로그램의 레지스트리가 포함되어 있습니다.
또한 사용 가능한 모델 목록을 유지 관리합니다.

이 레지스트리는 간단히 앱이라고하며 django.apps에서 사용할 수 있습니다.

``` python
>>> from django.apps import apps
>>> apps.get_app_config('admin').verbose_name
'Administration'
```

## Projects and applications

프로젝트라는 용어는 Django 앱 어플리케이션을 설명합니다. 프로젝트 Python 패키지는 주로 설정 모듈로 정의되지만, 일반적으로 다른 것들이 포함되어 있습니다.예를들어 django-admin startprojet mysite를 실행하면, settings.py, urls.py 및 wsgi.py가 mysite Python package에 포함되어 mysite project directory가 제공 됩니다.이 프로젝트 페키지는 종종 특정 어플리케이션과 관련이 없는 CSS, templates와 같은 것을 포함되도록 확장 됩니다.

프로젝트의 루트 디렉토리 (manage.py를 포함하는 디렉토리)는 일반적으로 별도록 설치되지 않는 모든 프로젝트 응용 프로그램의 컨테이너입니다.

Application이라는 용어는 몇 가지 기능을 제공하는 Python 패키지를 나타냅니다. Application은 다양한 프로젝트에서 재사용 될 수 있습니다.

Application에는 models, views, templates, template tags, static files, URLs, middleware 등 몇개의 조합이 포함 됩니다.

일반적으로 INSTALLED_APPS설정을 사용하고 선택적으로 URLconf, MIDDLEWARE 설정 또는 템플릿 상속과 같은 다른 메커니즘과 함께 프로젝트에 연결 됩니다.

Django Application은 프레임 워크의 다양한 부분과 상호 작용하는 코드 세트라는 것을 이애하는 것이 중요합니다. Appliaction 객체는 없습니다. 그러나 Django가 주로 구성 및 내부 검사를 위해 설치된 Application과 상호 작용해야 하는 곳이 몇 군데 있습니다. 그렇기 때문에 Application registry는 설치된 각 Application의 AppConfig 인스턴스에서 메타 데이터를 유지 관리 합니다.

Project package가 Appliaction이라고 간주 할 수 없고, models이 있어야 한다고 제한하지 않습니다

## Configuring applications

Application을 구성하려면, AppConfig라는 서브 클래스를 만들고 INSTALLED_APPS에 서비스클래스의 경로를 넣으십시오

INSTALLED_APPS에 application module의 경로가 단순히 포함되어 있으면, Django는 module안에 default_app_config 변수를 체크합니다.

정의가 되어 있다면, 그것은 application의 AppConfig의 서브 클래스의 경로 입니다.

default_app_config가 없다면, Django에서 기본 AppConfig 클래스를 사용합니다.

default_app_config를 사용하면 django.contrib.admin과 같은 Django 1.7이전의 Application에서 사용자가 INSTALLED_APPS를 업데이트하지 않고도 AppConfig 기능을 opt-in 할 수 있습니다.

새로운 Application은 default_app_config를 피해야 합니다. 대신 INSTALLED_APPS에서 명시적으로 구성한 적절한 AppConfig 서브 클래스에  대한 점으로 구분 된 경로가 필요합니다.

### For application authors

'Rock n'roll '이라는 플러그 형 앱을 만드는 경우 관리자에게 적절한 이름을 제공하는 방법은 다음과 같습니다.

``` python
# rock_n_roll/apps.py

from django.apps import AppConfig

class RockNRollConfig(AppConfig):
    name = 'rock_n_roll'
    verbose_name = "Rock ’n’ roll"
```

application이 다음과 같이 기본적으로 AppConfig 서브클래스를 로드하도록 할 수 있습니다.

``` python 
# rock_n_roll/__init__.py

default_app_config = 'rock_n_roll.apps.RockNRollConfig'
```

INSTALLED_APPS에 'rock_n_roll'만 포함 된 경우 RockNRollConfig가 사용됩니다. 이를 통해 사용자가 INSTALLED_APPS 설정을 업데이트하지 않고 AppConfig기능을 사용할 수 있습니다. 이 사용 사례 외에도 default_app_config를 사용하지 말고 대신 다음 설명과 같이 INSTALLED_APPS에서 앱 구성 클래스를 지정하는 것이 가장 좋습니다,

물론 사용자에게 INSTALLED_APPS 설정에 'rock_n_roll.apps.RockNRollConfig'를 넣도록 지시할 수 도 있습니다. 다른 동작을 가진 여러 다른 AppConfig 서브 클래스를 제공하고 사용자가 INSTALLED_APPS 설정을 통해 하나를 선택할 수 있습니다.

권장되는 규칙은 구성 클래스를 앱이라는 프로그램의 하위 모듈에 배치하는 것입니다. 그러나 이것은 장고가 시행하지 않습니다.

이 구성이 적용되는 응용 프로그램을 결정하려면 Django의 name특성을 포함해야 합니다. AppConfig API 참조에 문서화 된 속성을 정의 할 있습니다.


>Note

>코드가 Application의 __init__.py에서 Application registry를 가져오는 경우 앱의 이름은 앱의 하위 모듈과 충돌합니다. 가장 좋은 방법은 해당 코드를 하위모듈로 이동하고 가져오는 것입니다. 해결 방법은 registry를 다른 이름으로 가져 오는 것입니다.

``` python 
from django.apps import apps as django_apps
```

### For application users

Anthology라는 프로젝트에서 "Rock n 'roll'을 사용하지만 대신 "Jazz Manouche"로 표시하려는 경우 고유 한 구성을 제공 할 수 있습니다.

 ``` python
 # anthology/apps.py

from rock_n_roll.apps import RockNRollConfig

class JazzManoucheConfig(RockNRollConfig):
    verbose_name = "Jazz Manouche"

# anthology/settings.py

INSTALLED_APPS = [
    'anthology.apps.JazzManoucheConfig',
    # ...
]
 ```

 ## Application configuration
 
 **class AppConfig[[source]('https://docs.djangoproject.com/en/2.2/_modules/django/apps/config/#AppConfig')]**

Appliation 구성 객체는 application의 메타데이터에 저장합니다. 몇개의 속성은 AppConfig 서버클래스에서 구성을 할 수 있습니다.
다른 것은 Django에 의해서 설정되고 읽기만 됩니다.

### Configurable attributes

**AppConfig.name**



















































