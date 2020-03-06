---
title: 다른 PC에서 로컬PC Django 웹 접근해서 확인하는 방법
date: 2020-03-06 09:30:28 -0400
categories: jekyll update
---

# 다른 PC에서 로컬PC Django 웹 접근해서 확인하는 방법

## How?

1. Django 설정 파일 settings.py를 찾는다.
2. settings.py에서 'ALLOWED_HOSTS 부분에 로컬PC의 ip주소를 아래와 같이 추가한다.

``` python
ALLOWED_HOSTS = [
    # localhost, 127.0.0.1 제외 
    'xxx,xxx,xxx,xxx'
]
...

3. 저정 후 아래와 같이 django 서버를 실행 합니다.

``` shell
$ > python manage.py runserver 0.0.0.0:8000
```

4. 다른 PC에서 `xxx,xxx,xxx,xxx:8080` IP를 입력하면 화면에서 보입니다.