---
title: Express JS localhost 에서 OpenSSL 로 HTTPS 서버 만들기
date: 2016-08-05 00:46:32
tags: Java Script, JS, Node JS, Express JS, OpenSSL
---
<p>최신 버전의 Chrome 에서는 HTTPS 가 아닌 HTTP 환경에서 geolocation API 나 getUserMedia 등을 사용할 수 없다.<br />그 말인 즉슨, 일반적인 localhost 에서도 위 API 들을 사용할 수 없다는 것이다.<br />하지만, Open SSL 을 이용해 자체 인증서를 만들어서, 개발용으로 https 서버를 구축할 수 있다.</p>
__개인키 발급__
<p>먼저, 개인키를 발급한다.</p>
```sh
$ openssl genrsa 1024 > key.pem
Generating RSA private key, 1024 bit long modulus
...........................++++++
..........................++++++
e is 65537 (0x10001)
```

__인증서 파일 생성__
<p>개인키를 통해 인증서를 만든다.</p>
```sh
$ openssl req -x509 -new -key key.pem > cert.pem
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:*****
State or Province Name (full name) [Some-State]:*****
Locality Name (eg, city) []:*****
Organization Name (eg, company) [Internet Widgits Pty Ltd]:*****
Organizational Unit Name (eg, section) []:*****
Common Name (e.g. server FQDN or YOUR name) []:*****
Email Address []:*****
```
<p>인증서를 만들기 위해 몇 가지 정보를 기입하면, 인증서 파일 cert.pem 이 만들어진다.</p>

__Express 프로젝트 생성__
```sh
$ express https_test && cd https_test && npm install && npm install --save https fs
```
<p>Express 프로젝트를 생성한 뒤, bin/www 를 수정한다.</p>
```js
/* ... */
var http = require('http');
var https = require('https');

var fs = require('fs');

var options = {
  key: fs.readFileSync('../key.pem'),
  cert: fs.readFileSync('../cert.pem')
};

var HTTP_PORT = 80;
var HTTPS_PORT = 443;

/* ... */
var port = normalizePort(HTTP_PORT);
app.set('port', port);

var httpsPort = normalizePort(HTTPS_PORT);
app.set('httpsPort', httpsPort);

/* ... */
var server = http.createServer(app);
var httpsServer = https.createServer(options, app);

/* ... */
server.listen(port);
server.on('error', onError);
server.on('listening', onListening);

httpsServer.listen(httpsPort);
httpsServer.on('error', onError);
httpsServer.on('listening', onListening);

/* ... */
```

__Node JS 서버 실행__
```sh
$ sudo npm start
```

__확인__
<p>브라우저에서 https://localhost 로 접속을 한다.<br />브라우저 별로 상이한 경고 메시지가 뜨는데, 대부분 고급 버튼을 누르면, 어떻게 어떻게 하세요~ 라는 설명이 있을 것이다.</p>

