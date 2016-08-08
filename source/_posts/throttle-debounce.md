---
title: Throttle 과 debounce 로 이벤트 관리하기
date: 2016-08-08 01:03:32
tags: Java Script, js, throttle, debounce, scroll
---
<p>onmousemove, onscroll 같은 함수는 마우스를 움직이거나 스크롤을 하는 동안 GlobalEventHandlers 에 의해 여러번 호출될 수 있다.<br />당연히, 스크롤과 같은 이벤트를 listening 하고 있으면, 성능이 저하될 수 밖에 없다.<br />브라우저는 매 스크롤 이벤트가 일어나는 순간마다 (심하면, 1 초에도 수 번씩) callback 함수를 실행하게 될 것이기 때문이다.<br />이런 작업을 n 초에 한 번 수행하도록 하는 방식으로 이를 개선할 수 있다.</p>

__throttle__
<p>스크롤링은 debounce 에 적합하지 않다. 스크롤을 움직이는 것은 짧은 시간 간격을 가지기 때문이다.<br />throttle 은 유저가 스크롤을 움직이기 시작하면, 적합한 때에 작업이 일어날 수 있도록 한다.</p>

__debounce__
<p>debounce 는 키 이벤트와 AJAX 를 이용할 때 주로 쓰인다.<br />debounce 를 이용하면, 폼을 입력할 때, 매 타자마다 함수를 실행하지 않고, 타자를 다 치고난 뒤 함수를 실행할 수 있다.</p>

<p>이 둘의 차이는 [링크](https://css-tricks.com/debouncing-throttling-explained-examples/)를 통해 더 자세히 알 수 있다.</p>

__throttle 로 스크롤 이벤트 관리하기__

<p>먼저 debounce 나 throttle 을 적용하지 않은 코드를 보면 다음과 같다.</p>
<p data-height="265" data-theme-id="dark" data-slug-hash="BzqbdB" data-default-tab="js,result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/wonism/pen/BzqbdB/">Listening scroll event</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>
<p>위 예제를 보면, 무엇이 문제인지 알 것이다.<br />이벤트를 미루고, 이를 한 번만 실행할 수 있게 하기 위한 방법은 throttle 과 debounce 두 가지다.<br />throttle 은 시간 주기를 통해 이벤트의 흐름을 보장한다. 반면, debounce 는 이벤트들을 하나의 이벤트로 만든다.<br />throttle 은 시간 기반이고, debounce 는 event driven 기반이다.<br />throttle 과 debounce 는 이러한 차이로 쓰임새가 약간 다르다.</p>

```js
function throttle(cb, interval) {
  var now = Date.now();

  return function() {
    if ((now + interval - Date.now()) < 0) {
      cb();
      now = Date.now();
    }
  }
}
```
<p data-height="265" data-theme-id="dark" data-slug-hash="oLaVyQ" data-default-tab="js,result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/wonism/pen/oLaVyQ/">oLaVyQ</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

__debounce 로 키 이벤트 관리하기__
<p>먼저 debounce 나 throttle 을 적용하지 않은 코드를 보면 다음과 같다.</p>
<p data-height="265" data-theme-id="dark" data-slug-hash="VjENkJ" data-default-tab="js,result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/wonism/pen/VjENkJ/">Listening Keypress event</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>
<p>다음은 debounce 구현체이며, 그 아래는 키 이벤트들을 하나로 묶어서 처리하는 예제이다.</p>

```js
function debounce(cb, interval, immediate) {
  var timeout;

  return function() {
    var context = this, args = arguments;
    var later = function() {
      timeout = null;
      if (!immediate) {
        cb.apply(context, args);
      }
    };

    var callNow = immediate && !timeout;

    clearTimeout(timeout);
    timeout = setTimeout(later, interval);

    if (callNow) {
      cb.apply(context, args);
    }
  };
};
```

<p data-height="265" data-theme-id="dark" data-slug-hash="dXgAxE" data-default-tab="js,result" data-user="wonism" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/wonism/pen/dXgAxE/">Debouncing keypress events</a> by Jaewon (<a href="http://codepen.io/wonism">@wonism</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

