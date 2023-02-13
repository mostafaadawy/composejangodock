# create docker multiple images
- before diving inside this project you can begin from this docker start [proj](https://github.com/mostafaadawy/try_docker)
- using docker-compose
- first create empty folder for the project 
- create `dockerfile` check the code
```sh
FROM python:3
ENV PYTHONUNBUFFERD=1
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/
```
- as previous repo from used to select image for python 
- ENV for environment that tells the environments requirements that python will not be buffered 
- while work directory is defined then copying the requirements.txt in order to be installed or executed
- run line code defines that the execution tool will be pip install
- then copy the code in the current folder to the docker
- in requirements.txt `Django>3.0,<4.0` we define the required version of Django that we need to install as dependency 
- now lets create `docker-compose.yml` file that file that defines the services that our docker/app will use
- in this example we have two services web server and database
- so this file describes the services to use and how to linking together
- check the code
```sh
version: "3.9"

services:
    db:
        image: postgres
        environment:
            - POSTGRES_DB=postgres
            - POSTGRES_USER=postgres
            - POSTGRES_PASSWORD=postgres
    web:
        build: .
        command: python manage.py runserver 0.0.0.0:8000
        volumes:
            - .:/code
        ports:
            - "8000:8000"
        depends_on:
            - db
```
- for the code we can see tha first we have to describe the cversion of the docker compose
- then we describe the service tabs is important and here there are two services db that uses ready image and web that will be build in same place `.` 
- command in web service defines that when we call that service we run the server 
- also we define the volume of the server and the ports mapping left side is the docker while right is the os
- depends_on join the two service where it depends on db
- the environment in db describes the connection so later we need to define it
- and in our web django which by default depends on sqlite needs to configured by the path to postgres db service
# create project
in the original django project creation we create the virtualenv then create inside it after activating it the dependency which will be django then creates django project using django command `django-admin startproject dockeddjango .`
where . means in the current place and `dockeddjango` is the project name that we can use any name
- in our case we use the same command but prefixed by `docker compose run web` 
- to become `docker compose run web django-admin startproject dockeddjango .`
- this will create our project first file that will be executed is docker file that will install the dependency then build the services from compose
- in django we need to edit the database to link it with db service postgres in settings.py
- change this to 
```sh
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
- to 
```sh
DATABASES = {
     'default':{
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
     }
}
```
- to build our application in single service we used `docker build -t nameof docker` that creates image then this image will use the docker file description then we can run our docker container app by `docker run dockerimagenae` but here we build using `docker compose run web django-admin startproject dockeddjango .`
- and we run it using `docker-compose up`

