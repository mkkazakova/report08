# report08

### Client

В папке client создаём 2 файла:

![image](https://user-images.githubusercontent.com/125077130/233801505-10bcb35f-b67c-49f3-b344-18f988f3a0e6.png)

Содержимое client.py:

```
import urllib.request

fp = urllib.request.urlopen("http://localhost:56665/")

encodedContent = fp.read()
decodedContent = encodedContent.decode("utf8")

print(decodedContent)

fp.close()
```

Содержимое Dockerfile:

```
FROM python:latest

ADD client.py /client/

WORKDIR /client/
```

### Server

В папке server создаём 3 файла:

![image](https://user-images.githubusercontent.com/125077130/233801679-a7b99dc6-ae77-4aa1-8487-cbac0e88f750.png)

Содержимое server.py:

```
import http.server
import socketserver

handler = http.server.SimpleHTTPRequestHandler
with socketserver.TCPServer(("", 56665), handler) as httpd:
   httpd.serve_forever()
```

Содержимое Dockerfile:

```
FROM python:latest

ADD server.py /server/
ADD index.html /server/

WORKDIR /server/
```

Содержимое index.html

```
No one knows as mush as I don't.
```


### Action

В папке /.github/workflows создаём файл Action.yml

Содержимое Action.yml:

```
name: Docker Compose

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Login to DockerHub
      uses: docker/login-action@v1
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    - name: Build Docker images
      run: docker-compose build

    - name: Deploy with Docker Compose
      run: docker-compose up -d
```


### docker-compose.yml

Содержимое docker-compose.yml:

```
version: "3"

services:

  server:

    build: server/
    command: python ./server.py
    ports:
      - 56665:56665

  client:

    build: client/
    command: python ./client.py
    network_mode: host
    depends_on:
      - server
```
### test

```
sudo snap install docker
```

Следует проверить наличие у текущего пользователя прав на выполнение команд Docker, 
а также выполнить команду ```sudo usermod -aG docker $USER```, 
после чего перезагрузиться или выполнить ```newgrp docker```. 

Выполняем следующие действия в папке lab-08.

```
$ docker-compose build
$ docker-compose up
```

![image](https://user-images.githubusercontent.com/125077130/234293176-30314658-97db-4dd0-b64c-fbc71c63ebe4.png)


### Docker

Зарегестрироваться на сайте https://hub.docker.com В лабе на github.com: setting -> Secrets and variables -> Actions -> New repository secret

name: ```DOCKER_USERNAME```

secret: логин от Docker Hub

Снова New repository secret

name: ```DOCKER_PASSWORD```

secret: пароль от Docker Hub 


```
$ git add .
$ git commit -m "add all"
$ git push origin master
```

## Скрины

![image](https://user-images.githubusercontent.com/125077130/234292482-30444696-bc32-4b93-a02f-32e428662cb6.png)

![image](https://user-images.githubusercontent.com/125077130/234292535-1421bcd8-6545-4872-b965-2b45c8a52c2b.png)

![image](https://user-images.githubusercontent.com/125077130/234292676-33766f6c-54c8-446c-89a2-f6c98ce78762.png)

![image](https://user-images.githubusercontent.com/125077130/234292729-60b965ef-9dac-4e75-b2a3-f7ebe1269314.png)

![image](https://user-images.githubusercontent.com/125077130/234292864-be9f1d9f-6637-4efb-a6f3-127276c9b2e2.png)

![image](https://user-images.githubusercontent.com/125077130/234292937-34480f26-c1ac-40ba-a234-664184fb696c.png)

![image](https://user-images.githubusercontent.com/125077130/234293554-0582236e-1b06-449a-ac35-3ef3f9d21e58.png)
