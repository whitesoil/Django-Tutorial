# Python 이민 수난기
### dJango를 써보즈ㅏㅏㅏㅏ


1. 가상환경 생성하기
```
python -m venv [venv_name]
```
2. 가상환경 기본명령어
```
# 실행하기
source [venv_name]/bin/activate
# 혹은
. [venv_name]/bin/activate

# 종료하기
deactivate [venv_name]
```
3. Django 설치하기
```
# pip 업그레이드
python -m pip install --upgrade pip

# Django 설치하기
pip install django

# Django 버전 설정하여 설치하기
pip install django~=[version]
```
4. Django 프로젝트 생성하기
```
django-admin startproject [Project_name] [Directory_path]
```