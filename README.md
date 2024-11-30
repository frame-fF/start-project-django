# start-project-django
step start project django

### virtualenv
```python
virtualenv venv
or
python -m virtualenv venv
```
### Activate
```python
venv\Scripts\activate        (window)
source venv/bin/activate     (linux)
```
### install Django
```python
pip install django
```
### Create Project
```python
create folder project
```
```python
django-admin startproject config .
```
### Create App
```python
python manage.py startapp mywebsite
```
### Config settings.py
```python
INSTALLED_APPS = [
	'mywebsite',
]
```
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [Path.joinpath(BASE_DIR, 'templates')], 
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```
```python
from django.utils.translation import gettext_lazy as _
```
```python
# django-admin makemessages --all --ignore=venv
# django-admin compilemessages
```
```python
LANGUAGES = (
    ('en', _('English')),
    ('th', _('Thai')),
)

LOCALE_PATHS = [
    BASE_DIR / 'locale/',
]
```
```python
LANGUAGE_CODE = 'th'

TIME_ZONE = 'Asia/Bangkok'

USE_I18N = True

USE_L10N = True

USE_TZ = True
```
```python
SESSION_ENGINE = 'django.contrib.sessions.backends.signed_cookies'

SESSION_CACHE_ALIAS = "default"

SESSION_EXPIRE_AT_BROWSER_CLOSE = True
```
```python
STATIC_URL = '/static/'

STATICFILES_DIRS = [Path.joinpath(BASE_DIR, 'static')]
STATIC_ROOT = Path.joinpath(BASE_DIR, 'staticfiles')

MEDIA_URL = '/media/'
MEDIA_ROOT = Path.joinpath(BASE_DIR, 'media')
```
### Config urls.py
```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static
from django.contrib.staticfiles.urls import staticfiles_urlpatterns

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('mywebsite.urls')),
]

urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
urlpatterns += staticfiles_urlpatterns()
```
### Create urls.py in App
```python
from django.urls import path
from mywebsite import views

app_name = 'mywebsite'

urlpatterns = [
    path('', views.index, name='index'),
]
```
### Create code in view.py
```python
def index(request):
    return render(request, 'index.html')
```
### Create Folder
```python
static 
templates
```
### django whitenoise
```python
pip install whitenoise
```
```python
MIDDLEWARE = [
    'whitenoise.middleware.WhiteNoiseMiddleware',
]
```
```python
STORAGES = {
    "staticfiles": {
        "BACKEND": "whitenoise.storage.CompressedManifestStaticFilesStorage",
    },
}
```
### django django-cleanup
```python
pip install django-cleanup
```
```python
INSTALLED_APPS = (
    'django_cleanup.apps.CleanupConfig',
)
```
### django django-storages
```python
pip install django-storages boto3
```
```python
DEFAULT_FILE_STORAGE = "storages.backends.s3boto3.S3Boto3Storage"
```
```python
AWS_S3_REGION_NAME = env("AWS_S3_REGION_NAME")

AWS_S3_ENDPOINT_URL = f"https://{AWS_S3_REGION_NAME}.vultrobjects.com"

AWS_S3_USE_SSL = True

AWS_STORAGE_BUCKET_NAME = env("AWS_STORAGE_BUCKET_NAME")

AWS_ACCESS_KEY_ID = env("AWS_ACCESS_KEY_ID")

AWS_SECRET_ACCESS_KEY = env("AWS_SECRET_ACCESS_KEY")

AWS_DEFAULT_ACL="public-read"
```
### django env
```python
pip install django-environ
```
### Create .env
```python
DEBUG=off

SECRET_KEY=xxxxxxxxx

ALLOWED_HOSTS='127.0.0.1'

CSRF_TRUSTED_ORIGINS='http://127.0.0.1:8000/'
```
```python
import environ
import os

env = environ.Env(
    DEBUG=(bool, False)
)

environ.Env.read_env(os.path.join(BASE_DIR, '.env'))

SECRET_KEY = env('SECRET_KEY')
DEBUG = env('DEBUG')
ALLOWED_HOSTS = env('ALLOWED_HOSTS').split(',')
CSRF_TRUSTED_ORIGINS = env('CSRF_TRUSTED_ORIGINS').split(',')
```
### Create NPM
```python
 npm
