# Python flask app dockerization

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## Description

Main purpose of this lab is dockerization of python web application that use flask framework to serve HTTP content.

## Table of Contents

- [Requirements](#requirements)
- [Usage](#usage)
- [Configuration](#configuration)
  - Image creation
  - Container deployment 
- [Launch](#launch)
- [License](#license)

## Requirements

 - python package installed

Windows Powershell : <br>
```
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

Invoke-WebRequest -Uri "https://www.python.org/ftp/python/3.7.0/python-3.7.0.exe" -OutFile "c:/temp/python-3.7.0.exe"

c:/temp/python-3.7.0.exe /quiet InstallAllUsers=0 InstallLauncherAllUsers=0 PrependPath=1 Include_test=0
```
Linux bash : <br>
```
sudo dnf install python3 | sudo apt get install python3
```

 - requirements.txt

 file placed into app directory and contains all libraries that need to be included to properly execute code<br>Actually file contains only one line<br>
 `flask`

## Configuration

  ### 1. Image creation

We construct docker image to pack all dependencies and whole application files into one program that can be deployed fast and easly<br>
On listing below we see pure Dockerfile, that must be placed into application directory

```
FROM python:3.9
WORKDIR \flask-web-hosting
ADD app.py .
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
COPY . .
CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0"]
```

First line defines python version that will be included into image we create<br>
WORKDIR keyword select working directory folder - all application and middleware content<br>
ADD app.py . - define main python file<br>
requirements.txt contains dependencies/libraries crucial for application work<br>
last but not least CMD keyword executes command - in this case starts flask server from localhost<br>
`docker build --tag python-my-flask-webapp .`
Above command built for us image 

  ### 2. Container deployment

  Now lets choose which image to run. By run command, we can run an image by passing the image's name as a parameter.<br>
  While running the above command, you'll notice that on the command line it indicates that the application is running.<br>
  But when you enter http://localhost:5000/ on the browser, the greeting will be:<br>
  `This site can’t be reached localhost refused to connect.`<br><br>
  Regardless of whether the container is running, it is doing so in isolation mode and cannot connect to localhost:5000.<br>
The best solution is to run the image in detached mode. Because we need to view this application in the browser rather than the container, we'll modify our docker run and add two additional tags: "-d" to run it in detached mode and "-p" to specify the port to be exposed.<br>
The docker run command will now be formatted as follows:<br>
`docker run -d -p 5000:5000 python-my-flask-webapp`

## Launch

Container should appear in container list<br>
`docker ps`<br>
![image](https://github.com/damian-andrzej/docker_flask_webapp/assets/102800704/ab5a2157-1c2f-489a-922a-9bb38016ba25)

Lets check container in action - type in your searchbar http://localhost:5000/ <br>

![image](https://github.com/damian-andrzej/docker_flask_webapp/assets/102800704/54f87824-cb44-4392-9085-1dd4990ba0a5)



  
