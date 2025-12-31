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

Result : 啟動名為 erb8 的虛擬環境 deactivate 退出目前的虛擬環境

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

will have a folder named 'pages' ,and have **init**.py , apps.py and others files

migrations -> connect Database
**init**.py -> must have , constructor
admin.py
apps.py -> class , have a PagesConfig
models.py -> Database structure
test.py -> test the modal
views.py -> control MVT

```mermaid
flowchart TD
    subgraph Config_Folder [1. Project Config Folder]
        direction TB
        Init[__init__.py<br/>Suite Initialization]
        Settings[settings.py<br/>總管：Data Base/App lists/key]
        WSGI_ASGI[wsgi.py / asgi.py<br/>External Window: Receives Browser Requests]
        MainURL[urls.py<br/>Main Route: Large Map]
    end

    subgraph App_Folder [2. App Folder Function: clinic_app / pages]
        direction TB
        AppURL[urls.py<br/>Sub-Route: Regional Map]
        View[views.py<br/>Logical Processing：Run Function]
        Model[models.py<br/>Data定義：如 Patient Class]
        Template[templates/<br/>HTML Template Panel]
    end

    %% 流程連線
    Browser((Browser)) -- 1. Send Request --> WSGI_ASGI
    WSGI_ASGI -. Read setting .-> Settings
    WSGI_ASGI -- 2. Find the path --> MainURL

    MainURL -- "3. include('clinic_app.urls')" --> AppURL
    AppURL -- 4. point to specific View --> View

    View -- 5. Read data資料 --> Model
    View -- 6. Write data --> Template
    Template -- 7. Return HTML --> Browser

    %% Comment
    style Config_Folder fill:#f5f5f5,stroke:#333,stroke-dasharray: 5 5
    style App_Folder fill:#e1f5fe,stroke:#01579b
```

---

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

1. 起完 server
2. 去 settings.py
3. 搵 SECRET_KEY
4. 起.env
5. copy 搵 SECRET_KEY 落去
6. 再打

```bash
1. after create a folder named pages(whatever you like)
2. go to config.settings.py
3. Suggestion : rename 'INSTALLED_APPS' to DJANGO_APPS , and add APPLICATION_APPS
```

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

### Django 首要：註冊->分流->做 endpoint-> 開 folder，copy:

```bash
python manage.py startapp pages
```

然後去 views.py 做 function(endpoint 既名：def xxx return render (YYY)), 去番 templates 加 folder, folder 入面 naming 加'\_' : example : \_footer.html,
其他 html 打 dummy 野：<h1>listings</h1>，令佢做閉環，做 test

emmet 快速鍵：h1{search}

version 4.2 : listings:index
version 3.2 : listings (冇 app_name 定議第一次分流)
4.2 先用到 index

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

create a new Folder <b>templates</b> , open a new Folder <b>pages</b> under <b>templates</b> ,
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

copy 完.html, 無 css ,要 create folder 'base' under 'templates' 放 css
navigation bar , footer 大情況會相同

.html 入面，css 放頭，js 放尾

```mermaid
flowchart LR
    Browser -->|Parse HTML + JS| DOM["DOM<br/>h1 element"]
    Browser -->|Parse CSS| CSSOM["CSSOM<br/>h1 { ... }"]
```

### JS(動作) -> User 可以等,已經有 response 出黎

### CSS 已經存在，再 load HTML，最後先 load JS

create a folder named 'partials' under templates

#### create 3 file under folder 'partials':

1.\_footer.html,
2.\_navbar.html
3.\_topbar.html

then under 'base.html'
打

```html
{% include 'partials/_topbar.html' %} {% include 'partials/_navbar.html' %} {%
block content %}{%endblock %} {% include 'partials/_footer.html' %}
```

inside **body** **/body**

\_topbar.html , 係剛剛開既.html file

然後再加

```javascript
    <script src="assets/js/jquery-3.3.1.min.js "></script>
    <script src="assets/js/bootstrap.bundle.min.js "></script>
    <script src="assets/js/main.js "></script>
```

係/body 上面，/html 上面

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

last , 改 pages> views.py ，

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