```
```python
npm install -D tailwindcss
npm install -D flowbite
npm install -D flowbite-typography
npm install -D clean-css-cli
```
```python
npx tailwindcss init
```
### Set scripts package.json
```python
{
  "scripts": {
    "build-test": "tailwindcss -i ../static/flowbite/input.css -o ../static/flowbite/output.min.css --watch",
    "build-product": "tailwindcss -o ../static/flowbite/output.min.css --minify && cleancss -o ../static/flowbite/output.min.css ../static/flowbite/output.min.css"
  },
  "devDependencies": {
    "@tailwindcss/typography": "^0.5.10",
    "clean-css-cli": "^5.6.2",
    "flowbite": "^1.8.1",
    "flowbite-typography": "^1.0.3",
    "tailwindcss": "^3.3.3"
  }
}
```
### Set scripts package.json
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {

  darkMode: 'class',

  future: {
    removeDeprecatedGapUtilities: true,
    purgeLayersByDefault: true,
  },

  content: [
    "../templates/**/*.{html,js,css}",
    "../templates/*.{html,js,css}",
    "./templates/**/*.{html,js,css}",
    "./templates/*.{html,js,css}",
    "../apps/**/*.{html,js,css}",
    "../apps/*.{html,js,css}",  
    "./apps/**/*.{html,js,css}",
    "./apps/*.{html,js,css}",
    "./node_modules/**/*.{html,js,css}",
    "../node_modules/**/*.{html,js,css}",  
    "../static/**/*.{html,js,css}",
    "../static/*.{html,js,css}",
    "./static/**/*.{html,js,css}",
    "./static/*.{html,js,css}",
    // "./node_modules/flowbite/**/*.{html,js,css}",
    // "../node_modules/flowbite/**/*.{html,js,css}",
    // "../flowbite/**/*.{html,js,css}",
    // "../flowbite/*.{html,js,css}",  
    // "./flowbite/**/*.{html,js,css}",
    // "./flowbite/*.{html,js,css}",  
  ],
  theme: {
    extend: {
      colors: {
        'custom-green': '#06C755',
        'custom-black': '#000000',
        primary: {"50":"#eff6ff","100":"#dbeafe","200":"#bfdbfe","300":"#93c5fd","400":"#60a5fa","500":"#3b82f6","600":"#2563eb","700":"#1d4ed8","800":"#1e40af","900":"#1e3a8a","950":"#172554"}
      },
    },
  },
  plugins: [
    require('flowbite/plugin'),
    require('flowbite-typography'),
  ],
}
```
### Create static/npm_static/input.css 
```python
@tailwind base;
@tailwind components;
@tailwind utilities;
```
### run
```python
npm run build-test
```
### Create Docker
```python
pip install uvicorn
```
```python
pip freeze > requirements.txt
```
### Create Dockerfile
```python
FROM ubuntu:22.04

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
ENV DJANGO_SETTINGS_MODULE=projectmanager.settings

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get -y install curl \
    && curl -s https://deb.nodesource.com/setup_18.x | bash \
    && apt-get install -y --no-install-recommends \
    build-essential \
    python3-dev \
    libpq-dev \
    nodejs \
    python3-pip \ 
    && python3 -m pip install --upgrade pip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt /app/requirements.txt
COPY ./projectmanager/. /app/

WORKDIR /app/npm
RUN rm -rf node_modules package-lock.json \
    && npm install \
    && npm run build

WORKDIR /app
RUN pip install --no-cache-dir -r requirements.txt \
    && python3 manage.py collectstatic --noinput

EXPOSE 8080
```
### Create docker-compose.yml
```python
version: '3'

services:

  web:
    container_name: frame-dev-web
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 8080:8080
    working_dir: /app
    command: "sh -c 'python3 manage.py migrate \
              && uvicorn projectmanager.asgi:application --host 0.0.0.0 --port 8080 --lifespan=off'"
    restart: always
networks:
  default:
    external: true
    name: frame_dev
```
### คำสั่งอื่นๆ
`python manage.py makemigrations`

`python manage.py migrate`

`python manage.py migrate --fake`

`python manage.py createsuperuser`

`python manage.py runserver`

`python manage.py collectstatic`

`python manage.py collectstatic --noinput --clear`

`pip freeze > requirements.txt`

`pip install -r requirements.txt`

`python -m pip install -U --force pip`

### Database
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
### Mysql
```python
pip install mysqlclient
```

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'HOST': '',
        'USER': '',
        'PASSWORD': '',
        'PORT': '3306',
        'NAME': ''
    }
}
```



### PowerShell with the "Run as Administrator" 

```python
set-executionpolicy remotesigned
or
Set-ExecutionPolicy unrestricted
```

### dumpdata 

```python
python manage.py dumpdata > data.json
or
python manage.py dumpdata blog > data.json
fixerror
python -Xutf8 ./manage.py dumpdata > data.json
```
```python
หากเกิดปัญหาให้ save เป็น utf-8
python manage.py loaddata data.json
```
