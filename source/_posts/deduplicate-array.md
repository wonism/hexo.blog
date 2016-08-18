---
title: Array 에서 중복되는 원소 제거하기
date: 2016-08-16 23:16:00
tags: [Java Script, js, array, duplicate, deduplicate, 중복 원소 제거]
---

<h3 id="배열이-원시-데이터-타입으로-된-원소만-가질-경우"><a href="#배열이-원시-데이터-타입으로-된-원소만-가질-경우" class="headerlink" title="배열이 원시 데이터 타입으로 된 원소만 가질 경우"></a>배열이 원시 데이터 타입으로 된 원소만 가질 경우</h3>
__ES5__
<p>배열이 원시 데이터 타입만을 포함할 경우, Array.prototype.filter() 메소드와 Array.prototype.indexOf() 메소드 를 사용하여, 중복 원소를 제거할 수 있다.</p>
<ul><li>Array.prototype.filter() : 매개 변수로 받는 함수의 반환값으로 true 인 것들로만 구성된 배열을 생성한다.</li><li>Array.prototype.indexOf() : 매개 변수로 받은 값을 배열에서 찾으며, 해당되는 인덱스를 반환한다.</li></ul></p>
```js
var uniqEls = [1, 2, 'a', 'a', 2].filter(function (el, i, arr) {
  return arr.indexOf(el) === i;
});

console.log(uniqEls); // [ 1, 2, 'a' ]
```

__ES6__
<p>arrow function 을 사용하면, 위 코드를 아래와 같이 작성할 수 있다.</p>
```js
let uniqEls = [ 1, 2, 'a', 'a', 2 ].filter((el, i, arr) => arr.indexOf(el) === i);
console.log(uniqEls); // [ 1, 2, 'a' ]
```
<p>Set 객체는 유니크한 값을 저장할 수 있게 해주는데, 이를 사용하면, 다음과 같이 코드를 작성할 수 있다.</p>
<ul><li>Array.from() 메소드는 유사 배열 혹은 반복 가능한 객체로부터 새 Array 인스턴스를 생성한다.</li></ul>
```js
let uniqEls = Array.from(new Set([ 1, 2, 'a', 'a', 2 ]));
console.log(uniqEls); // [ 1, 2, 'a' ]
```
<p>... 는 spread operator 로 표현식이 여러 인자(함수 호출 시)나 여러 원소(배열 리터럴 사용 시) 등이 예상되는 곳에 확장될 수 있도록 하는 연산자이다. 이를 사용하면, 아래와 같이 코드를 작성할 수 있다.</p>
```js
let uniqEls = [...new Set([1, 2, 'a', 'a', 2])];
console.log(uniqEls); // [ 1, 2, 'a' ]
```

<h3 id="배열이-객체로-된-원소를-가질-경우"><a href="#배열이-객체로-된-원소를-가질-경우" class="headerlink" title="배열이 객체로 된 원소를 가질 경우"></a>배열이 객체로 된 원소를 가질 경우</h3>

<p>원시 데이터 타입은 값으로 저장되는 반면, 객체는 참조로 저장된다.</p>
```js
1 === 1 // true
'a' === 'a' // true
{ a: 1 } === { a: 1 } // false
```
<p>따라서, 중복 원소를 제거할 때, 위와 같은 방식으로 접근해서는 안되고, 다음과 같은 식으로 접근한다.</p>
```js
function rmDupl(arr) {
  let hashTable = {};

  return arr.filter((el) => {
    let key = JSON.stringify(el);
    let alreadyExist = !!hashTable[key];

    return (alreadyExist ? false : hashTable[key] = true);
  });
}

let uniqEls = rmDupl([
  { a: 1 },
  { a: '1' },
  { a: 1, b: 3 },
  { a: 1 },
  1,
  '1',
  [ 1, 2, 3 ],
  [ 1, 2, 3 ]
]);

console.log(uniqEls); // [ { a: 1 }, { a: '1' }, { a: 1, b: 3 }, 1, '1' ,[1, 2, 3] ]
```
<p>rmDupl 함수가 실행될 때, hashTable 은 아무런 property 를 갖지 않는 객체로 초기화된다.<br />filtering 함수 내부에서 key 는 중복을 제거할 배열의 원소를 JSON.stringify 를 통해 문자열로 만든 값이다.<br />alreadyExist 는 hashTable 에 위에서 초기화된 key 값을 프로퍼티로 가지고 있으면, true, 아니면 false 로 초기화된다.<br />그리고 마지막에 alreadyExist 값에 따라 false 나 true 를 반환하게되어, 중복된 원소들을 제거하게 되는 것이다.</p>

__참고__
<a href="http://stackoverflow.com/questions/9229645/remove-duplicates-from-javascript-array/9229821#9229821" target="_blank">SO - Remove Duplicates from JavaScript Array</a>

