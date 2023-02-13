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