同理，呢個 set 左 static，之後打個 D under static path 既 file 唔洗加'static'

去 Terminal，打

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

再改 css and js

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

改 navbar 做 click 就會著燈：
由

```html
<li class="nav-item active mr-3"></li>
```

改成

```html
<li {% if '/' == request.path %} class="nav-item active mr-3" {% else %}
class='nav-item me-3' {% endif %}>
```

如果改其他 page，轉 if 之後個 endpoint:

```html
<li {% if '/about' == request.path %} class="nav-item active mr-3" {% else %}
class='nav-item me-3' {% endif %}>
```

---

\_footer.html :

```html
<footer id="main-footer" class="py-4 bg-primary text-white text-center">
  Copyright &copy;
  <span class="year"></span> BC Health Care
</footer>
```

\_topbar.html :

```html
<section id="top-bar" class="p-3">
  <div class="container">
    <div class="row">
      <div class="col-md-4"><i class="fas fa-phone"></i> (852)-555-5555</div>
      <div class="col-md-4">
        <i class="fas fa-envelope-open"></i> contact@healthcare.co
      </div>
      <div class="col-md-4">
        <div class="social text-right">
          <a href="#">
            <i class="fab fa-twitter"></i>
          </a>
          <!-- you can add external link to replace # -->
          <a href="#">
            <i class="fab fa-facebook"></i>
          </a>
          <a href="#">
            <i class="fab fa-linkedin"></i>
          </a>
          <a href="#">
            <i class="fab fa-instagram"></i>
          </a>
          <a href="#">
            <i class="fab fa-pinterest"></i>
          </a>
        </div>
      </div>
    </div>
  </div>
</section>
```

\_navbar.html :

```html
{% load static %}
<nav class="navbar navbar-expand-lg navbar-dark bg-primary sticky-top">
  <div class="container">
    <!-- <a class="navbar-brand" href="index.html"> -->
    <a class="navbar-brand" href={% url 'pages:index' %}>
    <img src={% static "img/logo.png" %} class="logo" alt="" />
  </a>
    <button
    class="navbar-toggler"
    type="button"
    data-toggle="collapse"
    data-target="#navbarNavAltMarkup"
    >
    <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
      <ul class="navbar-nav">
        <!-- <li class="nav-item active mr-3"> -->
          <!-- hard 左active -->
          <!-- 加statement , python only-->
        <li {% if '/' == request.path %} class="nav-item active mr-3" {% else %} class='nav-item me-3' {% endif %}>
          <!-- <a class="nav-link" href="index.html">Home</a> -->
          <!-- pages 係跟app_name="something" -->
          <a class="nav-link" href={% url 'pages:index' %}>Home</a>
        </li>
          <!--  <li class="nav-item mr-3"> -->
          {% comment %} <!--  將 {% if '/' == request.path %} 改番做目標page:/about就做到click入個page,變active --> {% endcomment %}
        <li {% if '/about' == request.path %} class="nav-item active mr-3" {% else %} class='nav-item me-3' {% endif %}>
          <a class="nav-link" href={% url 'pages:about' %}>About</a>
        </li>
        <li {% if '/listings' == request.path %} class="nav-item active mr-3" {% else %} class='nav-item me-3' {% endif %}>
        <li class="nav-item mr-3">
          {% comment %} fix {% endcomment %}
          <a class="nav-link" href={% url 'listings:index' %}>
            Inpatient medical services</a
          >
        </li>
      </ul>

      <ul class="navbar-nav ml-auto">
        <li class="nav-item mr-3">
          {% comment %} fix {% endcomment %}
          <a class="nav-link" href="register.html">
            <i class="fas fa-user-plus"></i> Register</a
          >
        </li>
        <li class="nav-item mr-3">
          {% comment %} fix {% endcomment %}
          <a class="nav-link" href="login.html">
            <i class="fas fa-sign-in-alt"></i>

            Login</a
          >
        </li>
      </ul>
    </div>
  </div>
</nav>
```

如果想某 d file 回到 git 既上一步：

```bash
git restore terminal/partials/_navbar.html
```

如果想搵 git 既 diff(改動過既 file)

```bash
git diff --name-only
```

