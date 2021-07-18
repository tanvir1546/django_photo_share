# django_photo_share

Install Apache2 and dependencies in Raspberry Pi:
```
sudo apt-get update && sudo apt-get upgrade -y 
sudo apt-get install apache2 -y
sudo apt install libapache2-mod-wsgi-py3
sudo apt install python3 python3-venv python3-pip
```
Configure Apache2 for django:
```
sudo nano /etc/apache2/sites-enabled/000-default.conf
```

Add following line at the end before </VirtualHost>
```	
    Alias /static /home/pi/web/static
    <Directory /home/pi/web/static>
    Require all granted
    </Directory>

    <Directory /home/pi/web/web>
    	<Files wsgi.py>
    Require all granted
     </Files>
    </Directory>

    WSGIDaemonProcess django python-path=/home/pi/web python-home=/home/pi/web-env
    WSGIProcessGroup django
    WSGIScriptAlias / /home/pi/web/web/wsgi.py
```
Restart apache server:
```
sudo systemctl restart apache2
```
Create and activate VENV
```
mkdir -p /home/pi/web/static
python3 -m venv web-env
source web-env/bin/activate
```
install Django
```
python3 -m pip install django
django-admin startproject pidjango .
```
Add the ip :
```
nano /home/pi/web/web/settings.py
ALLOWED_HOSTS = ["192.168.0.104","localhost",]
```
Changes in web/settings.py
```
INSTALLED_APPS = [
    ...
#third party app
    'crispy_forms',
    'taggit',
#custom app
    'photoapp',
    'users',
]

TAGGIT_CASE_INSENSITIVE= True
CRISPY_TEMPLATE_PACK='bootstrap4'
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        ...            ],
        },
    },
]
#Change the time zone
TIME_ZONE = 'Asia/Tokyo'   


MEDIA_URL ='/media/'
MEDIA_ROOT= BASE_DIR / 'media/'
USE_TZ = True

# Django Authentication
LOGIN_URL = 'user:login'
LOGIN_REDIRECT_URL = 'photo:list'

LOGOUT_REDIRECT_URL = 'photo:list'

```
**TO be continued**

