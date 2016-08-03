---
title: Java Script String
date: 2016-08-01 22:58:58
tags: [Java Script, js, String, 문자열]
---
## Java Script String
<p>문자열은 16 비트 값들이 연속적으로 나열된 변경 불가능한 값으로, 각 문자는 유니코드 문자로 표현된다.<br />&nbsp;&nbsp;&nbsp;&nbsp;\- 자바스크립트는 유니코드 문자열에 대해 정규화 과정을 수행하지 않는다. 따라서, 같은 문자로 보이더라도 다르게 인코딩이 되어있는 경우가 있다.<br />&nbsp;&nbsp;&nbsp;&nbsp;\- 문자열 비교 연산을 수행할 때, 이러한 문제를 해결하려면, [String.prototype.localeCompare 메소드](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)를 사용한다.</p>

### 문자열 리터럴
<p>문자열 그대로를 포함하려면 단순히 문자열을 작은 따홈표 혹은 큰 따옴표로 둘러싸면 된다.<br />Airbnb 의 [Java Script Style Guide](https://github.com/airbnb/javascript#strings) 에 따르면, 작은 따옴표를 쓰길 권장한다.<br />ES3 에서는 문자열 리터럴은 한 줄로 작성해야 하지만, ES5 에서는 줄 끝에 역슬래시를 쓰면, 한 줄을 여러 줄로 작성할 수 있다.<br />문자열 리터럴에 줄바꿈 문자를 포함시켜야 한다면, \n 을 사용한다.<br />템플릿 리터럴로 문자열을 표현할 수 있다.<br />&nbsp;&nbsp;&nbsp;&nbsp;\- 따옴표 대신 back-tick(\`) 를 사용한다.<br />&nbsp;&nbsp;&nbsp;&nbsp;\- ES6 명세에서는 Template literals 라고 하며, 이전의 명세에서는 Template Strings 라고 불려졌다.<br />&nbsp;&nbsp;&nbsp;&nbsp;\- multi line string 과 string interpolation 를 사용할 수 있다.<br />&nbsp;&nbsp;&nbsp;&nbsp;\- Internet Explorer 에서는 사용할 수 없다. (IE 11 에서도 사용할 수 **없다.**)</p>

```js
'two\nlines'
/*
 * two
 * lines
 */

'one\
long\
line'
/*
 * onelongline
 */


var a = 2;
var b = 3;

`this is
template
literal.
a + b = ${ a + b }
`
/*
 * this is
 * template
 * literal.
 * a + b = 5
 */
```

<p>''로 감싸진 문자열에서 ' 를 이스케이프 하기 위해서는 \ 를 앞에 붙여준다.</p>

```js
'Hi! I\'m Jaewon.'
```

### String 클래스의 프로퍼티 및 메소드
<p>String 클래스의 프로퍼티와 메소드를 사용하면, 문자열의 길이를 구하거나, 특정 문자열 치환, 분리 등의 작업을 쉽게 수행할 수 있다.<br />다음 예제는 자주 쓰이는 String 관련 메소드들이다.</p>

```js
var str = 'Hello, World!';

/**** 문자열 길이 구하기 *****/
str.length; // 13

/**** 특정 위치 문자 구하기 *****/
str.charAt(0); // H : 첫 번째 문자
str.charAt(str.length - 1); // ! : 마지막 문자

/**** 특정 구간 문자열 구하기 *****/
str.substring(1, 4); // ell : 1 ~ 4 까지 (1 ~ 2 : e / 2 ~ 3 : l / 3 ~ 4 : l) String 객체의 부분 집합을 반환한다.
str.substring(4, 1); // 위와 같다.
// 모든 인자의 최소값은 0 이고, 최대값은 str.length - 1 이다.

/**** 문자열 더하기 *****/
str.concat(' Nice to meet you!'); // 'Hello, World! Nice to meet you!'
str.concat('1', '2', '3'); // 'Hello, World!123'
str + ' Nice to meet you!'); // 'Hello, World! Nice to meet you!'
str + '1' + '2' + '3'; // 'Hello, World!123'
// + 연산자를 통해 문자열을 더하는 것이 훨씬 쉽지만, 성능상의 이점을 위해 concat 메소드를 이용하길 권장한다.

/**** 문자열 자르기 *****/
str.slice(1, 4); // ell : 1 ~ 4 이외의 문자열을 제거하고, 새 문자열을 반환한다.
str.slice(4, 1); // '' : 4 위치에서부터 1 번째 위치까지 잘라낸다.
str.slice(1, -1); // str.slice(1, str.length - 1);
// 인자에서 음수는 str.length 에서 해당 수의 절대값을 뺀 값이다.

/**** 특정 문자열 포함하는지 여부 확인하기 *****/
str.match(/\wo/g); // ['lo', 'wo'] : match 메소드의 인자 RegExp 와 매칭되는 문자열을 배열로 반환한다.
str.match(/\d/); // null
// includes 는 IE 에서 지원하지 않는다.
str.includes('el'); // true
str.includes('Hi'); // false

/**** 특정 문자 위치 찾기 *****/
str.indexOf('l'); // 2 : 문자 l 이 위치한 첫 번째 위치
str.lastIndexOf('l'); // 10 : 문자 l 이 위치한 마지막 위치
str.indexOf('l', 3); // 3 : 세 번째 문자 이후(포함), 문자 l 이 등장하는 첫 위치

/**** 문자열 분리하기 *****/
str.split(', '); // ['Hello', 'World!'] 문자열 Hello, World! 를 문자열 ', ' 로 구분하고, 이를 배열로 반환한다.

/***** 문자열 치환하기 *****/
str.replace('H', 'h'); // hello, World! : h 를 H 로 치환한다.
// 첫 번째 인자에 RegExp 가 올 수 있다.
str.replace(/l/g, 'L'); // heLLo, WorLd : 모든 l 을 L 로 치환한다.
str.replace(/\w/g, '0'); // 00000, 00000! : 모든 문자(알파벳, 숫자, _ 등) 를 0 으로 치환한다.

/***** 문자열 반복하기 *****/
// repeat 은 IE 에서 지원하지 않는다.
str.repeat(-1); // RangeError : 인자로 0 이상의 수가 와야한다.
str.repeat(0); // ''
str.repeat(1.5); // 'Hello, World!' : 소수점은 버림한다.
str.repeat(3); // 'Hello, World!Hello, World!Hello, World!'

/***** 문자열 대소문자로 변경하기 *****/
str.toUpperCase(); // HELLO, WORLD! : 대문자로 치환한다.
str.tolowerCase(); // hello, world! : 대문자로 치환한다.

/***** 문자열 여백 제거하기 *****/
(' ' + str + ' ').trim(); // 'Hello, World!' : 양쪽 여백을 제거한다.
(' ' + str + ' ').trimLeft(); // 'Hello, World! ' : 왼쪽 여백을 제거한다.
(' ' + str + ' ').trimRight(); // ' Hello, World!' : 오른쪽 여백을 제거한다.
```

