# Python 이민 수난기
## dJango를 써보즈ㅏㅏㅏㅏ

1. 가상환경 생성하기
```
$ python -m venv [venv_name]
```

2. 가상환경 기본명령어
```
# 실행하기
$ source [venv_name]/bin/activate
# 혹은
$ . [venv_name]/bin/activate

# 종료하기
$ deactivate [venv_name]
```

3. Django 설치하기
```
# pip 업그레이드
$ python -m pip install --upgrade pip

# Django 설치하기
$ pip install django

# Django 버전 설정하여 설치하기
$ pip install django~=[version]
```

4. Django 프로젝트 생성하기
```
$ django-admin startproject [Project_name] [Directory_path]
```

5. settings.py 변경하기
```
# Project 폴더 내에서 settings.py 를 찾아서 변경하세요

# 시간대 변경
TIME_ZONE = 'Asia/Seoul'

# 정적파일 경로
STATUC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR,'static')

# 호스트 정보, Public IP 입력, * 도 가능
ALLOWED_HOSTS = ['[Public IP]']

# DB 설정
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3', # DB_ENGINE, MySQL
        'NAME': '[DB_NAME]',
        'USER': '[DB_USER]',
        'PASSWORD': '[DB_PASSWORD]',
        'HOST': '[IP or Endpoint]',
        'PORT': '[port]'
    }
}
```

6. DB 적용
```
# models.py 변경시 실행, Migration 파일 생성
$ python manage.py makemigrations

# Migration 파일을 DB에 반영
$ python manage.py migrate
```

7. 서버 실행해보기
```
$ python manage.py runserver

# 혹은
# 로컬용
$ python manage.py runserver 0:[PORT]
```

8. App 만들기
```
$ python manage.py startapp [App_name]
```

9. Django에 App 추가하기
```
# settings.py 내부 수정
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    [App_name] # 추가
]
```

10. Models.py에 모델 추가하기
```
from django.utils import timezone

# Post 모델 정의
class Post(models.Model):
    author = models.ForeignKey('auth.User',on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
        default = timezone.now
    )
    published_date = models.DateTimeField(
        blank=True, null=True
    )

    objects = models.Manager()

    def publish(self):
        self.published_date = timezone.now()
        self.save()
    
    def __str__(self):
        return self.title
```

11. DB에 모델을 위한 테이블 만들기
```
$ python manage.py makemigrations [App_name]

$ python manage.py migrate [App_name]
```

12. admin.py 세팅
```
# admin.py
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

13. 접속해보기
```
$ python manage.py runserver

# 다음 url로 접속
http://localhost:[PORT]/admin
```

14. Superuser 만들기
```
$ python manage.py createsuperuser
```

15. Django URL 세팅
```
# [Project]/urls.py
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path(r'^admin/', admin.site.urls),

    # admin 제외한 모든 URL을 App으로 포워딩
    path(r'',include('[App_name].urls')),
]
```

16. App에 URL 세팅
```
# [App_name]/urls.py 생성
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$',views.post_list, name='post_list'),
]
```

17. views 세팅
```
# views.py
from django.shortcuts import render

# Create your views here.

def post_list(request):
    return render(request,'blog/post_list.html',{})
```

18. html파일 생성
```
# [App_name]/templates/blog 폴더 생성
# blog 폴더 내에 post_list.html파일 생성
# post_list.html
<html>
    <p>Hello World!<p>
<html>
```

19. view에 동적데이터 넘겨주기
```
#views.py
from django.shortcuts import render
from django.utils import timezone
from .models import Post

# Create your views here.

def post_list(request):    
    posts = Post.objects.filter(published_date_lte=timezone.now()).order_by('published_date')
    return render(request,'blog/post_list.html',{'posts':posts})
```

20. post_list.html에 데이터 표시하기
```
<html>
<body>
    <div>
        <h1><a href = "https://aws.amazon.com">AWS</a></h1>
    </div>
    {% for post in posts %}
        <div>
            <p>published {{post.published_date}}</p>
            <h1><a href="">{{post.title}}</a></h1>
            <p>{{post.text|linebreaksbr}}</p>
        </div>
    {% endfor %}
</body>
<html>
```