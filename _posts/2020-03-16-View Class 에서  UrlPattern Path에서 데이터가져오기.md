---
title: View Class 에서  UrlPattern Path에서 데이터가져오기
date: 2020-03-16 09:30:28 -0400
categories: jekyll update
---

## URL을 사용하는 방법

urls.py

```python
url(r'^profile/(?P<user_id>\d+)/$', views.profile, name='qa_profiles'),
```

## PATH을 사용하는 방법