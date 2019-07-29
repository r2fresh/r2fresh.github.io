---
title: Django Error List
date: 2019-07-28 11:37:28 -0400
categories: jekyll update
---

## Django Error 1

python을 Migrate를 하라고 할때 실행을 하면 아래와 같이 에러가 발생 할 경우가 있다.

그럴경우 Python은 맨 아래의 에러를 보고 에러가 발생한 파일을 찾아가서 수정을 하고 진행을 해야 한다.

```bash
$> python manage.py migrate
Traceback (most recent call last):
  File "manage.py", line 22, in <module>
    execute_from_command_line(sys.argv)
  File "/Users/r2fresh78/.pyenv/versions/3.6.1/lib/python3.6/site-packages/django/core/management/__init__.py", line 364, in execute_from_command_line
    utility.execute()
  File "/Users/r2fresh78/.pyenv/versions/3.6.1/lib/python3.6/site-packages/django/core/management/__init__.py", line 338, in execute
    django.setup()
  File "/Users/r2fresh78/.pyenv/versions/3.6.1/lib/python3.6/site-packages/django/__init__.py", line 27, in setup
    apps.populate(settings.INSTALLED_APPS)
  File "/Users/r2fresh78/.pyenv/versions/3.6.1/lib/python3.6/site-packages/django/apps/registry.py", line 108, in populate
    app_config.import_models()
  File "/Users/r2fresh78/.pyenv/versions/3.6.1/lib/python3.6/site-packages/django/apps/config.py", line 202, in import_models
    self.models_module = import_module(models_module_name)
  File "/Users/r2fresh78/.pyenv/versions/3.6.1/lib/python3.6/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 978, in _gcd_import
  File "<frozen importlib._bootstrap>", line 961, in _find_and_load
  File "<frozen importlib._bootstrap>", line 950, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 655, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 678, in exec_module
  File "<frozen importlib._bootstrap>", line 205, in _call_with_frames_removed
  File "/Users/r2fresh78/workspace/r2/edu/django-playlist/djangonautic/articles/models.py", line 4, in <module>
    class Article(models.Model):
  File "/Users/r2fresh78/workspace/r2/edu/django-playlist/djangonautic/articles/models.py", line 8, in Article
    date = models.DateTileField(auto_now_add=True)
AttributeError: module 'django.db.models' has no attribute 'DateTileField'
```

해결 방법은 맨 아래에서 보여주는 models.py의 팡리에서 DateTileField 부분을 DateTimeField로 변경하면 migrate도 문제없이 된다.

## Django Error 2

Django에서 `python manage.py migrate`로 실행 시 아래와 같이 에러 발생

```
$> python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  No migrations to apply.
  Your models have changes that are not yet reflected in a migration, and so won't be applied.
  Run 'manage.py makemigrations' to make new migrations, and then re-run 'manage.py migrate' to apply them.
```

먼저 `python manage.py makemigrations`를 입력

```
$> python manage.py makemigrations
Migrations for 'articles':
  articles/migrations/0001_initial.py
    - Create model Article
```

다시 `python manage.py migrate`을 실행

```
$> python manage.py migrate
Operations to perform:
  Apply all migrations: admin, articles, auth, contenttypes, sessions
Running migrations:
  Applying articles.0001_initial... OK
```
