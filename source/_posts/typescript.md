---
title: 타입스크립트 기초 (1)
date: 2016-08-11 00:04:59
tags: Java Script, js, Type Script, Tutorial
---

<p>타입스크립트를 배워보려고 쓰는 포스트이다.<br />이번 포스트에서는 타입스크립트 설치, 특징, 컴파일, 타입들에 대해서 알아본다.</p>

__타입스크립트의 이점__
- 정적 타입으로 인해 타입스크립트로 코드를 작성할 때 예측하기 쉬워지며, 디버그하기에도 쉽다.
- 모듈과 네임스페이스, 강력한 OOP 지원으로 크고 복잡한 애플리케이션의 코드 베이스를 체계화하기 쉽다.
- Angular 2 프레임워크가 타입스크립트로 쓰여져있다.
- TypeScript 컴파일러는 TypeScript 문법을 자바스크립트 문법으로 변환하는 과정에서 변수 정의 순서나 이름을 변경하지 않아서, 디버깅에 용이하다.
  - 부가적으로 소스 맵도 제공하므로 TypeScript 소스를 직접 디버깅할 수도 있다.

__설치__
```sh
$ npm install -g typescript

# tsc -v 명령어로 버전을 확인할 수 있다.
$ tsc -v
```

__컴파일__
<p>타입스크립트는 .ts 파일 (또는 .tsx) 로 작성된다.<br />ts 파일은 프라우저에서 바로 사용할 수 없으며, .js 파일로 컴파일되어야 한다.<br />ts 파일을 컴파일 하기 위해 tsc 명령어를 수행하거나, gulp 와 같은 태스크 러너를 사용한다.<br />(본인은 터미널을 사용하므로, 터미널 기준으로 설명을 한다.)</p>
```sh
# 하나의 파일을 컴파일할 때
$ tsc FILE1.ts

# n 개 이상의 파일을 컴파일할 때
$ tsc FILE1.ts FILE2.ts

# 모든 파일을 컴파일할 때
$ tsc *.ts

# 변화를 감지하여 자동으로 컴파일하도록 할 때
$ tsc FILE1.ts --watch
```
<p>빌드 세팅을 하기 위해 tsconfig.json 파일을 생성할 수도 있다. (tsconfig.json 에 대한 글은 [공식 홈페이지](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html)에서 볼 수 있다.)</p>

__정적 타입__
<p>타입스크립트가 다른 compile-to-js 언어와 다른 점은 정적 타입을 지원한다는 것이다.<br />즉, 변수의 타입을 선언할 수 있다는 것이다. 이때, 컴파일러는 변수의 값으로 다른 타입을 할당할 수 없도록 하며, 타입 선언이 빠졌을 경우, 개발자의 코드에서 타입을 추측한다.<br />다음과 같이 예제 코드 example.ts 를 작성한다.</p>

```ts
var greeting: string = 'Hello, Type Script!', // String
  age: number = 27, // Number
  isFunny: boolean = true; // Boolean

// var gretting = 'Hello, World!'; 도 가능하다.

// 이 함수는 string, number, boolean 타입의 인자를 받으며, return 타입이 없다.
function say(greet: string, age: number, condition: boolean): void {
  if (condition) {
    console.log(greeting + ' I\'m ' + age + ' years old.');
  }
}

say(greeting, age, isFunny);
```

```sh
$ tsc example.ts
```

<p>컴파일하면, example.js 가 생성된다.</p>
```js
var greeting = 'Hello, Type Script!', // String
age = 27, // Number
isFunny = true; // Boolean
// var gretting = 'Hello, World!'; 도 가능하다..
// 이 함수는 string, number, boolean 타입의 인자를 받으며, return 타입이 없다..
function say(greet, age, condition) {
  if (condition) {
    console.log(greeting + ' I\'m ' + age + ' years old.');
  }
}
age = ' asdf afawef awe';
say(greeting, age, isFunny);
```

<p>만약, ts 작성 시 다음과 같이 잘못된 변수 타입을 대입할 경우 에러가 발생한다.</p>
```ts
var age: number = '27';
```
```sh
example.ts(3,1): error TS2322: Type 'string' is not assignable to type 'number'.
```

