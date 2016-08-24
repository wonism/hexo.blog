---
title: Node JS 에서 ImageMagick 을 이용한 이미지 다루기
date: 2016-08-25 00:36:34
tags: imagemagick, image processing, Node JS
---
<p>Image Magick 사용 방법에 대한 포스트이다.</p>
<p>Image Magick은 Ruby, Python, Go 등에서도 이미지 변환을 위해 많이 사용되며, 당연히, Node.js도 지원한다.</p>
<p>먼저 Image Magick 을 사용하기 위해서는<br />imagemagick을 설치한다.</p>
<p><ul><li>데비안/우분투의 경우</li></ul>
```sh
$ sudo apt-get install imagemagick
```
<ul><li>OS X의 경우</li></ul>
```sh
$ brew install imagemagick
```
로 imagemagick 을 설치한다.<br />그리고, npm 을 통해 imagemagick 모듈을 설치한다.</p>
```sh
$ npm install imagemagick
```
<p>이로써 imagemagick을 사용할 준비를 마쳤으며, 간단한 코드를 통해 대략적인 사용 방법을 알아 보겠다.<br />이미지를 200x200 크기로 크로핑 및 리사이징하여 썸네일 이미지를 만드는 예제 코드이다.</p>
```js
const im = require('imagemagick');

im.crop({
  srcPath: 'PATH/TO/IMAGE', // 다루려는 이미지
  dstPath: 'PATH/TO/DIST', // 프로세싱한 결과물의 주소
  width: 200, // 가로 길이
  height: 200, // 세로 길이
  quality: 0.8, // 이미지의 화질
  gravity: 'Center' // 8 방면 + 중앙 중 어디를 중심으로 이미지를 크롭할 지 정할 수 있는 옵션
}, (error, stdout, stderror) => {
  if (err) throw err;
  // TODO
});
```
<p>*참고*<br />더 많은 코드는 github node-imagemagick repository 에 있다. (https://github.com/rsms/node-imagemagick)</p>
