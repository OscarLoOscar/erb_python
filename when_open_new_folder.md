https://www.youtube.com/watch?v=pM9IO3FCULQ

<h2>WaiLung version create python project</h2>

1. add .gitignore
2. go to https://www.toptal.com/developers/gitignore/api/django to copy the content of .gitignore
3. why we need .gitignore? we need to git file to github ,we dont need *.log, *.pot , *.pyc , __pycache__/ , local_settings.py ,db.sqlite3 , db.sqlite3-journal , media
4. no need keep trick :
# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

and so on

5. keep trick source only , then git push to github
6. python into pyend , pyend into ~ , project同pyenv無關係 >> why we need to do this ? Separate these thing , no need to consider project and python version. Reuse the pyenv for erb9,erb10,for other project use
7. Install Django
8. 
```bash
pip install django==5.2
```
9. .whl >> compiled byte code , you can not see the source code
10. 
```bash
pip freeze
```
ypu will see some folders here
11. we can create project now, we use these command one time
```bash
django-admin startproject config . (django-admin startproject project_name) . means under folder erb8 
```
12. will have manage.py and config folder , it have 5 .py files inside : __init__.py , asgi.py , settings.py , urls.py , wsgi.py
manage.py = 啓動file
__init__.py = 每個folder都當class看待
asgi.py = deploy 先用，寫real time既backend（whatsapp, wechat) 要馬上investigate用 
settings.py = Django命脈,唯一知Django version地方,course會番呢知file around 20times 
urls.py = router，RestfulAPI
wsgi.py = 寫static page

13. 
```bash
python manage.py runserver
```
result :
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
December 16, 2025 - 09:48:41
Django version 5.2, using settings 'config.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.

WARNING: This is a development server. Do not use it in a production setting. Use a production WSGI or ASGI server instead.
For more information on production servers see: https://docs.djangoproject.com/en/5.2/howto/deployment/

14. open another iterm2
15. Go to settings.py
```python
import os
from pathlib import Path
from dotenv import load_dotenv

BASE_DIR = Path(file).resolve().parent.parent
load_dotenv(os.path.join(BASE_DIR, '.env'))
SECRET_KEY = os.getenv('SECRET_KEY')
DEBUG = os.getenv('DEBUG') == 'True'

```

16. after create a folder named pages(whatever you like)
   16.1 go to config.settings.py
   16.2 Suggestion : rename 'INSTALLED_APPS' to DJANGO_APPS , and add APPLICATION_APPS
   16.3 add a file named 'urls.py'
```python=
from django.urls import path
from  . import views

app_name = 'pages'

urlpatterns = [
  path('',views.index,name = 'index'),
  path('about',views.about,name='about')
]
```