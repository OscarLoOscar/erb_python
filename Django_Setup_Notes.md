Django 開發環境設定筆記本文件紀錄了在 macOS 環境下，結合 pyenv、virtualenvwrapper 及 Django 的開發環境配置流程。
1. Python 版本與虛擬環境管理建立特定版本的虛擬環境當系統預設 Python 版本（如 3.14）與專案需求（如 3.12）不符時，需指定路徑建立環境。
```Bash
使用 pyenv 的特定版本路徑來建立虛擬環境
```

### 假設想用 3.12.12 建立名為 erb8 的環境
```bash
virtualenv -p $(pyenv prefix 3.12.12)/bin/python ~/.virtualenvs/erb8
```

Common Command and Result:
```bash
workon erb8
```
Result : 啟動名為 erb8 的虛擬環境deactivate退出目前的虛擬環境
```bash
python --version
```
Result : 檢查目前環境使用的 Python 版本
```bash
pip list
```
Result : 查看目前環境已安裝的套件（如 Django）

2. Django 專案基礎配置安裝 **MUST HAVE**
```Bash
pip install django python-dotenv
```
Result : 環境變數管理 (.env)為保護敏感資料（如 SECRET_KEY），
需在專案根目錄（與 manage.py 同級）建立 .env 檔案。
.env 內容範例：Plaintext
```python
SECRET_KEY=django-insecure-your-secret-key-here
DEBUG=True
```
修改 settings.py 讀取設定在 settings.py 頂部加入讀取邏輯：
```Python
import os
from pathlib import Path
from dotenv import load_dotenv

BASE_DIR = Path(__file__).resolve().parent.parent

### 載入 .env
load_dotenv(os.path.join(BASE_DIR, '.env'))

### 套用變數
SECRET_KEY = os.getenv('SECRET_KEY')
DEBUG = os.getenv('DEBUG') == 'True'
```

3. 資料庫與伺服器運行初始化資料庫當出現 unapplied migration(s) 警告時執行，用於建立系統內建的 Table。
```Bash
python manage.py migrate
```
建立管理員
```Bash
python manage.py createsuperuser
```
啟動開發伺服器
```Bash
python manage.py runserver
```
---
<h3>template website</h3>

