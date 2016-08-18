---
title: Instagram API 토큰 발급받기
date: 2016-08-18 20:04:58
tags: instagram, api, access token
---
<p>인스타그램의 API 를 사용하려면, access token 이 있어야 한다.</p>
<p>토큰을 발급받기 위해, 인스타그램 클라이언트 매니지 페이지에서 애플리케이션을 등록하고 인증을 한다.</p>

__애플리케이션 등록하기__
<ol>
<li>[인스타그램 클라이언트 매니지 페이지](https://www.instagram.com/developer/clients/manage/) 에 접속한다.</li>
<li>*Register a New Client* 버튼을 누르고, 해당 웹 애플리케이션에 대한 정보들을 입력한다.</li>
<li>다음, Manage Clients 에서 방금 만든 애플리케이션의 *MANAGE* 버튼을 누르고, Security 탭에서 *Disable implicit OAuth* 의 체크를 해제한다.</li>
</ol>

__인증하기__
<p>먼저 브라우저로 [https://api.instagram.com/oauth/authorize/?client_id=CLIENT-ID&redirect_uri=REDIRECT-URI&response_type=code](https://api.instagram.com/oauth/authorize/?client_id=CLIENT-ID&redirect_uri=REDIRECT-URI&response_type=code)에 접속한다.</p>
<ul>
<li>CLIENT-ID : 애플리케이션을 등록한 뒤 발급된 ID 로 클라이언트 매니지 페이지에서 볼 수 있다.</li>
<li>REDIRECT-URI : 애플리케이션을 등록할 때, *Valid redirect URIs* 를 입력한 주소를 인코딩한다.(콘솔창을 띄우고, encodeURIComponent(주소) 를 실행하면 된다.)</li>
</ul>
<p>그럼, [http://HOST/?code=CODE_VALUE](http://HOST/?code=CODE_VALUE) 와 같이 쿼리스트링이 붙은 채로 리다이렉트된 화면을 볼 수 있다.<br />code 의 값을 메모해둔다.</p>
<p>다음, curl 명령어로 인스타그램 api 서버에 인증 리퀘스트를 보낸다.</p>
```sh
curl -F 'client_id=CLIENT_ID' \
-F 'client_secret=CLIENT_SECRET' \
-F 'grant_type=authorization_code' \
-F 'redirect_uri=AUTHORIZATION_REDIRECT_URI' \
-F 'code=CODE' \
https://api.instagram.com/oauth/access_token
```
<p>그럼 다음과 같은 JSON 형식의 텍스트가 올 것이다.<br />이제 이 토큰을 통해 Instagram 의 API 를 사용할 수 있다.</p>
```sh
{"access_token": "ACCESS_TOKEN", "user": {"username": "INSTAGRAM_ID", "bio": "", "website": "", "profile_picture": "USER_PROFILE_IMAGE", "full_name": "USER_NAME", "id": "USER_ID"}}
```

