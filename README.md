# start-project-django
step start project django

### virtualenv
```python
py -3.12 -m pip install virtualenv
py -3.12 -m virtualenv .venv    
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
# fix window
# https://mlocati.github.io/articles/gettext-iconv-windows.html
# add path
```
```python
create folder locale
-en
-th
```
```python
MIDDLEWARE = [
    'django.middleware.locale.LocaleMiddleware',
]
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
from django.conf.urls.i18n import i18n_patterns
from django.views.i18n import set_language

urlpatterns = i18n_patterns(
    path('admin/', admin.site.urls),
    path('i18n/', set_language, name='set_language'),
    path('', include('apps.home.urls')),
)

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
STORAGES = {
    "staticfiles": {
        "BACKEND": "whitenoise.storage.CompressedManifestStaticFilesStorage",
    },
    "default": {
        "BACKEND": "storages.backends.s3boto3.S3Boto3Storage",
    },
}
```
```python
AWS_S3_REGION_NAME = env("AWS_S3_REGION_NAME")

AWS_S3_ENDPOINT_URL = env("AWS_S3_ENDPOINT_URL")

AWS_S3_USE_SSL = True

AWS_STORAGE_BUCKET_NAME = env("AWS_STORAGE_BUCKET_NAME")

AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}

AWS_LOCATION = env("AWS_LOCATION")

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
DEBUG=on

SECRET_KEY=xxxxxx

DOMAIN_NAME=127.0.0.1
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
ALLOWED_HOSTS = [env('DOMAIN_NAME')]
CSRF_TRUSTED_ORIGINS = [f"https://{env('DOMAIN_NAME')}", f"http://{env('DOMAIN_NAME')}"]
```
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [Path.joinpath(BASE_DIR, 'templates')],
        'APP_DIRS': env('DEBUG'),
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

if not env('DEBUG'):
    TEMPLATES[0]['OPTIONS']['loaders'] = [
        ('django.template.loaders.cached.Loader', [
            'django.template.loaders.filesystem.Loader',
            'django.template.loaders.app_directories.Loader',
        ]),
    ]
```

### Create RedisCache
```python
pip install django-redis
```
```python
SESSION_ENGINE = "django.contrib.sessions.backends.cache"
```
```python
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": f"redis://{env('REDIS_HOST')}/{env('REDIS_PORT')}",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
            "COMPRESSOR": "django_redis.compressors.zlib.ZlibCompressor",
        }
    }
}
```
### LOGGING
```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'ERROR',
            'class': 'logging.FileHandler',
            'filename': os.path.join(BASE_DIR, 'error.log'),
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file'],
            'level': 'ERROR',
            'propagate': True,
        },
    },
}
```
### Create NPM
```python
 flowbite
```
```python
npm install tailwindcss @tailwindcss/cli
npm install flowbite
npm install flowbite-typography
npm install clean-css-cli
```
```python
npx tailwindcss init
```
### Set scripts package.json
```python
{
  "scripts": {
    "build-dev": "npx @tailwindcss/cli -i ./src/input.css -o ../static/flowbite/output.min.css --watch",
    "build-prod": "npx @tailwindcss/cli -i ./src/input.css -o ../static/flowbite/output.min.css --minify && cleancss -o ../static/flowbite/output.min.css ../static/flowbite/output.min.css"
  },
  "devDependencies": {
    "@tailwindcss/cli": "^4.0.1",
    "clean-css-cli": "^5.6.3",
    "flowbite": "^3.0.0",
    "flowbite-typography": "^1.0.5",
    "tailwindcss": "^4.0.1"
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
### Create /flowbite/src/input.css 
```python
@import "tailwindcss";
@config "../tailwind.config.js";
```
### run
```python
npm run build-test
```
### Security settings
```python
if not env('DEBUG'):
    SECURE_HSTS_SECONDS = 31536000
    SECURE_HSTS_INCLUDE_SUBDOMAINS = True
    SECURE_SSL_REDIRECT = False
    SECURE_HSTS_PRELOAD = True
    SESSION_COOKIE_SECURE = True
    CSRF_COOKIE_SECURE = True
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
`python manage.py check --deploy`

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

`Get-ChildItem -Path . -Recurse -Filter "*.py" | Where-Object { $_.FullName -match "\\migrations\\" -and $_.Name -ne "__init__.py" } | Remove-Item`

`Get-ChildItem -Path . -Recurse -Filter "*.pyc" | Where-Object { $_.FullName -match "\\migrations\\" } | Remove-Item`

### Database
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
### PostgreSQL 
```python
pip install psycopg2
```

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'HOST': env('DB_HOST'),
        'USER': env('DB_USER'),
        'PASSWORD': env('DB_PASSWORD'),
        'PORT': env('DB_PORT'),
        'NAME': env('DB_NAME'),
        'CONN_MAX_AGE': 600, 
        'OPTIONS': {
            'options': '-c search_path=public,other_schema',
        },
    },
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
