---
title: for 반복문과 forEach() 반복문
date: 2016-08-07 00:13:58
tags: Java Script, js, for, foreach, loop
---

<p>배열의 길이만큼 반복을 할 때, for 와 forEach 를 사용할 수 있다.</p>

```js
var matrix = [
  [1, 2, 3],
  [4, 5, 6, 7],
  [8, 9, 10, 11, 12]
];

for (var i = 0, len = matrix.length; i < len; i++) {
  console.log(matrix[i]);
}

matrix.forEach(function(element, index) {
  console.log(element);
});
```
<p>for loop 에는 3 가지 표현식이 있다.<br />먼저, 첫 번째 표현식 var i = 0, len = matrix.length; 는 반복문이 시작되기 전에 실행된다.<br />그 다음 두번째 표현식 i < len 은 반복문의 조건식이다.<br />세 번째 표현식 i++ 는 매 루프가 끝나면 수행된다.</p><p>Array.prototype.forEach() 메소드는 인자로 함수를 받는다. 이 함수는 매 반복마다 실행되며, 원소와 인덱스를 인자로 받는다.</p>

__for vs forEach__
<p>반복문을 수행할 때, forEach 는 for 에 비해 가독성이 좋다.(성능은 for 가 forEach 보다 좋지만, 여기서는 다루지 않는다.)<br />위의 코드로 예를 들면, 배열의 원소 element 는 매 반복마다 함수에 인자로 전달되므로 products[i] 처럼 배열에 접근할 필요가 없다.<br />특히, for loop 로 배열 안의 배열에 접근을 한다면, 코드는 다음과 같아질 것이다.</p>
```js
for (var i = 0; i < maxtix.length; i++) {
  console.log(matrix[i]);
  for (var j = 0; j < matrix[i].length; j++) {
    console.log(matrix[i][j]);
  }
}

/* or */
for (var i = 0; i < maxtix.length; i++) {
  var m = matrix[i];
  console.log(m);
  for (var j = 0; j < m.length; j++) {
    var m2 = m[j];
    console.log(m2);
  }
}
```

<p>하지만, forEach 를 사용하면, i 나 j 같은 카운트 변수가 필요 없어지며, 읽기에도 더 쉬워진다.</p>

```js
matrix.forEach(function (m) {
  m.forEach(fuction (m2) {
    console.log(m2);
  });
});
```

<p>또한, 다음과 같은 실수를 예방할 수 있다.</p>
```js
var arr = [1, 2, 3, 4];
for (var i = 0; i <= arr.length; i++) {
  console.log(arr[i]);
}
```

__반복문 중간에 멈추기__
<p>for 루프 에서는 break(); 를 통해 반복문을 멈출 수 있다.</p>
```js
var arr = [1, 2, 3, 4];
for (var i = 0; i < arr.length; i++) {
  if (arr[i] === 3) {
    // Do something ...
    break;
  }
}
```
<p>Array 클래스의 메소드에는 find 가 있는데 일부 브라우저에서는 구현되지 않았으므로, 폴리필을 추가하거나, Babel 과 같은 transpiler 를 사용해야 한다.</p>
```js
var arr = [1, 2, 3, 4];

function findThree(element) {
  return element === 3;
}

console.log(arr.find(findThree));
```

