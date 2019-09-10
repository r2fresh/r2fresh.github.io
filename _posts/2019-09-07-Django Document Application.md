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

Application에서 전체 경로 (에 : **'django.contrib.admin'**) 

이 속성은 구성을 적용하는 application을 정의 한다. 모든 AppConfig 서브 클래스에서 설정해야 한다.

이것은 Django에서 Unique 한 값이어야 한다.

**AppConfig.label**

Application의 짧은 이름 (예 : **'admin'**)

두개의 Application에서 label이 충돌하면 다시 지정해야 합니다. label을 지정 안 할경우 name의 마지막이 기본값이 됩니다.
또한 Python의 식별자로 가능해야 합니다.

label도 name과 마찬가지로 Uinique한 값이어야 합니다.

**AppConfig.verbose_name**

Application에서 사람이 읽을 수 있는 이름입니다.

이 속성의 기본값은 label.title()입니다.

아래와 같은 소스코드일 경우 Django Amdin Page에서는 'TOPPINGS_MODULES'로 표시가 된다
``` python
class ModulesConfig(AppConfig):
    name = 'toppings_modules'
    label = 'toppings_modules'
```

아래와 같은 소스코드일 경우 Django Amdin Page에서는 'TOPPINGS MODULES'로 표시가 된다
``` python
class ModulesConfig(AppConfig):
    name = 'toppings_modules'
    label = 'toppings_modules'
    verbose_name = 'Toppings Modules'
```

**AppConfig.path**

FileSystem에서 Application Directory의 경로 (예 : **'/usr/lib/pythonX.Y/dist'**)

대부분의 경우 Django에서 자동으로 감지하고 설정할 수 있지만 AppConfig 서브 클래스에서 클래스 속성으로 명시적으로 대체를 제공 할 수도 있습니다. 몇가지 상황에서는 이것이 필요합니다. 예를 들어 앱 패키지의 경로가 여러개인 네임스페이스 패키지인 경우

### Read-only attributes

**AppConfig.module**

Application의 Root 모듈 (예 : **<module 'django.contrib.admin' from 'django/contrib/admin/__init__.py'>**)

**AppConfig.models_module**

모델이 포함 된 모듈 (예 : **< 'django / contrib / admin / models.py'의 'django.contrib.admin.models'모듈>**)

Application에 모델 모듈이 포함되어 있지 않은 경우 없음 일 수 있습니다. **pre_migrate** 및 **post_migrate**와 같은 데이터베이스 관련 신호는 모델 모듈이있는 Application에 대해서만 생성됩니다.

### Methods

**AppConfig.get_modules()[[source]('https://docs.djangoproject.com/en/2.2/_modules/django/apps/config/#AppConfig.get_models')]**

Application의 반복 가능한 Model 클래스를 돌려줍니다.

app registry가 채워져야 합니다.

**AppConfig.get_model(model_name, require_ready=True)[[source]('https://docs.djangoproject.com/en/2.2/_modules/django/apps/config/#AppConfig.get_model')]**

주어진 model_name을 가진 모델을 반환합니다. model_name은 대소 문자를 구분하지 않습니다.

이 응용 프로그램에 해당 모델이 없으면 LookupError가 발생합니다.

require_ready 인수가 False로 설정되어 있지 않으면 앱 레지스트리가 완전히 채워 져야합니다. require_ready는 apps.get_model ()에서와 동일하게 작동합니다.

**AppConfig.ready()[[source]('https://docs.djangoproject.com/en/2.2/_modules/django/apps/config/#AppConfig.ready')]**

서브 클래스는 signals를 등록하는 것과 같은 고기화 테스크를 수행 할 수 있습니다. resistry가 완전히 채워지면 바로 호출 됩니다.

AppConfig 클래스가 정의 된 모듈 수준에서 모델을 가져올 수는 없지만 import 문 또는 get_model ()을 사용하여 ready ()로 모델을 가져올 수 있습니다.

모델 신호를 등록하는 경우 모델 클래스 자체를 사용하는 대신 문자열 레이블로 발신자를 참조 할 수 있습니다.

``` python 
from django.db.models.signals import pre_save

def ready(self):
    # importing model classes
    from .models import MyModel  # or...
    MyModel = self.get_model('MyModel')

    # registering signals with the model's string label
    pre_save.connect(receiver, sender='app_label.MyModel')
```

!Warning

