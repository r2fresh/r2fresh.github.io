---
title: Django JsonResponse
date: 2019-12-11 15:42:28 -0400
categories: jekyll update
---

See below the class signature:

``` python
JsonResponse(data, encoder, safe, json_dumps_params, **kwargs)
```


https://simpleisbetterthancomplex.com/tutorial/2016/07/27/how-to-return-json-encoded-response.html


``` python
class Board(View):

    model = Board

    def get(self, request): 

        logger.info( type(self.model) )
        # <class 'django.db.models.base.ModelBase'>

        logger.info( type(self.model.objects) )
        # <class 'django.db.models.manager.Manager'>

        logger.info( type(self.model.objects.all()) )
        # <class 'django.db.models.query.QuerySet'>
        return HttpResponse(isinstance(self.model, dict))
```

https://docs.djangoproject.com/en/dev/ref/models/querysets/


Django model 클래스는

class django.db.models.base.ModelBase

Django queryset 클래스는 

class 

