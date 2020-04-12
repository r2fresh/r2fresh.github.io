---
title: Django에서 Json 만들기
date: 2020-03-14 09:30:28 -0400
categories: jekyll update
---

## QuerySet 만들기

### 기본 QuerySet 만들기

* model.objects.all() : 테이블 데이터 전부
* model.objects.get(조건) : 하나의 열만 가져옴
* model.objects.filter(조건) : 조건에 맞는 열들을 가져옴
* model.objects.exclude(조건) : 조건에 맞지 않는 열들을 가져옴

### QuerySet 가공하기

* values(칼럼명) : object의 컬럼명만 뽑아서 반환함 values()로 사용할 경우 all()과 같은 의미가 됨
* annotate(쿼리) : 칼럼명에 다른 이름을 붙이거나 새 컬럼을 만들어 붙일 때 사용

```python
questions.filter(user=user).annotate(num_answers=Count('answer'))` 
```

### 대표적으로 사용하는 함수

* Count : 필드 개수를 샘. 위처럼 외래키의 개수를 샐 때 유용
* F : 필드값을 가져옴. django에서는 외래키에 접근하기 위해서는 field__foreign_field의 형태로 언더바를 두개씩 이어야 하기 때문에 컬럼명을 재등록할 때 사용하기에 유용함

## list 만들기

만들어진 QuerySet을 `list(QuerySet)`하면 dict로 구성된 list가 만들어 진다.

## JSON 만들기

### JsonResponse를 이용한 방법

```python
from django.http import JsonResponse

jsonData = JsonResponse(리스트 타입 객체, safe=False, json_dumps_params={'ensure_ascii': False})

return jsonData
```

### json을 이용한 방법
```python
import json
from django.core.serializers.json import DjangoJSONEncoder
from django.http import HttpResponse

jsonData = json.dumps(리스트 타입 객체, ensure_ascii=False, cls=DjangoJSONEncoder)

return HttpResponse(jsonData);
```
여기서 cls는 `json.JSONEncoder`을 사용해도 되지만 `datetime`으로 parsing의 에러가 발생할수도 있어 Django에서 제공하는 것을 사용한다.

### DjangoJSONEncoder를 사용하지 않는 방법
```python
import json
import datetime
from django.http import HttpResponse

def json_default(value): 
    if isinstance(value, datetime.date): 
        return value.strftime('%Y-%m-%d') 
    raise TypeError('not JSON serializable')

...

jsonData = json.dumps(리스트 타입 객체, ensure_ascii=False, default=json_default)

return HttpResponse(jsonData);
```