[[envato.com](https://elements.envato.com/)](https://elements.envato.com/web-templates/django)

---
### under Django , create a folder named 'pages'

```bash
python manage.py startapp pages
```

will have a folder named 'pages' ,and have __init__.py , apps.py and others files

migrations -> connect Database
__init__.py ->  must have , constructor
admin.py
apps.py -> class , have a PagesConfig
models.py -> Database structure
test.py -> test the modal
views.py -> control MVT

Node.js MVC -> Model (Database) + View(Template)[HTML] + Controller(Router(URL) and Function(Database/Fetching))
Django MVT -> Model(Database) + View(Function/Controller) + Template[HTML]
Django will have another file : URLS
---
```bash 
check version
python --version
```

```bash
pip install django
pip install python-dotenv
python manage.py runserver
```

```bash 
# 先 deactivate 依家呢個
deactivate

# 喺 virtualenvwrapper 嘅路徑起環境
virtualenv -p $(pyenv prefix 3.12.12)/bin/python ~/.virtualenvs/erb8

# 之後就可以隨時隨地用
workon erb8
```

```bash 
python manage.py migrate
```

```bash
python manage.py startapp pages
```

起完server
去settings.py
搵SECRET_KEY
起.env
copy 搵SECRET_KEY落去
再打
1. after create a folder named pages(whatever you like)
2. go to config.settings.py
3. Suggestion : rename 'INSTALLED_APPS' to DJANGO_APPS , and add APPLICATION_APPS
4. example: 
   
```python
   DJANGO_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
  ]

APPLICATION_APPS = ['pages.apps.PagesConfig']

INSTALLED_APPS = DJANGO_APPS + APPLICATION_APPS

ROOT_URLCONF = 'config.urls'

```

---

### Django首要：註冊->分流->做endpoint-> 開folder，copy:
```bash
python manage.py startapp pages
```
然後去views.py 做function(endpoint 既名：def xxx return render (YYY)), 去番templates 加folder, folder入面naming 加'_' : example : _footer.html, 
其他html打dummy野：<h1>listings</h1>，令佢做閉環，做test

emmet快速鍵：h1{search}

version 4.2 : listings:index
version 3.2 : listings (冇app_name定議第一次分流)
4.2先用到index

1. add a file named 'urls.py' under folder pages
```python=
from django.urls import path
from  . import views

app_name = 'pages'

urlpatterns = [
  path('',views.index,name = 'index'),
  path('about',views.about,name='about')
]
```

---

create a new Folder  <b>templates</b> , open a new Folder <b>pages</b> under <b>templates</b> ,
Then open a new file named <b>index.html</b>
copyed file , need to told Django : 
```python
TEMPLATES = [
    {
      ...,
  'DIRS': [],
  ...
    }
  TO
  TEMPLATES = [
    {
      ...,
  'DIRS': [os.path.join(BASE_DIR,'templates')],
  ...
```

After than , go to pages>views.py
```python 
def index(request):
  return HttpResponse('<h1>Hello World</h1>')
```

modify to 
```python
def index(request):
  return render(request,'pages/index.html')
```
<b>No need write templates/pages/index.html,since set 'DIRS': [os.path.join(BASE_DIR,'templates')]</b>

---
 copy完.html, 無css ,要create folder 'base' under 'templates' 放css
 navigation bar , footer 大情況會相同

.html入面，css放頭，js放尾

```mermaid
flowchart LR
    Browser -->|Parse HTML + JS| DOM["DOM<br/>h1 element"]
    Browser -->|Parse CSS| CSSOM["CSSOM<br/>h1 { ... }"]
```
### JS(動作) -> User可以等,已經有response出黎
### CSS已經存在，再load HTML，最後先load JS

create a folder named 'partials' under templates
#### create 3 file under folder 'partials': 
1._footer.html, 
2._navbar.html
3._topbar.html

then under 'base.html'
打
```html
      {% include 'partials/_topbar.html' %}
      {% include 'partials/_navbar.html' %}
      {% block content %}{%endblock %}
      {% include 'partials/_footer.html' %}
```
inside **body** **/body**

_topbar.html , 係剛剛開既.html file

然後再加
```javascript
    <script src="assets/js/jquery-3.3.1.min.js "></script>
    <script src="assets/js/bootstrap.bundle.min.js "></script>
    <script src="assets/js/main.js "></script>
```
係/body上面，/html上面

---
Back to index.html
```
係<!DOCTYPE html>上面打{% extends 'base.html'%}
delete 埋JS，base.html有既野，同埋<!DOCTYPE html>

<body>入面加 {% block content %}同 {% endblock %}

再delete埋<html lang="en">
  <body>
    </body>
</html>
```
---
Create 'about.html' under pages , parallel to 'index.html'

last , 改pages> views.py ，
```python
def about(request):
  # return HttpResponse('<h1>about</h1>')
  return render(request,'pages/about.html')
```
backend call 'media'
frontend call 'assets'

---

open a folder named 'static' under config , config > static

under config>settings.py
STATIC_URL 下面加
```python
STATIC_URL = 'static/'
STATIC_ROOT = os.path.join(BASE_DIR,'static')
STATICFILES_DIRS = [
    os.path.join(BASE_DIR,'config/static')
]
```
同理，呢個set左static，之後打個D under static path既file唔洗加'static'

去Terminal，打
```bash
python manage.py collectstatic
```

```
static入面會有admin folder == Correct
打command起個static file，static入面會有admin入面會有css/img/js folder，for admin 用
會有css，img，JS 原因：會scan晒成個project，有css，img，JS 記group埋，放係度
```

---
back to templates/base.html
<!DOCTYPE html>之前加
```python
{% load static %}
```

再改css and js
```python
    <link rel="stylesheet" href={% static "css/all.css" %} />
    <script src={% static "/js/jquery-3.3.1.min.js" %}></script>
```

```html
將<a class="nav-link" href="index.html">Home</a>
轉做
<a class="nav-link" href={% url 'pages:index' %}>Home</a>
```

```mermaid
flowchart LR
    image --> static
    image --> dynamic
```

---
改navbar做click就會著燈：
由
```html
<li class="nav-item active mr-3"> 
```
改成
```html
<li {% if '/' == request.path %} class="nav-item active mr-3" {% else %} class='nav-item me-3' {% endif %}>
```

如果改其他page，轉if之後個endpoint:
```html
<li {% if '/about' == request.path %} class="nav-item active mr-3" {% else %} class='nav-item me-3' {% endif %}>
```

---
如果想某d file 回到git既上一步：
```bash
git restore terminal/partials/_navbar.html
```

---

### how to solve the .jpg question(CSS問題) : 
1. 開無限
2. 開另一個browser
3. F5試緊既browser
4. 裝幾個browser

---

## Define Database
以呢個app為例：
Listings(一個醫生可以行幾間診所) --> Doctor <--> App <--> Model:DB Table/Data Field

OneToMany Relationship

---
---
### Download postgres
downlink:
```
https://postgresapp.com/downloads.html
```

after finish, click 
```
postgres
```

```postgres
\password postgres
```

suggest pw : Admin1234

```postgres
CREATE DATABASE clinic OWNER postgres;
```

list all database:
```postgres
\l or \list
```

Exit:
```postgres
\q
```

---
## How to connect database by Django?

1. Django 要 psycopg2 同 Postgres 溝通
```bash
pip install pstcopg2
# pip install psycopg2-binary
```
Result:
```
Collecting psycopg2-binary
  Downloading psycopg2_binary-2.9.11-cp314-cp314-macosx_11_0_arm64.whl.metadata (4.9 kB)
Downloading psycopg2_binary-2.9.11-cp314-cp314-macosx_11_0_arm64.whl (3.9 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.9/3.9 MB 339.1 kB/s  0:00:11
Installing collected packages: psycopg2-binary
Successfully installed psycopg2-binary-2.9.11
```

2. Go to settings.py
find DATABASES
3. 執行 Migration
最後，將 Django 內建嘅 Table (例如 User, Admin) 同埋你自定義嘅 Models 同步去 Postgres：
```Bash
python manage.py migrate
```

Result :
```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
  ```


  ```
  https://superset.apache.org/docs/6.0.0/configuration/databases
  ```

  ```
  PostgreSQL
  pip install psycopg2
  postgresql://<UserName>:<DBPassword>@<Database Host>/<Database Name>
  ```

---
---
ORM - Object Relation Model
唔洗打SQL，好多framework都用緊
改postgresql security：
1. postgresql database right click
2. Properties
3. Security
4. Privileges
5. Grantee , click '+'
6. Choose postgres
7. Privileges set All
8. Then save
---
---
.zshrc setting:
# set postgresql
export PG_HOME=/Application/Postgres.app/Contents/Version/18
export PATH=$PATH:$PG_HOME/bin
### linux :
#### terminal click : cd /lib/postgresql
#### export PG_HOME=/ib/postgresql/18
##### export PATH=$PATH:$PG_HOME/bin
##### sudo apt install libpq-dev python3

### macOS
#### Finder > Application > Postgresql(not PgAdmin4) > right click > 顯示套裝內容 > Contents > Versions > left click the version > bin > find 'psql'
#### If has 
---

1. 第一步：安裝 Library喺 Terminal 執行：
```bash
pip install dj-database-url
```
2. 第二步：修改 settings.py
喺 settings.py 頂部 import 之後，用以下格式套用條 String：

```bash
import dj_database_url
```
# version 1
```python
DATABASE = {
  'default' : {
    'ENGINE': 'django.db.database.postgresql',
    'NAME' : 'clinic',
    'USER':'postgres'
    'PASSWORD' : '1234'
    'HOST':'localhost'
  }
}
```
# version 2
# 將你條 String 放喺呢度
```python
DATABASE_URL = "postgresql://oscarlo:Admin1234@127.0.0.1:5432/clinic"

DATABASES = {
    'default': dj_database_url.config(
        default=DATABASE_URL,
        conn_max_age=600  # 可選：維持連線，增加效能
    )
}
```
3. 
```bash
python manage.py makemigrations
python manage.py migrate
```

```python
喺專案根目錄建立 .env 檔案：
DATABASE_URL=postgresql://oscarlo:xxxx@127.0.0.1:5432/clinic
喺 settings.py 
```

```Python
入面讀取環境變數：

import os
import dj_database_url

# 讀取系統或 .env 入面嘅 DATABASE_URL
DATABASES = {
    'default': dj_database_url.config(
        default=os.environ.get('DATABASE_URL')
    )
}
```

---
4. 去Doctors ,create models.py
```python
class Doctor(models.Model):
  name = models.CharField(max_length = 200)
  photo = models.ImageField('upload_to_photos/%Y/%m/%d')
  description = models.TeztField(blank=True)
  phone = models.CharField(max_length=20,blank='00000000')
  email = models.EmailField(max_length=50,unique=True,blank=False)
  is_mvp = models.BooleanField(default=True)
  hire_date = models.DateTimeField(auto_now_add =True)
  def __str__(self):
    return self.name

ImageField 唔無save張相，係save條URL
```

Check Field:
```bash
https://docs.djangoproject.com/en/6.0/ref/forms/fields/#:~:text=CharField%28%29
```

then will show error:
```
ERRORS:
doctors.Doctor.photo: (fields.E210) Cannot use ImageField because Pillow is not installed.
	HINT: Get Pillow at https://pypi.org/project/Pillow/ or run command "python -m pip install Pillow".
```

so need to 
```bash
pip install pillow
```

3. Go to listings> models.py
```python
from django.db import models
from doctors.models import Doctor
# Create your models here.
class Listing(models.Model):
  doctor = models.ForeignKey(Doctor,on_delete= models.DO_NOTHING)
  title = models.CharField(max_length=200)
  address = models.CharField(max_length=200)
  district = models.CharField(max_length=50)
  description = models.TextField(blank=True)
  services = models.TextField(blank=True)
  service = models.IntegerField()
  room_type = models.CharField(max_length=200,default='')
  rooms = models.CharField(max_length=2)
  profession = models.CharField(max_length=200,default='')
  photo_main = models.ImageField(upload_to = 'photos/%Y/%d',blank=True)
  photo_1 = models.ImageField(upload_to = 'photos/%Y/%d',blank=True)
  photo_2 = models.ImageField(upload_to = 'photos/%Y/%d',blank=True)
  photo_3 = models.ImageField(upload_to = 'photos/%Y/%d',blank=True)
  photo_4 = models.ImageField(upload_to = 'photos/%Y/%d',blank=True)
  is_published = models.BooleanField(default=True)
  list_date = models.DateTimeField(auto_now_add=True) # 有D version會error你，所以要係config > settings.py set TIME_ZONE = "Asia/Hong_Kong"
  def __str__(self):
    return self.title
```


---

要Build App:
```bash
python manage.py startapp clinic_app(目標folder name)
```
1. define the data structure: 暫時放listings.models.py
```python
class Patient(models.Model):
  name = models.CharField(max_length=100)
  age = models.InterField()
  phone = models.CharField(max_length=20)
  created_at = models.DataTimeField(auto_now_add=True)

  def __str__(self):
    return self.name
```

```bash
# 1. 根據你 models.py 嘅內容整一份list
python manage.py makemigrations
```
#### Result : 
```bash
Migrations for 'doctors':
  doctors/migrations/0001_initial.py
    + Create model Doctor
Migrations for 'listings':
  listings/migrations/0001_initial.py
    + Create model Listing
```

Will have a folder named 'migrations'
0001_initial.py -> define the settings of this model [Metaprogramming : gen another programming(not only django/python)]

can regenerate this folder again , new file will named '0002_initial'
DONT delete file 0001_initial.py , postgresql will hand

```mermaid 
erDiagram
    Doctor ||--o{ Listing : "manages"

    Doctor {
        int id PK
        string name
        file photo
        string phone 
        string email UK
    }

    Listing {
        int id PK
        string title
        string address
        string doctor_id FK
    }
```


**唔洗自己寫ID，因為in 0001_initial.py 會見到('id',models.BigAutoField)
config > settings DEFAULT_AUTO_FIELD define左**
```python
One to Many Relationship:
since :  'models.ForeignKey'
```

```python
One to One:
models.OneToOneField(
        Place,
        on_delete=models.CASCADE,
        primary_key=True,
    )
```
Reference : https://docs.djangoproject.com/en/6.0/topics/db/examples/one_to_one/

```python
Many to Many:
models.py define 2 class,
是但一個class加'models.ManyToMany'
```

#### 如果想用其他Field做primary key,加'primary_key=True'： 
```python
from django.db import models

# Create your models here.
class Doctor(models.Model):
  name = models.CharField(max_length = 200)
  photo = models.ImageField(upload_to='photos/%Y/%m/%d')
  description = models.TextField(blank=True)
  phone = models.CharField(max_length=20, primary_key=True)
  email = models.EmailField(max_length=50,unique=True,blank=False)
  is_mvp = models.BooleanField(default=True)
  hire_date = models.DateTimeField(auto_now_add =True)
  def __str__(self):
    return self.name
```
---

```bash
# 2. Build Table
python manage.py migrate
```
#### Result :
Operations to perform:
  Apply all migrations: admin, auth, clinic_app, content types, sessions
Running migrations:
  Applying clinic_app.0001_initial... OK

---

Checking : 
Go to PgAdmin4,
Schemas > Table > django_migrations
In this case , will see : 
app : doctors and listings
name : both are 0001_initial

---

Define another class , its does not affect your class before (Optional) , under listings > models.py now
```python
  class Meta:
    ordering = ['-list_date']
    indexes = [models.Index(field = ['list_date'])]
    def __str__(self):
      return self.title
```

Sort by descending order now , since '-'
-> thats mean **can not** modify in PgAdmin4

Every Time need to update : 
```
python manage.py makemigrations
```
Result:
```bash
Migrations for 'listings':
  listings/migrations/0002_alter_listing_options_and_more.py
    ~ Change Meta options on listing
    + Create index listings_li_list_da_153ce3_idx on field(s) list_date of model listing
```
---

#### Login:
1. 
```bash
python manage.py createsuperuser
```
2. need to add a name and password of this user
3. email 亂打is ok
4. go to localhost:8000/admin
5. login

Django administration 係Django Virtual Env度，唔洗自己寫

**可以係session度set expiry date係幾耐**
---

將database註冊係admin度：
back to vscode
doctors > admin.py
```python
from django.contrib import admin
from .models import Doctors
# Register your models here.
class DoctorsAdmin(admin.ModalAdmin):
  list_display = 'name','email','is_mvp','hire_date'
  list_display_links = 'name','email'
  list_editable = 'is_mvp'
  search_fields = 'name','email'
  list_per_page = 25

admin.site.register(Doctors,DoctorsAdmin)
```
---
Go to config > settings.py
```python
MEDIA_ROOT = os.oath.join(BASE_DIR,'media')
MEDIA_URL = '/media/'
```

Go to config>urls.py
add path into 'urlpatterns'

```python
from django.contrib import admin
from django.urls import path,include
from django.conf.urls.static import static
from django.conf import settings
urlpatterns = [
    path('',include('pages.urls',namespace='pages')),
    path('admin/', admin.site.urls),
    path('listings/', include('listings.urls',namespace='listings')) 
] + static(settings.MEDIA_URL,document_root=settings.MEDIA_ROOT)

admin.site.site_header = 'Clinic Administration'
admin.site.site_title = 'Clinic Admin Portal'
admin.site.index_template = 'Welcome to Clinic Portal'

```

Then go to listings > admin.py
```python
from django.contrib import admin
from .models import Listing

# Register your models here.
class ListingAdmin(admin.ModelAdmin):
  list_display = 'id','title','district','is_published','rooms','doctor',
  list_display_links='id','title'
  list_editable = 'is_published','rooms'
  search_fields = 'title','district','doctor_name'
  list_per_page=25

admin.site.register(Listing,ListingAdmin)
```

Docker : https://www.portainer.io
https://www.digitalocean.com

搵食既話，相/片放：cloudinary

---
---
1. Create
```python
Read 
Python

all_patients = Patient.objects.all()

patient = Patient.objects.get(id=1)

young_patients = Patient.objects.filter(age__lt=30) 
```

4. Read 
   
```Python
all_patients = Patient.objects.all()

patient = Patient.objects.get(id=1)

young_patients = Patient.objects.filter(age__lt=30) 
```

5. Update 

```Python
patient = Patient.objects.get(name="Oscar")

patient.age = 26
patient.phone = "88888888"

patient.save()
```

6. Delete 

```Python
patient = Patient.objects.get(id=1)
patient.delete()

Patient.objects.filter(age__gt=60).delete()
```

7. Test:

```bash
   最快試 CRUD 嘅方法係用 Django Shell：

喺 Terminal 輸入：

python manage.py shell
```

```bash
want to quit 'python manage.py shell'

exit() or quit()
```