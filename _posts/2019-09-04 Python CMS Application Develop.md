---
title: Django Application Develop
date: 2019-09-04 11:37:28 -0400
categories: jekyll update
---

# Django Application Develop

``` bash
$> python manage.py startapp polls
```

apps.py

verbose_name을 추가한다.

verbose_name은 Django CMS에서 Application을 추가할때 카테코리를 생각할 수 있는 이름이다.

``` python
from django.apps import AppConfig

class PollsConfig(AppConfig):
    name = 'polls'
    verbose_name = 'Polls' // 추가
```

### absolute_import란?

``` python
>>> from __future__ import absolute_import    # 표준 모듈과 동일한 이름의 로컬 모듈을 사용 가능하게 해줌.
```

## \_\_future__ 모듈

> 참고 : https://jangjy.tistory.com/291

이 모듈은 파이썬2.x 에서 몇몇 기능들을 파이썬3.x 와 같이 사용 가능하게 만들어 주는 모듈이다.
흔히 사용되는 몇몇 기능은 아래와 같다.

#### print_function
``` python
>>> print "hello", "world"
hello world    # python 2.x
SyntaxError: invalid syntax    # python 3.x
```

``` python
>>> print ("hello", "world")
("hello", "world")    # python 2.x, 튜플이 출력 됨
hello world    # python 3.x
```

``` python
>>> from __future__ import print_function
print ("hello", "world")
hello world    # python 2.x & 3.x, 원하는 출력 가능.
```

#### Division
``` python
>>> from __future__ import division    # python 3 스타일의 나누기 지원.
```


#### Absolute Import
``` python
>>> from __future__ import absolute_import    # 표준 모듈과 동일한 이름의 로컬 모듈을 사용 가능하게 해줌.
```

### python module import

> 참고 : https://antilibrary.org/1260