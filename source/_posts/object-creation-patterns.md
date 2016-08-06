---
title: 자바스크립트 객체 생성 패턴
date: 2016-08-06 21:46:00
tags: Java Script, JS, prototype, oop, object, json
---
<p>자바스크립트 객체를 생성하는 스타일에는 여러 가지가 있다.<br />예제를 통해 '객체 리터럴', '팩토리 함수', '프로토타입 체인', 'ES5 / ES6 클래스' 로 객체를 생성하는 방법을 볼 것이다.</p>

__객체 리터럴__
<p>객체를 생성하는 가장 간단한 방법은 객체 리터럴을 사용하는 것이다.<br />하지만, 같은 타입의 객체를 만들고자 할 때, 메소드와 프로퍼티에 대해 코드를 복사하여 붙여넣을 수 밖에 없다는 단점이 있다.</p>
```js
var o = {
  x: 'hi',
  y: 5,
  f: function () {},
  g: function () {}
};
```

__Factory 함수__
<p>팩토리 함수를 이용하는 것은 같은 구조를 갖는 객체를 만들 수 있는 가장 간단한 방식이다.<br />객체 리터럴을 반환하는 이 함수를 통해, 같은 타입의 객체를 여러 개 만들 수 있다.<br />하지만, 매 호출되는 함수마다 객체를 포함하기 때문에 메모리를 과하게 사용한다는 단점이 있다.<br />[유투브 동영상 강의](https://www.youtube.com/watch?v=ImwrezYhw4w) 에서 더 자세한 내용을 알 수 있다.</p>
```js
function factoryFn() {
  return {
    x: 'hi',
    y: 5,
    f: function () { },
    g: function () { }
  };
}

var o = factoryFn();
```

__프로토타입__
<p>프로토타입은 생성자 함수를 통해 생성된 객체들이 공통으로 가지게 되는 속성이나 메소드를 가지는 객체로, 각각 객체들은 이 프로토타입 객체를 공유하게 되어, 메모리를 절약할 수 있다.</p>
<p>\* 프로토타입 체인이란, 함수 객체의 prototype 이라는 속성을 통해 객체가 연결되는 것이다. 이 때, 이 연결은 \_\_proto\_\_ 를 통해 연결된다.<br />이러한 특징으로, 공유된 객체를 통해 객체를 생성할 수 있다.</p>
```js
var thingPrototype = {
  f: function () {},
  g: function () {}
};

function thing() {
  var o = Object.create(thingPrototype);

  o.x = 42;
  o.y = 3.14;

  return o;
}

var o = thing();
```

<p>위 예제를 보면, 함수 thing 에서 Object.prototype.create 메소드와 thingPrototype 을 통해 객체를 만들고, 객체의 프로퍼티들을 설정해 준 뒤, 이를 반환하고 있다.<br />(Object.prototype.create 메소드에서 인자로 부모 객체로 사용할 객체를 받는데, 이를 통해 코드를 쉽게 재활용 할 수 있다. 이 메소드는 2 번째 매개변수를 받을 수 있는데, 이는 반환되는 자식 객체의 속성에 추가되는 부분이다.)<br />하지만 이때, 공유된 객체 thingPrototype 을 따로 만들 필요가 없으므로, 다음과 같이 코드를 수정할 수 있다.</p>

```js
function thing() {
  var o = Object.create(thing.prototype);

  o.x = 42;
  o.y = 3.14;

  return o;
}

thing.prototype.f = function () {};
thing.prototype.g = function () {};

var o = thing();
```

<p>instanceof<br /><br />instanceof 는 해당 객체가 어떤 생성자 함수를 통해 생성됐는지를 알 수 있는 함수로 true 나 false 를 반환한다.</p>

__ES5 Class__
<p>자바스크립트에서 클래스는 프로토타입 기반의 상속 매커니즘을 기반으로 한다.<br />메소드들과 프로퍼티를 지정하는 클래스 함수를 기반으로 객체를 만들 수 있다.</p>
```js
function create(fn) {
  var o = Object.create(fn.prototype);

  fn.call(o);

  return o;
}

// ...

function Thing() {
  this.x = 42;
  this.y = 3.14;
}

Thing.prototype.f = function() {};
Thing.prototype.g = function() {};

var o = create(Thing);
```

<p>create 함수를 사용하는 대신, new 키워드를 이용할 수 있다.</p>

```js
function Thing() {
  this.x = 42;
  this.y = 3.14;
}

Thing.prototype.f = function() {};
Thing.prototype.g = function() {};

var o = new Thing();
```

__ES6 Class__
<p>ES6 클래스는 ES5 클래스보다 깔끔한 문법을 갖는다.<br />constructor() 를 통해 프로퍼티를 설정할 수 있고, 메소드를 선언할 때도, 클래스 내부에 선언한다.</p>
```js
class Thing {
  constructor() {
    this.x = 42;
    this.y = 3.14;
  }

  f() {}
  g() {}
}

var o = new Thing();
```