---

### how to solve the .jpg question(CSS 問題) :

1. 開無限
2. 開另一個 browser
3. F5 試緊既 browser
4. 裝幾個 browser

---

## Define Database

以呢個 app 為例：
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

Username : admin
suggest pw : 1234

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
唔洗打 SQL，好多 framework 都用緊
改 postgresql security：

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

1. 第一步：安裝 Library 喺 Terminal 執行：

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

4. 去 Doctors ,create models.py

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

https://pypi.org

so need to

```bash
pip install pillow
```

3. Go to listings> models.py , 打 table 要既 field, same on Java Entity

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

要 Build App:

```bash
python manage.py startapp clinic_app(目標folder name)
```

1. define the data structure: 暫時放 listings.models.py

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

**唔洗自己寫 ID，因為 in 0001_initial.py 會見到('id',models.BigAutoField)
config > settings DEFAULT_AUTO_FIELD define 左**

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

#### 如果想用其他 Field 做 primary key,加'primary_key=True'：

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

#### Sort by descending order now , since '-'

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
3. email 亂打 is ok
4. go to localhost:8000/admin
5. login

Django administration 係 Django Virtual Env 度，唔洗自己寫

## **可以係 session 度 set expiry date 係幾耐**

將 database 註冊係 admin 度：
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
# views.py
def get_patients(request):
    # 讀取全部
    all_p = Patient.objects.all()
    # 條件過濾 (年齡小於30)
    young_p = Patient.objects.filter(age__lt=30)
    # 獲取單一資料 (id=1)，不存在則報404
    patient = get_object_or_404(Patient, id=1)
    return render(request, 'index.html', {'patients': all_p})
```

2. Read

```Python
# views.py
def create_patient(request):
    if request.method == 'POST':
        # 從 POST 請求中取值
        name = request.POST.get('name')
        # 直接存入資料庫
        Patient.objects.create(name=name, age=25)
        return redirect('index')
```

3. Update

```Python
# views.py
def update_patient(request, id):
    patient = get_object_or_404(Patient, id=id)
    if request.method == 'POST':
        # 修改物件屬性
        patient.name = request.POST.get('name')
        # 執行 Save 更新回資料庫
        patient.save()
        return redirect('index')
```

4. Delete

```Python
# views.py
def delete_patient(request, id):
    patient = get_object_or_404(Patient, id=id)
    # 只有當 POST 提交時才執行刪除
    if request.method == 'POST':
        patient.delete()
        return redirect('index')
```

5. Test:

```bash
   最快試 CRUD 嘅方法係用 Django Shell：

喺 Terminal 輸入：

python manage.py shell
```

```bash
want to quit 'python manage.py shell'

exit() or quit()
```

---

29/12/2025
Model 同 app -> admin.py 有關係 -> list_display 係 Django 要有 -> 決定點 display 呢個 database

```python
class DoctorsAdmin(admin.ModelAdmin):
  list_display = 'name','email','is_mvp','hire_date'
  list_display_links = 'name','email'
  list_editable = 'is_mvp',
  search_fields = 'name',
  list_per_page = 25
```

templates > pages > index.html
依家想係呢度放 listing 既 data 入黎

example : <h1 class="display-4 mb-4">Clinic Searching Just Got So Easy</h1>

pages > views.py > highlight / delete

```python
def index(request):
  return render(request,'pages/index.html')