위에서 설명한대로 모델 클래스에 액세스 할 수 있지만 ready () 구현에서 데이터베이스와 상호 작용하지 마십시오. 여기에는 쿼리를 실행하는 모델 메소드 (save (), delete (), 관리자 메소드 등)와 django.db.connection을 통한 원시 SQL 쿼리가 포함됩니다. ready () 메소드는 모든 관리 명령을 시작할 때 실행됩니다. 예를 들어 테스트 데이터베이스 구성이 프로덕션 설정과 별개이지만 manage.py 테스트는 여전히 프로덕션 데이터베이스에 대해 일부 쿼리를 실행합니다!

Note

일반적인 초기화 과정에서 Django는 ready 메소드를 한 번만 호출합니다. 그러나 일부 경우, 특히 설치된 응용 프로그램을 다루는 테스트에서 ready가 두 번 이상 호출 될 수 있습니다. 이 경우 dem 등원 (imdempotent) 메소드를 작성하거나 AppConfig 클래스에 플래그를 지정하여 정확히 한 번만 실행해야하는 코드가 다시 실행되지 않도록하십시오.

### Namespace packages as apps

`__init__.py` 파일이없는 Python 패키지는“네임 스페이스 패키지”라고하며 sys.path의 다른 위치에있는 여러 디렉토리에 분산 될 수 있습니다 (PEP 420 참조).

Django 응용 프로그램에는 Django (구성에 따라 다름)에서 템플릿, 정적 자산 등을 검색하는 단일 기본 파일 시스템 경로가 필요합니다. 따라서 네임 스페이스 패키지는 다음 중 하나에 해당하는 경우에만 Django 응용 프로그램 일 수 있습니다.

1. 네임 스페이스 패키지는 실제로 단일 위치 만 있습니다 (즉, 둘 이상의 디렉토리에 분산되지 않습니다).
2. 응용 프로그램을 구성하는 데 사용되는 AppConfig 클래스에는 경로 클래스 속성이 있습니다. 이는 Django가 응용 프로그램의 단일 기본 경로로 사용하는 절대 디렉토리 경로입니다.

이러한 조건 중 어느 것도 충족되지 않으면 Django는 부적절한 구성을 발생시킵니다.

## Applicaion registry

Application registry는 다음과 같은 공유 API를 제공합니다. 아래에 나열되지 않은 방법은 비공개로 간주되며 예고없이 변경 될 수 있습니다.

**apps.ready**

 `False`로 있다가 Registry가 완전히 채워지고 모든 AppConfig.ready() 메소드가 호출 된 후 `True`로 설정되는 Boolean 속성

**assps.get_app_configs()**

AppConfig의 인스턴스를 iterable하게 리턴합니다.

**apps.get_app_config(app_label)**

AppConfig의 인스턴스를 iterable하게 리턴합니다. 주어진 app_lable의 application의 AppConfig를 리턴합니다.

**apps.is_installed(app_name)**

지정된 name의 Application이 registry에 있는 확인합니다. app_name은 앱의 전체 이름입니다.
(예 : **django.contrib.admin**)

**apps.get_model(app_label, model_name, require_ready=True)**

지정된 app_label 및 model_name을 가진 모델을 리턴합니다. 바로 가기로서이 메소드는 app_label.model_name 형식의 단일 인수를 허용합니다. model_name은 대소 문자를 구분하지 않습니다.

이러한 응용 프로그램이나 모델이 없으면 LookupError가 발생합니다. 정확히 하나의 점을 포함하지 않는 단일 인수로 호출하면 ValueError가 발생합니다.

require_ready 인수가 False로 설정되어 있지 않으면 앱 레지스트리가 완전히 채워 져야합니다.

require_ready를 False로 설정하면 앱 레지스트리가 채워지는 동안, 특히 모델을 가져 오는 두 번째 단계에서 모델을 찾을 수 있습니다. 그러면 get_model ()은 모델을 가져 오는 것과 같은 효과를 갖습니다. 주요 사용 사례는 AUTH_USER_MODEL과 같은 설정으로 모델 클래스를 구성하는 것입니다.

require_ready가 False 인 경우, get_model ()은 앱 레지스트리가 완전히 채워질 때까지 완전히 작동하지 않을 수있는 모델 클래스 (예 : 역 접근자가 누락 될 수 있음)를 반환합니다. 따라서 require_ready를 기본값 인 True로 두는 것이 가장 좋습니다.

## Initialization process

