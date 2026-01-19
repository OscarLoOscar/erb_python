Go to `http://127.0.0.1:8000/accounts/3rdparty/` to check **Account Connections** , is that has **You can sign in to your account using any of the following third-party accounts: ABC@gmail.com Google**

## Google login:

register.html , add :

```html
{% extends "base.html" %}
<!---->
{% load humanize %}
<!---->
{% load static %}
<!--Google login -->
{% load socialaccount %}
```

---

settings.py:

```python
INSTALLED_APPS = [
    ...
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'allauth.socialaccount.providers.google', # Google 提供商
    ...
]

AUTHENTICATION_BACKENDS = [
    'django.contrib.auth.backends.ModelBackend',
    'allauth.account.auth_backends.AuthenticationBackend',
]

SITE_ID = 1
LOGIN_REDIRECT_URL = '/' # 登入後去邊
SOCIALACCOUNT_LOGIN_ON_GET = True # 撳掣即跳轉，唔使再確認一次

SOCIALACCOUNT_ADAPTER = 'allauth.socialaccount.adapter.DefaultSocialAccountAdapter'
LOGGING = {
    'version': 1,
    'handlers': {
        'console': {'class': 'logging.StreamHandler'},
    },
    'loggers': {
        'allauth': {'handlers': ['console'], 'level': 'DEBUG'},
    },
}
```

```
喺 Admin 後台登記 Google Client ID
雖然你有 JSON，但 Django 需要喺資料庫紀錄呢組 ID：

執行 python manage.py migrate（建立 allauth 相關 Table）

入 Django Admin -> Social Applications -> Add Social Application

Provider: 選 Google

Name: 求其改（例如 "Google Login"）

Client id: 填入你 JSON 入面嗰串長 ID

Secret key: 填入你 JSON 入面嗰串密鑰

Chosen sites: 將右邊嘅 example.com（或 localhost）搬去左邊
```

---

---

## Django Google Login 完整集成指南

本指南整合了在使用 `django-allauth` 過程中遇到的常見錯誤、環境配置及解決方案

```bash
pip install django-allauth requests PyJWT
```

### 一、 環境安裝 (解決 `ModuleNotFoundError` & `AttributeError`)

#### 1. 錯誤發生

- 錯誤 A: `No module named 'requests'` —— 缺少網絡請求庫
- 錯誤 B: `module 'jwt' has no attribute 'PyJWTError'` —— 關鍵衝突！ 安裝了 jwt 套件而非 PyJWT

#### 2. 解決方案 (正確安裝順序)

必須先卸載衝突套件，再安裝正確版本：

```Bash
pip uninstall jwt pyjwt -y
```

### 二、 Settings.py 配置 (解決 `ImproperlyConfigured`)

#### 1. 關鍵 Middleware 修正

`django-allauth` 65.x+ 版本必須加入 `AccountMiddleware`

```Python
MIDDLEWARE = [ # ... 其他 django 中間件
'django.contrib.auth.middleware.AuthenticationMiddleware',

    # 必須加入這一行
    'allauth.account.middleware.AccountMiddleware',

    # ... 其他中間件

]

# Allauth 配置

AUTHENTICATION_BACKENDS = [
'django.contrib.auth.backends.ModelBackend',
'allauth.account.auth_backends.AuthenticationBackend',
'users.backends.EmailOrUsernameModelBackend', # 你的自定義 Backend
]

INSTALLED_APPS = [
# ...
'django.contrib.sites',
'allauth',
'allauth.account',
'allauth.socialaccount',
'allauth.socialaccount.providers.google',
]

SITE_ID = 1
SOCIALACCOUNT_LOGIN_ON_GET = True # 點擊按鈕直接跳轉
LOGIN_REDIRECT_URL = 'pages:index' # 登入後跳轉路徑
```

---

### 三、 資料庫同步 (解決 `DuplicateTable`)

#### 1. 錯誤發生

`psycopg2.errors.DuplicateTable: relation "orders_order" already exists` 原因：資料庫已有 Table，但 Migration 檔案重新生成過

#### 2. 解決方案 (Fake Migrate)

```Bash
# 同步現有自定義 App 狀態

python manage.py migrate --fake orders
python manage.py migrate --fake users

# ... 其他報錯的 App

# 執行 Allauth 必備的遷移

python manage.py migrate
```

### 四、 Template 修正 (解決 `TemplateSyntaxError`)

#### 1. 錯誤發生

`'block' tag with name 'content' appears more than once` 原因：同一個 HTML 檔案內寫了兩次 `{% block content %}`

#### 2. 正確的 `register.html` 結構

```HTML
{% extends "base.html" %}
{% load static %}
{% load humanize %}
{% load socialaccount %} {% block extra_css %}

<style>
  /* 你的自定義 Checkbox CSS */
  .checkbox-wrapper { position: relative; ... }
  .checkmark { ... }
</style>

{% endblock %}

{% block content %}
<a href="{% provider_login_url 'google' %}" class="btn btn-outline-light border mb-4">
<img src="{% static 'img/google_logo.png' %}" style="height:25px;">
</a>

  <div class="mb-4 mx-auto text-left" style="width: 90%">
    <label class="d-flex align-items-start" style="cursor: pointer">
      <div class="checkbox-wrapper mr-2">
        <input type="checkbox" name="agree_terms" required>
        <span class="checkmark"></span>
      </div>
      <span class="small text-muted">
        我同意網站 <a href="#">服務條款</a> 及 <a href="#">隱私權政策</a>，<br>且我已滿 18 歲
      </span>
    </label>
  </div>
{% endblock %}
```

### 五、 Google Cloud Console & Admin 設定

1. Google Console 設定
   Authorized redirect URIs: `http://127.0.0.1:8000/accounts/google/login/callback/` (注意：尾部必須有斜槓)

2. Django Admin 設定

- 進入 `Social Applications` -> `Add`

- Provider: Google

- Client id: 填入 JSON 中的 `client_id`

- Secret key: 填入 JSON 中的 `client_secret`

- Key: 留空

- Sites: 將 `127.0.0.1:8000` (或 `example.com`) 移至右側 Chosen sites

##### 檢查清單

是否已執行 `pip install pyjwt` 並卸載 `jwt？`

`MIDDLEWARE` 是否包含 `AccountMiddleware？`

Template 是否只有一個 `{% block content %}`？

Admin 裡的 Social Application 是否關聯了正確的 Site？