```

```mermaid
flowchart TD

    Client((Client)) -- 輸入 [ABC.com/about]
    (https://ABC.com/about) --> DNS{DNS Server}
    DNS -- 回傳 IP: 297.297.297.297 --> Client

    subgraph Server [Django Server]
        direction TB
        Entry[Client 接收 Request ,
        example : URLS/listings/1,
        listings就係resource ] --> URLConf[一次分流: urls.py]
        URLConf -- 比對路徑 /about --> View[二次分流: views.py f]

        View --> DB[(Database / Model)]
        DB -- 執行 SQL 回傳資料 --> View

        View --> Template[Template + Data]
        Template --> Response[生成 HTML Response]
    end

    Response --> Client
```

1. **括號形狀**：
   - `(( ))` 圓形（Client Side）。
   - `{ }` 決策 or 查詢（DNS）。
   - `[( )]` Database。
2. **方向優化**：使用 `flowchart TD` (Top to Down) cooperate to `subgraph`，令查詢 Database 的 flow be like 開發 logic（from URL -> View -> DB -> Template）。
3. **Content 補強**：
   - **一次分流**：對應 `urls.py`（Go which App/Page）。
   - **二次分流**：對應 `views.py`（Implement which Function）。

---

工具：方便 debug[Development 用]

```python
pip install django-debug-toolbar
```

https://django-debug-toolbar.readthedocs.io/en/latest/installation.html

```bash
1. First, ensure that 'django.contrib.staticfiles' is in your INSTALLED_APPS setting, and configured properly:

INSTALLED_APPS = [
    # ...
    "django.contrib.staticfiles",
    # ...
]

STATIC_URL = "static/"

2. Second, ensure that your TEMPLATES setting contains a DjangoTemplates backend whose APP_DIRS options is set to True:

TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "APP_DIRS": True,
        # ...
    }
]
Install the App

Add "debug_toolbar" to your INSTALLED_APPS setting:

3. INSTALLED_APPS = [
    # ...
    "debug_toolbar",
    # ...
]

4. Add the URLs in config urls.py

Add django-debug-toolbar’s URLs to your project’s URLconf:

from django.urls import include, path
from debug_toolbar.toolbar import debug_toolbar_urls

urlpatterns = [
    # ... the rest of your URLconf goes here ...
] + debug_toolbar_urls()
```

Result :

```python
urlpatterns = [
    path('',include('pages.urls',namespace='pages')),
    path('admin/', admin.site.urls),
    path('listings/', include('listings.urls',namespace='listings'))
] + static(settings.MEDIA_URL,document_root=settings.MEDIA_ROOT)+ debug_toolbar_urls()
```

```python

DJANGO_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',# 裝完 django-debug-toolbar
    'clinic_app', # 加啱啱起既startapp XX(project name)
]

# 註冊地方,先可以係localhost/admin見到
APPLICATION_APPS = ['pages.apps.PagesConfig',
                    'doctors.apps.DoctorsConfig',
                    'listings.apps.ListingsConfig'
                    ]

THIRD_PARTY_APPS = ['debug_toolbar']# 裝完 django-debug-toolbar手動加, for development用

INSTALLED_APPS = DJANGO_APPS + APPLICATION_APPS + THIRD_PARTY_APPS

