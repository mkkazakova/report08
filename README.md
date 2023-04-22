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
