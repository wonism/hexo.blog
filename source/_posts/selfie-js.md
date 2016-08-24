---
title: navigator.getUserMedia API 로 셀피 찍기
date: 2016-08-23 23:38:47
tags: Java Script, js, Camera, Selfie, Photo, getUserMedia, 카메라
---

__Navigator.getUserMedia()__
<p>getUserMedia 는 사용자의 단말기에서 미디어 스트림을 받는 API 로 오디오나 비디오 등의 미디어 스트림 정보를 가져올 수 있다.</p>
<p>기본적인 사용방법은 다음과 같다.`navigator.getUserMedia(constraints, successCallback, errorCallback);`<br />여기서 `constraints`는 미디어에 어떠한 타입을 요청할지를 정의하는 객체로, `audio`, `video` 등을 받을 수 있다.<br />`successCallback` 은 MediaStream 을 인자로 받는, getUserMedia API 가 성공적으로 호출된 뒤에 실행되는 함수이다.<br />마지막으로 `errorCallbak` 은 MediaStreamError 를 인자로 받으며, getUserMedia API 를 호출하는 데 실패하면 실행되는 함수이다.</p>
<p>*주의사항*<br />getUserMedia() 는 Chrome 21, Opera 18, Firefox 17 이후 버전에서 사용가능하며, 크롬 47 이후 버전부터는 https 환경에서만 getUserMedia() 를 사용할 수 있다. (localhost 에선 사용 가능)<br />링크 : [https://goo.gl/rStTGz](https://goo.gle/rStTGz)</p>
<p>*참고*<br />[Let's Encrypt 로 HTTPS 사용하기](https://wonism.github.io/2016/07/26/letsencrypt) 는 Let's Encrypt 와 crontab 을 통해 웹서버에 SSL 보안을 적용하는 방법을 설명한 포스트이다.</p>
<p>이제 canvas 와 video 태그, getUserMedia() 로 사진을 찍을 수 있는 간단한 웹 애플리케이션을 만들어 보겠다.</p>

<p>먼저, 필요한 Tag 들을 HTML 파일에 작성한다.</p>
```html
<h1>Selfie with Java Script</h1>
<div class="" id="selfie-wrapper">
  <video class="" id="selfie-video"></video>
  <img class="" id="selfie-image" src="">
  <button class="" id="selfie-shutter">촬영</button>
  <button class="" id="selfie-remove">삭제</button>
  <a class="" id="selfie-download" download="selfie.png">받기</a>
  <canvas class="" id="selfie-store" style="display: none;"></canvas>
</div>
```

<p>다음으로는 getUserMedia() 를 호출한다.</p>

```js
// 카메라에 담기고 있는 화면 (video element)
var video = document.getElementById('selfie-video');
// 찍힌 사진
var image = document.getElementById('selfie-image');
// 셔터 버튼
var shutter = document.getElementById('selfie-shutter');
// 삭제 버튼
var remove = document.getElementById('selfie-remove');
// 받기 버튼
var download = document.getElementById('selfie-download');
// 이미지를 그릴 <canvas> element
var store = document.getElementById('selfie-store');

// 브라우저에서 지원하는 객체를 getUserMedia 에 저장
var getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia || navigator.msGetUserMedia;

if (!getUserMedia) {
  throw new Error('This browser is not support user media.');
} else {
  getUserMedia.call(navigator, {
    video: true
  }, function (stream) {
    if (window.URL) {
      video.src = window.URL.createObjectURL(stream);
    } else {
      video.src = stream;
    }

    // video 가 재생되면, display 함수를 실행
    video.onplay = function () {
      display();
    };

    video.play();
  }, function (err) {
    alert(err.message);
  });
}

function display() {
  video.setAttribute('style', 'display: block;');
}

function takePhoto() {
  // 캔버스에 그림을 그리기 위해, Canvas RenderingContext2D 값을 가져온다.
  var context = store.getContext('2d');

  // 그림의 크기는 video 의 크기와 같다.
  var width = video.videoWidth,
      height = video.videoHeight;

  if (width && height) {
    store.width = width;
    store.height = height;

    // (0, 0) 위치에서부터 비디오의 크기만큼의 좌표까지 video 의 이미지를 그린다.
    context.drawImage(video, 0, 0, width, height);

    // 캔버스에 그린 그림을 주소 형태로 반환한다.
    return store.toDataURL('image/png');
  }
}

shutter.onclick = function () {
  // 찍은 사진을 저장한다.
  var snap = takePhoto();

  // 이미지의 소스와 다운로드 링크 주소를 캔버스에서 그린 그림의 주소 값으로 한다.
  image.setAttribute('src', snap);
  image.setAttribute('style', 'display: block;');
  download.setAttribute('href', snap);
};

remove.onclick = function () {
  // 이미지의 소스와 다운로드 링크 주소를 제거한다.
  image.setAttribute('src', '');
  image.setAttribute('style', 'display: none;');
  download.setAttribute('href', '');
};
var remove = document.getElementById('selfie-remove');
var download = document.getElementById('selfie-download');
```
<p>실행 결과는 아래와 같다.</p>

<h1>Selfie with Java Script</h1>
<div class="" id="selfie-wrapper">
  <video class="" id="selfie-video"></video><img class="" id="selfie-image" src="">
  <button class="" id="selfie-shutter">촬영</button><button class="" id="selfie-remove">삭제</button><a class="" id="selfie-download" download="selfie.png">받기</a><canvas class="" id="selfie-store" style="display: none;"></canvas>
</div>

<script>
var video = document.getElementById('selfie-video');
var image = document.getElementById('selfie-image');
var shutter = document.getElementById('selfie-shutter');
var remove = document.getElementById('selfie-remove');
var download = document.getElementById('selfie-download');
var store = document.getElementById('selfie-store');

var getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia || navigator.msGetUserMedia;

if (!getUserMedia) {
  throw new Error('This browser is not support user media.');
} else {
  getUserMedia.call(navigator, {
    video: true
  }, function (stream) {
    if (window.URL) {
      video.src = window.URL.createObjectURL(stream);
    } else {
      video.src = stream;
    }

    video.onplay = function () {
      display();
    };

    video.play();
  }, function (err) {
    alert(err.message);
  });
}

function display() {
  video.setAttribute('style', 'display: block;');
}

function takePhoto() {
  var context = store.getContext('2d');

  var width = video.videoWidth,
      height = video.videoHeight;

  if (width && height) {
    store.width = width;
    store.height = height;

    context.drawImage(video, 0, 0, width, height);

    return store.toDataURL('image/png');
  }
}

shutter.onclick = function () {
  var snap = takePhoto();

  image.setAttribute('src', snap);
  image.setAttribute('style', 'display: block;');
  download.setAttribute('href', snap);
};

remove.onclick = function () {
  image.setAttribute('src', '');
  image.setAttribute('style', 'display: none;');
  download.setAttribute('href', '');
};
var remove = document.getElementById('selfie-remove');
var download = document.getElementById('selfie-download');
</script>

