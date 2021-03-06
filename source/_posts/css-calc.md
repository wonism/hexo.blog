---
title: CSS 의 calc()
date: 2016-08-10 00:03:09
tags: CSS, calc
---
<p>CSS 함수 calc() 는 CSS3 에 추가된 기능 중 하나로, 계산을 해주는 속성이다.<br />calc() 는 &lt;length&gt;, &lt;frequency&gt;, &lt;angle&gt;, &lt;time&gt;, &lt;number&gt;, 또는 &lt;integer&gt; 가 필요한 곳 어디서든 사용 가능하며, 기존에 자바스크립트로 하던 계산을 대신 해줄 수 있다.<br />예를 들어, "100% 너비에서 50px 만큼을 뺀 만큼의 너비를 사용하고 싶다"면, 다음과 같이 코드를 작성한다.</p>
```css
* {
  /* So 100% means 100% */
  box-sizing: border-box;
}

p {
  width : 95%; /* calc() 를 지원하지 않는 브라우저를 위한 fallback */
  width : -webkit-calc(100% - 80px); /* WebKit */
  width : -moz-calc(100% - 80px); /* Firefox */
  width : -ms-calc(100% - 80px); /* MS Explorer */
  width : -o-calc(100% - 80px); /* Opera */
  width : calc(100% - 80px); /* Standard */
}
```
<p>calc() 내부에서는 \+, \-, \*, / 등의 사칙 연산이 가능하다.<br />이 때, 주의할 것은 \+ 연산과 \- 연산 시, 반드시 연산자의 양쪽에 공백이 한 칸 있어야 한다는 것이다.<br />또한, 같은 형식(길이는 길이 끼리, 각도는 각도끼리)을 갖는 피연산자 끼리만 연산이 가능하다.</p>

<p>다음 코드펜은 calc() 와 vh(View Height, 높이값의 100분의 1 값) 등 CSS 만을 사용하여 구현한 Scroll Indicator 이다.</p>
<p data-height="265" data-theme-id="dark" data-slug-hash="ZOrEmr" data-default-tab="html,result" data-user="MadeByMike" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/MadeByMike/pen/ZOrEmr/">CSS only scroll indicator</a> by Mike (<a href="http://codepen.io/MadeByMike">@MadeByMike</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

