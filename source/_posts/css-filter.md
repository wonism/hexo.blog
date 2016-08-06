---
title: CSS Filters
date: 2016-08-06 03:28:54
tags: CSS, Filter, W3C, Blur, Brightness, Grayscale, Contrast
---
<p>예전엔, 필터는 SVC 스펙이었지만, W3C 에 의해 필터 효과가 CSS 에 추가되었다.(하지만, 대다수의 브라우저에서 사용할 때, vendor-prefix 를 사용해야 한다.)<br />CSS 필터는 사용하기도 쉽고, 다양항 효과(blur, brighten, grayscale, contrast 등) 들을 구현할 수 있다.<br />또한, 다른 필터와 조합하여 사용할 수도 있다.</p>

### filter syntax
```css
SOME-ELEMENT {
  filter: <filter-function> [<filter-function>]* | none
}
```

__Brightness 효과__
<p>
brightness 필터는 이미지의 밝기를 조정한다.<br />brightness 필터는 0 이상의 값을 인자로 받는다. (낮을 수록 어두워 지며, 0% 가 인자로 오면, 시커먼 화면이 나오게 되며, 100% 가 인자로 오면, 원래의 밝기를 가진다.)<br />아래 예제에서는 200% 를 인자로 가지며, 2 배 밝아진다.</p>
```css
ELEMENT {
  filter: brightness(200%);
}
```
<p data-height="265" data-theme-id="dark" data-slug-hash="ZOqjmv" data-default-tab="result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="https://codepen.io/wonism/pen/ZOqjmv/">ZOqjmv</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

__Contrast 효과__
<p>contrast 필터는 이미지의 대비를 조정한다.<br />brightness 와 마찬가지로 0 이상의 값을 인자로 받는다. (낮을수록 회색 빛을 띄며, 높을수록 색이 원색을 띄게 된다.)<br /></p>
```css
ELEMENT {
  filter: contrast(50%);
}
```
<p data-height="265" data-theme-id="dark" data-slug-hash="BzqPgz" data-default-tab="result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/wonism/pen/BzqPgz/">BzqPgz</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

__Grayscale 효과__
<p>grayscale 필터는 이미지에 회색 음영을 준다.<br />인자로 0 이상의 값을 받으며, 0 일 경우, 원본의 이미지, 100% 일 경우 완전한 흑백 이미지로 바뀐다.</p>
```css
ELEMENT {
  filter: grayscale(50%);
}
```
<p data-height="265" data-theme-id="dark" data-slug-hash="RRZEEK" data-default-tab="result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/wonism/pen/RRZEEK/">RRZEEK</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

__Invert 효과__
<p>Invert 필터는 색상을 반전시킨다.<br />인자로 0 이상의 값을 받으며, 0 의 경우, 원본의 이미지, 100% 일 경우 색상이 완전히 반전된 이미지로 바뀐다.</p>
```css
ELEMENT {
  filter: invert(90%);
}
```
<p data-height="265" data-theme-id="dark" data-slug-hash="dXgwBX" data-default-tab="result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/wonism/pen/dXgwBX/">CSS Filter (Invert)</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

__Blur 효과__
<p>Blu 필터는 이미지에 가우시안 블러를 적용한다.<br />이 필터는 이미지의 색상을 번지게 하며, 이미지 외곽선으로도 번지게 한다.<br />인자로는 px 단위의 숫자를 입력받는데, 높은 값이 올 수록 많이 번지게 된다.</p>
```css
ELEMENT {
  filter: blur(10px);
}
```
<p data-height="265" data-theme-id="dark" data-slug-hash="PzyVYO" data-default-tab="result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/wonism/pen/PzyVYO/">CSS Filter (blur)</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

__여러 효과 중첩하기__
<p>맨 위에서 살펴본 Filter 속성의 Syntax 를 보면 알 수 있듯이, 여러 효과를 주기 위해 Filter 이름과 값 들을 띄어쓰기로 구분하면 된다.</p>
<p data-height="265" data-theme-id="dark" data-slug-hash="oLamNL" data-default-tab="result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/wonism/pen/oLamNL/">oLamNL</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

<p>이외에도 많은 효과들이 있으며, [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/filter) 에 가면 더 많은 효과 예제들을 볼 수 있다.</p>