TEMPLATES = [
    {   'debug_toolbar.middleware.DebugToolbarMiddleware',
        'BACKEND': 'django.template.backends.django.DjangoTemplates',# 裝完 django-debug-toolbar
        # 'DIRS': [],
        'DIRS': [os.path.join(BASE_DIR,'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

INTERNAL_IPS = [
    "127.0.0.1",
]
```

---

Go to pages > view.py
modify to

```python
from listings.models import Listing

def index(request):
  listings = Listing.objects.all() # 大階Listing，拎data model，all() -> 同DB溝通，拎晒所有data
  content = {"listings" : listings}
  return render(request,'pages/index.html',content)# 之後會多database，但煩，難改，呢個方式方便加model
```

Go to pages > index.html
add a for-loop

```python
{% if listings %}
<!-- Listing 1-->
{% for listing in listings %}
...others
{% endfor %}  {% else %}
<div class="col-md-6 col-lg-4 mb-4">
<h2>No Client Available</h2>
</div>
{% endif %}
<!-- Listing 2-->
```

```python
from src="assets/img/clinics/clinic-1.jpg"
change to src="{{ listing.photo_main.url}}"
```

---

Django humanize :

```bash
Please read the docu to know the keywords of humanize
https://docs.djangoproject.com/en/6.0/ref/contrib/humanize/
```

Go to Settings.py
Add ' 'django.contrib.humanize',' in DJANGO_APPSDJANGO_APPS

Add

```python
<!-- load humanize-->
{% load humanize %}
```

At the top of templates > pages > index.html

---

改 listing.html

```python
<a
  href="{% url 'listing' listing.id%}"
  class="btn btn-primary btn-block"
>
  More Info
</a>
```

```
brew install tcl-tk
```

Listing Item (Dynamic load database):

```html
    <section id="listings" class="py-4">
      <div class="container">
        <div class="row">
          <!-- Listing 1 -->
          {% if listings %}
          <!-- for loop listings -->
          {% for listing in listings %}
          <div class="col-md-6 col-lg-4 mb-4">
            <div class="card listing-preview">
              {% comment %} fix {% endcomment %}
              <img
                class="card-img-top"
                src="{{ listing.photo_main.url}}"
                alt="{{listing.title}}"
              />
              <div class="card-img-overlay">
              </div>
              <div class="card-body">
                <div class="listing-heading text-center">
                  <h4 class="text-primary">{{listing.title}}</h4>
                  <p>
                    <i class="fas fa-map-marker text-secondary"></i> {{listing.district}}
                  </p>
                </div>
                <hr />
                <div class="row py-2 text-secondary">
                  <div class="col-6">
                    <i class="fas fa-th-large"></i> Fee : {{listing.service | apnumber | capfirst}}
                  </div>
                  <div class="col-6">
                    <i class="fas fa-dumbbell"></i> Treatment
                  </div>
                </div>
                <div class="row py-2 text-secondary">
                  <div class="col-6">
                    <i class="fas fa-bed"></i> {{listing.rooms}}
                  </div>
                  <div class="col-6">
                    <i class="fas fa-bath"></i> Bathrooms
                  </div>
                </div>
                <hr />
                <div class="row py-2 text-secondary">
                  <div class="col-12"><i class="fas fa-user"></i> {{listing.doctor}}</div>
                </div>
                <div class="row text-secondary pb-2">
                  <div class="col-12">
                    <i class="fas fa-clock"></i> {{listing.list_date | timesince}}
                  </div>
                </div>
                <hr />
                <a href={% url 'listings:listing' listing.id %} class="btn btn-primary btn-block"
                  >More Info</a
                >
              </div>
            </div>
          </div>
          {% endfor %}
          <!-- end of for-loop -->
          {% else %}
          <div>
            <p>No listings available at this moment.Please check back later.</p>
          </div>
          {% endif %}
        </div>
```

---

30/12/2025
Move to listings > view.py
add

```python
from django.core.paginator import Paginator

def listings(request):
  # print(request.path) # /listings/
  listings = Listing.objects.filter(is_published=True)
  paginator = Paginator(listings,3) # add this line at 30/12/2025 , 限制出3個data
  page = request.GET.get('page')
  paged_listings = paginator.get_page(page)
  content = {'listings':paged_listings}
#  content = {'listings':listings} from listings change to paged_listings
  return render(request,'listings/listings.html',content)

```

Move to listings >
find

```html
<ul class="pagination"></ul>
```

Add

```html
{% if listings.has_other_pages %}
```

before

```
<ul class="pagination">
```

and

```html
{% endif %}
```

before first <div>
目的：要唔要做 pagination
Result : show 3 record only

依家要做既野：
'<<' 1 2 -> at page1 , need to disable '<<' button
at page2 -> need to active '<<' button

頭尾：

```html
{% if listings.has_other_pages %}
<ul class="pagination">
  ...
</ul>
{% endif %}
```

Left Button:

```html
{% if listings.has_previous %}
<li class="page-item">
  <a class="page-link" href="?page={{ listings.previous_page_number }}"
    >&laquo;</a
  >
  <!-- &laquo; = left , since &l-->
</li>
{% else %}
<li class="page-item disabled">
  <a class="page-link">&laquo;</a>
  <!-- &laquo; = left , since &l-->
</li>
{% endif%}
```

Right Button:

```html
{% if listings.has_next %}
<li class="page-item">
  <a class="page-link" href="?page={{ listings.next_page_number }}">&raquo;</a>
</li>
{% else %}
<li class="page-item disabled">
  <a class="page-link">&raquo;</a>
</li>
{% endif%}
```

Looping Page Number:

```html
{% for i in listings.paginator.page_range %} {% if listings.number == i%}
<li class="page-item active">
  <a class="page-link">{{i}}</a>
</li>
{% else %}
<li class="page-item">
  <a class="page-link" href="?page={{i}}">{{i}}</a>
</li>
{% endif%} {% endfor%}
```

---

---

改 admin page color:
templates > 加 folder > new folder named : admin > open new file named : base_site.html(MUST FOLLOW)

```html
{% extends "admin/base.html" %}
<!---->
{% load static %}
<!---->
{% block branding %}
<!---->
<img
  src="{% static 'img/logo.png'%}"
  alt="Clinic Logo"
  class="brand_img"
  height="35"
  width="45"
/>
<span class="brands">Clinic Create Admin</span>
<!---->
{% endblock branding %}
<!---->
{% block extra_style %}
<link rel="stylesheet" href="{% static css/admin.css%}" />
<!---->
{% endblock extra_style %}
<!---->
```

config > css > 加 admin.css

```css
#header {
  height: 50px;
  background: #ff9914;
  color: #ffffcc
}
#branding{
  color: #ededed
}
a:link,a:visited {
  color: #ededed
}
div.breadcrumbs {
  background: #2a81fd
  color: #ededed
}
div.breadcrumbs a {
  color: #29ab88
}
.module h2,
.module caption,
.inlinegroup h2{
  background: #ffffcc
}
.button,
input[type="submit"],
input[type="button"],
.submit-row input,
a.button {
  background : #2d4363,
  color: #333
}

.brands {
  margin-left: 15px;
}
```

改完要

```bash
python manage.py collectstatic
```

多一次，轉親 color 都要 collectstatic

---

listings > views.py

```python
from django.shortcuts import render,get_object_or_404
from listings.models import Listing
from django.core.paginator import Paginator
# Create your views here.
# 3.最終分黎呢度，2個endpoint
def listings(request):
  # print(request.path) # /listings/
  listings = Listing.objects.filter(is_published=True)
  paginator = Paginator(listings,3)
  page = request.GET.get('page')
  paged_listings = paginator.get_page(page)
  content = {'listings':paged_listings}
  return render(request,'listings/listings.html',content)
# ('pages/index.html')

def listing(request,listing_id):
  # return HttpResponse('<h1>about</h1>')
  listing = get_object_or_404(Listing,pk=listing_id)
  content = {'listing':listing}
  return render(request,'listings/listing.html',content)

def search(request):
  return render(request,'listings/search.html')
```

---

listing.html

```html
{% extends 'base.html' %}
<!-- load base-->
{% load humanize %}
<!-- load humanize-->
{% load static %}
<!-- block content-->
{% block content %}

<section id="showcase-inner" class="py-5 text-white">
  <div class="container">
    <div class="row text-center">
      <div class="col-md-12">
        <h1 class="display-4">{{listing.title}}</h1>
        <p class="lead">
          <i class="fas fa-map-marker"></i> {{listing.district}}
        </p>
      </div>
    </div>
  </div>
</section>

<!-- Breadcrumb -->
<section id="bc" class="mt-3">
  <div class="container">
    <nav>
      <ol class="breadcrumb">
        <li class="breadcrumb-item">
          <a href="{% url 'pages:index' %}">Home</a>
        </li>
        <li class="breadcrumb-item">
          <a href="{% url 'listings:index' %}">Listings</a>
        </li>
        <li class="breadcrumb-item active">{{listing.title}}</li>
      </ol>
    </nav>
  </div>
</section>

<!-- Listing -->
<section id="listing" class="py-4">
  <div class="container">
    <a href="{% url 'listings:index' %}" class="btn btn-light mb-4"
      >Back To Listings</a
    >
    <div class="row">
      <div class="col-md-9">
        <!-- Home Main Image -->
        <img
          src="{{ listing.photo_main.url}}"
          alt=""
          class="img-main img-fluid mb-3"
        />
        <!-- Thumbnails -->
        <div class="row mb-5 thumbs">
          {% if listing.photo_1 %}
          <div class="col-md-2">
            <a href="{{ listing.photo_1.url }}" data-lightbox="home-images">
              <img src="{{ listing.photo_1.url }}" alt="" class="img-fluid" />
            </a>
          </div>
          {% endif %} {% if listing.photo_2 %}
          <div class="col-md-2">
            <a href="{{ listing.photo_2.url }}" data-lightbox="home-images">
              <img src="{{listing.photo_2.url}}" alt="" class="img-fluid" />
            </a>
          </div>
          {% endif %} {% if listing.photo_3 %}
          <div class="col-md-2">
            <a href="{{ listing.photo_3.url }}" data-lightbox="home-images">
              <img src="{{listing.photo_3.url}}" alt="" class="img-fluid" />
            </a>
          </div>
          {% endif %} {% if listing.photo_4 %}
          <div class="col-md-2">
            <a href="{{ listing.photo_4.url }}" data-lightbox="home-images">
              <img src="{{listing.photo_4.url}}" alt="" class="img-fluid" />
            </a>
          </div>
          {% endif %}
        </div>

        <!-- Fields -->
        <div class="row mb-5 fields text-center">
          <div class="col-md-12">
            <ul class="list-group list-group-flush">
              <li class="list-group-item text-secondary">
                <i class="fas fa-money-bill-alt"></i> Inpatient Fee Estimate:
                <span>$ {{listing.service | intcomma | capfirst}} /Day</span>
              </li>
              <li class="list-group-item text-secondary">
                <i class="fas fa-bed"></i> Rooms:
                <span> {{listing.rooms}} </span>
              </li>
            </ul>
          </div>
        </div>

        <!-- Description -->
        <div class="row mb-5 text-center">
          <div class="col-md-12">
            <h5 class="text-primary">Description</h5>
            <p>{{ listing.description }}</p>
          </div>
        </div>
      </div>
      <div class="col-md-3">
        <div class="card mb-3">
          <img
            class="card-img-top"
            src="{{listing.doctor.photo.url}}"
            alt="Seller of the month"
          />
          <div class="card-body">
            <h5 class="card-title">Medical Practitioner</h5>
            <h6 class="text-secondary">{{listing.doctor.name}}</h6>
          </div>
        </div>
        <button
          class="btn-primary btn-block btn-lg"
          data-toggle="modal"
          data-target="#inquiryModal"
        >
          Make An Inquiry
        </button>
      </div>
    </div>
  </div>
</section>

<!-- Inquiry Modal -->
<div class="modal fade" id="inquiryModal" role="dialog">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="inquiryModalLabel">Make An Inquiry</h5>
        <button type="button" class="close" data-dismiss="modal">
          <span>&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <form>
          <div class="form-group">
            <label for="property_name" class="col-form-label">Property:</label>
            <input
              type="text"
              name="listing"
              class="form-control"
              value="{{listing.title}}"
              disabled
            />
          </div>
          <div class="form-group">
            <label for="name" class="col-form-label">Name:</label>
            <input type="text" name="name" class="form-control" required />
          </div>
          <div class="form-group">
            <label for="email" class="col-form-label">Email:</label>
            <input type="email" name="email" class="form-control" required />
          </div>
          <div class="form-group">
            <label for="phone" class="col-form-label">Phone:</label>
            <input type="text" name="phone" class="form-control" />
          </div>
          <div class="form-group">
            <label for="message" class="col-form-label">Message:</label>
            <textarea name="message" class="form-control"></textarea>
          </div>
          <hr />
          <input
            type="submit"
            value="Send"
            class="btn btn-block btn-secondary"
          />
        </form>
      </div>
    </div>
  </div>
</div>
{% endblock %}
```

---

under templates > open 404.html

```html
{% extends "base.html"%} {% block content %}
<div class="container">
  <h1 class="mx-auto" style="height: 50vh; width: 500px">
    Clinic Page Not Find
  </h1>
  <p>The page you are looking for does not exist.</p>
</div>
{% endblock %}
```

---

lightbox js(https://lokeshdhakar.com/projects/lightbox2/)
folder static > css > lightbox.min.css (already has)

move to base.html

```js
<!-- lightbox -->
<link rel="stylesheet" href={% static "css/lightbox.min.css"%} /> and
<script src="{% static 'js/lightbox.min.js' %}"></script>
```

Javascript -> 係 base.html 加
