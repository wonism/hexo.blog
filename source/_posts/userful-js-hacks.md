---
title: 유용하게 쓰일 수 있는 자바스크립트 핵
date: 2016-08-03 22:22:47
tags: Java Script, js, tips
---
<p>유용한 Java Script 핵들을 몇 가지 소개하고자 한다. 이 핵들로 코드량을 감축하고 더 나아가, 성능도 향상시킬 수 있을 것이다.</p>

__Boolean 으로 형변환하기__
<p>Boolean 타입으로 형변환하기 위한 방법으로 Boolean 클래스 생성자를 이용하는 방법이 있다.<br />하지만, NOT 연산자인 ! 를 피연산자 앞에 2 번 사용하여 Boolean 타입으로 형변환을 하면, 코드량을 줄일 수 있다.<br />! 연산자는 피연산자 값을 변경하기 전에 피연산자를 불린 값으로 변환한 뒤, 값을 반전시키는데, ! 연산을 2 번 수행하게 되면, 원래 값의 참/거짓 값으로 돌아오기 때문이다.<br />!! 연산을 수행하게 되면, null, undefined, '', 0, NaN, [] 값을 제외한 모든 값들은 true 를 반환하게 된다.</p>

```js
!!null; // false
!!undefined; // false
!!NaN; // false
!!''; // false
!!0; // false
!![]; // true
!!{}; // true
!!-1; // true
```

__숫자로 변환하기__
<p>숫자로 구성된 문자열을 Number 로 바꾸기 위해, 이런 방법을 사용할 것이다. Number('VALUE'), parseInt('VALUE'), parseFloat('VALUER')<br />하지만, 단일 연산자 + 를 사용하면, 더 쉽게 바꿀 수 있다.<br />단항 연산자 + 는 피연산자를 숫자(또는 NaN)로 바꿔주기 때문이다.<br />Date 앞에 + 를 붙여주게 되면, 흔히 말하는 타임스탬프(13 자리 숫자)가 반환된다.<br /></p>

```js
+'1234'; // 1234
+'String'; // NaN
+'0xabcd'; // 43981
+'123e-2'; // 1.23
+'10.5'; // 10.5
```

__논리 연산자 활용 : 조건문 줄이기__
<p>논리 연산자 && 는 앞의 피 연산자가 false 이면 뒤의 피연산자를 실행하지 않는다.<br />이 특징을 단축 평가(short circuiting) 이라고 하는데, 이를 활용하여 조건문을 줄일 수 있다.</p>

```js
var o = { x: 10 };
var p = null;

o && o.x; // 10
p && p.x; // null
// p.x 를 실행하면, TypeError 가 발생한다. 하지만, && 의 왼쪽 피연산자가 false 라 p.x 를 실행하지 않고, 에러가 발생하지 않게 된다.
```

__논리 연산자 활용 : Default 값 지정하기__
<p>논리 연산자 || 는 앞의 피 연산자가 true 이면 뒤의 피연산자를 실행하지 않고, false 이면 뒤의 피연산자를 실행한다.<br />이 특징을 활용해 변수의 Default 값을 설정해줄 수 있다.</p>
```js
function Person(name, age) {
  this.name = name || '홍길동';
  this.age = age || 19;
}

var p1 = new Person('JS', 28);
console.log(p1.name); // 'JS'
console.log(p1.age); // 28

var p2 = new Person();
console.log(p2.name); // '홍길동'
console.log(p2.age); // 19

var p3 = new Person(null, 20);
console.log(p2.name); // '홍길동'
console.log(p2.age); // 20
```

__반복문에서 배열의 길이 캐싱하기__
<p>이 팁은 큰 배열을 사용할 때, 큰 성능 개선 효과를 볼 수 있다.<br />일반적으로, for (var i = 0; i < ARRAY.length; i++) 와 같은 식으로 for 루프를 사용한다.<br />이러한 방법은 작은 배열에서는 상관없지만, 큰 배열에서는 그렇지 않다.<br />매 루프마다 배열에 접근하여 length 를 구하기 때문이다.<br />이를 방지하기 위해, 변수에 배열의 길이를 할당하고 이를 사용한다.</p>
```js
var arr = new Array(10000);

/* A */
for (var a = 0; a < arr.length; a++) { }

/* B */
for (var b = 0, len = arr.length; b < len; b++) { }
```

__배열의 마지막 원소 구하기__
<p>Array 클래스의 slice 메소드는 두 개의 인자를 받는다. 이 때, 첫 번째 인자는 시작 지점, 두 번째 인자는 끝 지점으로 slice 를 통해 두 인자 사이의 원소들을 구할 수 있다.<br />Array.prototype.slice 에 인자 하나만을 전달하면, 2 번째 인자 값을 배열의 길이로 설정하여 메소드를 수행한다.<br />&nbsp;&nbsp;&nbsp;&nbsp;\- 인자로 음수를 받으면, 배열의 길이에서 이 값을 뺀 값을 인자로 전달한 것과 같은 효과를 볼 수 있다.</p></p>
```js
var array = [1,2,3,4,5,6];
console.log(array.slice(-1)); // [6]
console.log(array.slice(-2)); // [5, 6]
console.log(array.slice(-3)); // [4, 5, 6]
```

__배열 자르기__
<p>Array 객체의 length 프로퍼티를 기존 배열의 길이보다 작은 값으로 설정할 경우, 이 값 이상의 인덱스에 해당하는 원소들은 버려지게 된다.</p>
```js
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(arr); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

arr.length = 3;
console.log(arr); // [1, 2, 3]
```

__배열 합치기__
<p>두 배열을 합치고자 할때, Array.prototype.concat 메소드를 사용할 수 있다. 하지만, 이 메소드는 큰 배열을 합칠 때 적합하지 않다.<br />새 배열을 만들면서 많은 메모리를 소비하기 때문이다.<br />concat 메소드 대신, Array.prototype.push.apply 메소드를 사용하면, 메모리 사용량을 절약할 수 있다.<br />(Function.prototype.apply 는 Java Script 의 모든 함수에 사용할 수 있는 메소드이다. 자세한 내용은 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 에서 확인하길 바란다.)</p>

```js
/* concat */
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];

console.log(arr1.concat(arr2)); // [1, 2, 3, 4, 5, 6];

/* push apply */
var arr3 = [1, 2, 3];
var arr4 = [4, 5, 6];

console.log(arr3.push.apply(arr3, arr4)); // [1, 2, 3, 4, 5, 6];
```