### How applications are loaded

Django가 시작되면, django.setup()은 application registry를 채우기 위한 책임이 있습니다.

**setup(set_prefix=True)[[source]('https://docs.djangoproject.com/en/2.2/_modules/django/#setup')]**

Django 구성

* settings를 로딩
* 로깅을 설정
* set_prefix가 True 인 경우 URL 확인자 스크립트 접두어를 FORCE_SCRIPT_NAME으로 정의하고 정의한 경우 / 또는 그렇지 않으면 설정하십시오.
* application registry 초기화

이 함수는 자동으로 호출 된다.

* Django의 WSGI 지원을 통해 HTTP서버를 실행 할때
* management의 명령을 호출할 때

Application은 3단계로 초기화 됩니다. 각 단계에서 Django는 모든 Application을 INSTALLED_APPS 순서로 처리합니다.

1. 먼저 Django는 INSTALLED_APPS의 각 항목을 가져옵니다.
   
   만약 Django가 Application 구성 클래스일 경우 name 속성으로 정의 된 애플리케이의 루트 패키지를 가져 옵니다. Python 패키지 인 경우, Django는 기본 Application 구성을 만듭니다.

   이 단계에서 코드는 모델을 가져 오지 않아야 합니다.

   다시 말해, Application의 root package와 application의 구성 클래스는 어떤 모델이든 간접적으로 import해서는 안됩니다.

   엄밀히 말하면, Django는 일단 Appliation 구성이 로드 되면 모델을 가져올 수 있습니다. 그러나 INSTALLED_APPS 순서에 대한 불필요한 제약을 피하기 위해 단계에서는 모델을 가져오지 않는 것이 좋습니다.

   이단계가 완료되면 get_app_config()와 같은 애플리케이션 구성에서 작도하는 API를 사용할 수 있게 됩니다.

2. 그런 다음 Django는 각 Application의 모델 하위 모델을 가져오려고 시도 합니다.
   
   Application의 models.py 또는 models/\_\_init__.py에서 모델을 정의 하거나 가지고 와야 합니다.
   그렇지 않으면 이 시섬에서 Application Registry가 완전히 채워지지 않아 ORM이 오작동 될 수 있습니다.

   이 단계가 완료되면 get_model()과 같은 모델에서 작동하는 API를 사용할 수 있게 됩니다.

3. 마지막으로 Django는 각 Application의 구성의 ready() 메소드를 실행 합니다.

### Troubleshooting

여기에는 초기화 하는 동안 발생할 수 있는 몇가지 일반적인 문제가 있습니다. 

* **AppRegistryNotReady :** Application 구성 또는 모델 모듈을 가져 오면 Application Registry에 의존하는 코드가 트리거 될때 발생합니다.
  
  예를 들어 gettext ()는 앱 레지스트리를 사용하여 애플리케이션에서 번역 카탈로그를 찾습니다. 가져 오기 시간에 번역하려면 대신 gettext_lazy ()가 필요합니다. (gettext ()를 사용하면 버그가 발생합니다. 번역은 활성 언어에 따라 각 요청이 아닌 가져 오기 시간에 발생하기 때문입니다.)
  
  모델 모듈에서 가져 오기시 ORM을 사용하여 데이터베이스 쿼리를 실행하면이 예외가 트리거됩니다. 모든 모델을 사용할 수있을 때까지 ORM이 제대로 작동하지 않습니다.
  
  독립형 Python 스크립트에서 django.setup () 호출을 잊어 버린 경우에도이 예외가 발생합니다.

* **ImportError :** 이름을 가져올 수 없습니다 ... 가져 오기 시퀀스가 루프로 끝나는 경우에 발생합니다.
  
  이러한 문제를 해결하려면 모델 모듈 간의 종속성을 최소화하고 가져 오기시 가능한 적은 작업을 수행해야합니다. 임포트시 코드 실행을 피하기 위해 코드를 함수로 옮기고 결과를 캐시 할 수 있습니다. 결과가 처음 필요할 때 코드가 실행됩니다. 이 개념을 "게으른 평가"라고합니다.

* django.contrib.admin은 설치된 응용 프로그램에서 관리 모듈의 자동 검색을 자동으로 수행합니다. 이를 방지하려면 INSTALLED_APPS가 'django.contrib.admin'대신 'django.contrib.admin.apps.SimpleAdminConfig'를 포함하도록 변경하십시오.

