<p>Number, String, Boolean 이외에도 많은 타입들이 있으며, 다음은 타입들에 대한 간략한 설명과 코드이다.</p>
__Boolean__
true / false 를 가지는 타입
```ts
let isDone: boolean = false;
```
__Number__
<p>자바스크립트와 마찬가지로 숫자는 모두 부동소수점으로 표현된다. 타입스크립트는 16진수 리터럴, 10진수 리터럴 외에도 8진수 리터럴과 2진수 리터럴을 지원한다.</p>
```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```
__String__
<p>자바스크립트와 마찬가지로 문자열을 표현하기 위해 \' \' (Single Quotes) 나 \" \" (Double Quotes) 를 감싼다.</p>
```ts
let color: string = "blue";
color = 'red';
```
<p>또한, \` \` (Back-tick) 으로 감싸지는 템플릿 스트링을 사용할 수 있다.<br />템플릿 스트링은 멀티 라인과 embedded expression 을 지원하며, ${ } 로 표현식을 내장시킬 수 있다.</p>
```ts
let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ fullName }.

I'll be ${ age + 1 } years old next month.`
```
__Array__
<p>배열은 두 가지 방법으로 쓸 수 있다.<br />\: 뒤에 변수의 타입으로써, 원소 타입과 [] 를 쓴다.<br />혹은, Array\<원소타입\> 을 쓴다.</p>
```ts
let list: number[] = [1, 2, 3];
/* Or */
let list: Array<number> = [1, 2, 3];
```
__Tuple__
<p>한 쌍의 값을 표현할 수 있다.</p>
```ts
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

__Enum__
<p>열거자 목록이라고 하는 상수 집합으로 구성된 고유 형식인 열거형을 사용할 수 있다. C# 과 같은 스타일로 작성한다.</p>
```ts
enum Color {Red, Green, Blue};
let c: Color = Color.Green;
```
<p>다음은 ts 를 컴파일한 것이다.</p>
```js
var Color;
(function (Color) {
  Color[Color["Red"] = 0] = "Red";
  Color[Color["Green"] = 1] = "Green";
  Color[Color["Blue"] = 2] = "Blue";
})(Color || (Color = {}));
;
var c = Color.Green;
```
<p>기본적으로, enum 은 0 부터 시작을 한다. 만약, 시작되는 값을 1로 바꾸려면, 다음과 같이 코드를 작성한다.</p>
```ts
enum Color {Red = 1, Green, Blue};
let c: Color = Color.Green;
```
<p>모든 값들을 직접 지정하려면, 다음과 같이 코드를 작성한다.</p>
```ts
enum Color {Red = 1, Green = 2, Blue = 4};
let c: Color = Color.Green;
```
<p>만약, 2 에 매핑되어 있는 Color 가 무엇인지 확인하려면 다음과 같이 코드를 작성할 수 있다.</p>
```ts
enum Color {Red = 1, Green, Blue};
let colorName: string = Color[2];

alert(colorName);
```

__Any__
<p>데이터 타입을 미리 알 수 없을 때 사용할 수 있다. Any 를 사용하면, 컴파일할 때, 타입 체크를 하지 않는다.</p>
```ts
let notSure: any = 4;
notSure = 'maybe a string instead';
notSure = false; // okay, definitely a boolean
```
<p>또한, 배열의 원소 타입으로도 사용할 수 있다.</p>
```ts
let list: any[] = [1, true, 'free'];

list[1] = 100;
```

__Void__
<p>void 는 any 의 반대 개념으로 볼 수 있다. 어떤 타입도 가질 수 없을 때 사용할 수 있다.</p>
```ts
function warnUser(): void {
  alert('This is my warning message');
}
```
<p>변수에 void 타입을 사용하면, 해당 변수에는 undefined 나 null 밖에 할당할 수 없다.</p>
```ts
let unusable: void = undefined;
```

__Null and Undefined__
<p>TypeScript 에서는 undefined 와 null 도 타입을 가진다.</p>
```ts
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```

__Type Assertions__
<p>Type assertion 은 다른 언어의 형변환과 비슷하다.<br />하지만, 수행하면서 특별한 체크나 데이터 재구성을 하지 않는다.<br />두 가지 방법으로 Type assertion 을 사용할 수 있다. (하지만, JSX 에서는 as 스타일만 사용 가능하다.)</p>
```ts
let someValue: any = 'this is a string';
let strLength: number = (<string>someValue).length;

/* Or */

let someValue: any = 'this is a string.';
let strLength: number = (someValue as string).length;
```

<p>이 포스트는 [타입스크립트 기초 (2)](/2016/08/12/typescript-2) 로 이어진다.</p>
