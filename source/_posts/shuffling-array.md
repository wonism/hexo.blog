---
title: Fisher Yates Shuffle 을 통한 Array 순서 섞기
date: 2016-08-01 21:56:15
tags: [Java Script, js, array, shuffling, random, fisher yates shuffle]
---
## Array 순서 섞기와 순서 정렬하기

<p>Array 를 사용하다 보면 순서를 정렬하거나, 순서를 섞어야 하는 경우가 있다.</p>

### Array 순서 섞기

<p>배열의 순서 정렬은 Array 클래스의 메소드 중 sort 로 구현이 되어 있지만, 순서를 섞는 메소드는 구현되어 있지 않다.<br />이 메소드를 [Fisher Yates Shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)로 구현하고자 한다.<br />먼저, 랜덤한 수를 뽑는 대략적인 순서는 다음과 같다. (Range : 난수 범위, Roll : 해당 회차에서 뽑은 숫자, Scratch : 선택되지 않은 숫자, Result : 랜덤하게 뽑힌 숫자)</p>

| Range | Roll | Scratch | Result |
| :---: | :--: | :-----: | :----- |
| &nbsp; | &nbsp; | 1 2 3 4 5 6 7 8 | &nbsp; |
| 1~8 | 3 | 1 2 ~~3~~ 4 5 6 7 8 | 3 |
| 1~7 | 4 | 1 2 ~~3~~ 4 ~~5~~ 6 7 8 | 3 5 |
| 1~6 | 5 | 1 2 ~~3~~ 4 ~~5~~ 6 ~~7~~ 8 | 3 5 7 |
| 1~5 | 3 | 1 2 ~~3~~ ~~4~~ ~~5~~ 6 ~~7~~ 8 | 3 5 7 4 |
| 1~4 | 4 | 1 2 ~~3~~ ~~4~~ ~~5~~ 6 ~~7~~ ~~8~~ | 3 5 7 4 8 |
| 1~3 | 1 | ~~1~~ 2 ~~3~~ ~~4~~ ~~5~~ 6 ~~7~~ ~~8~~ | 3 5 7 4 8 1 |
| 1~2 | 2 | ~~1~~ 2 ~~3~~ ~~4~~ ~~5~~ ~~6~~ ~~7~~ ~~8~~ | 3 5 7 4 8 1 6 |
| 1~1 | 5 | ~~1~~ ~~2~~ ~~3~~ ~~4~~ ~~5~~ ~~6~~ ~~7~~ ~~8~~ | 3 5 7 4 8 1 2 |

<p>이를 코드로 Array 클래스의 메소드로 구현하면 다음과 같다.</p>

```js
Array.prototype.shuffle = function () {
  for (var i = this.length - 1; i > 0; i--) {
    var j = Math.floor(Math.random() * (i + 1));
    var temp = this[i];
    this[i] = this[j];
    this[j] = temp;
  }
  return this;
};
```

<p>만들어진 메소드를 실험하면, 다음과 같은 결과가 나온다.</p>

```js
var array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15];

array.shuffle(); // [10, 5, 1, 13, 12, 7, 4, 6, 3, 9, 2, 15, 11, 8, 14]
```

### Array 순서 정렬하기

<p>앞서 말했듯이, Array 클래스에는 sort 메소드가 내장되어 있다.<br />사용 방법은 다음과 같다.<br />&nbsp;&nbsp;&nbsp;&nbsp;\- Array.prototype.sort(compareFunction)<br />&nbsp;&nbsp;&nbsp;&nbsp;\- 여기서, 인자 compareFunction 은 함수이며, 원소들을 어떻게 정렬할 지 정할 수 있다.</p>

```js
function ascFn(a, b) {
  return a - b;
}

function descFn(a, b) {
  return b - a;
}

var array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15];

array.shuffle(); // [10, 5, 1, 13, 12, 7, 4, 6, 3, 9, 2, 15, 11, 8, 14]
array.sort(ascFn); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]
array.sort(descFn); // [15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

