---
title: 타입스크립트 기초 (2)
date: 2016-08-12 00:05:18
tags: Java Script, js, Type Script, Tutorial
---

<p>이 포스트는 [타입스크립트 기초 (1)](/2016/08/11/typescript/) 에서 이어지는 포스트이다.</p>

__인터페이스__
<p>함수에 전달될 파라미터들의 조합에 이름을 지을 수 있다.<br />자바스크립트로 번역되면, 인터페이스는 사라진다.</p>
```ts
/* Food 인터페이스는 name(string) 과 calories(number) 로 구성되어 있다. */
interface Food {
  name: string;
  calories: number;
}

/* speack 함수는 Food 인터페이스와 같은 타입의 객체를 받는다. */
function speak(food: Food): void{
  console.log('Our ' + food.name + ' has ' + food.calories + ' calories.');
}

/* Food 인터페이스에서 사용된 프로퍼티가 모두 있어야 한다. 이 때, 타입은 컴파일러가 추측한다. */
var ice_cream = {
  name: 'ice cream',
  calories: 200
}

speak(ice_cream);
```
<p>인터페이스에서 선언된 프로퍼티는 꼭 존재해야 하며, 선언된 타입과 일치해야 한다. 그렇지 않으면 컴파일러는 에러가 발생한다.</p>
```ts
interface Food {
  name: string;
  calories: number;
}

function speak(food: Food): void{
  console.log('Our ' + food.name + ' has ' + food.calories + ' grams.');
}


var ice_cream = {
  nmae: 'ice cream', // 이 곳에서 에러가 발생한다.
  calories: 200
}

speak(ice_cream);
```
```sh
main.ts(16,7): error TS2345: Argument of type '{ nmae: string; calories: number; }
is not assignable to parameter of type 'Food'.
Property 'name' is missing in type '{ nmae: string; calories: number; }'.
```
<p>인터페이스에 대한 자세한 설명은 TypeScript 공식 홈페이지의 [문서](http://www.typescriptlang.org/docs/handbook/interfaces.html)에서 볼 수 있다.</p>

__클래스__
<p>타입스크립트는 클래스 시스템을 제공하며, Java 나 C# 과 유사한 시스템이다.<br />'상속'과 '추상 클래스', '인터페이스 구현', 'getter/setter' 등이 포함되어 있다.</p>
<p>ES6 에서는 클래스가 구현되어 타입스크립트가 아닌 순수 JS 에서도 클래스 구현이 가능하다. 이 두 클래스 구현체는 비슷하지만, 타입스크립트가 더 엄격하게 구현되어 있다.</p>
```ts
class Menu {
  /* 프로퍼티들은 기본적으로 public 이며, private 나 protected 가 될 수도 있다. */
  items: Array<string>;
  pages: number;

  /* 올바른 constructor 가 와야 한다. */
  constructor(item_list: Array<string>, total_pages: number) {
    this.items = item_list;
    this.pages = total_pages;
  }

  list(): void {
    console.log('Our menu for today:');
    for(var i=0; i<this.items.length; i++) {
      console.log(this.items[i]);
    }
  }
}

var sundayMenu = new Menu(['pancakes', 'waffles', 'orange juice'], 1);

sundayMenu.list();
/*
Our menu for today:
pancakes
waffles
orange juice
*/

class HappyMeal extends Menu {
  /* 프로퍼티는 상속된다. */

  /* 새 constructor 가 정의되어야 한다. */
  constructor(item_list: Array<string>, total_pages: number) {
    /* 이 예제에서는 부모 클레스와 같은 생성자를 사용하기 위해 super 를 사용했다. */
    super(item_list, total_pages);
  }

  /* 기본적으로 메소드 또한 프로퍼티와 마찬가지로 상속되지만, */
  /* 이 예제에서는 메소드를 오버라이드한다.*/
  list(): void{
    console.log('Our special menu for children:');
    for(var i=0; i<this.items.length; i++) {
      console.log(this.items[i]);
    }
  }
}

var menu_for_children = new HappyMeal(['candy', 'drink', 'toy'], 1);

menu_for_children.list();
/*
Our special menu for children:
candy
drink
toy
*/
```
<p>클래스에 대한 자세한 설명은 TypeScript 공식 홈페이지의 [문서](http://www.typescriptlang.org/docs/handbook/classes.html)에서 볼 수 있다.</p>

__제네릭__
<p>제네릭을 사용하면, 데이터 타입에 상관없이 재사용 가능한 코드를 만들 수 있게 된다.</p>
```ts
/* 제네릭 함수에는 함수 이름에 <T> 가 따라온다. */
/* 함수를 호출할 때, 모든 T 인스턴스는 실제 제공되는 타입으로 변환된다. */
function genericFunc<T>(argument: T): T[] {
  var arrayOfT: T[] = [];
  arrayOfT.push(argument);
  return arrayOfT;
}

var arrayFromString = genericFunc<string>('beep');
console.log(arrayFromString[0]); // 'beep'
console.log(typeof arrayFromString[0]); // string

var arrayFromNumber = genericFunc(42);
console.log(arrayFromNumber[0]); // 42
console.log(typeof arrayFromNumber[0]); // number
```
<p>제네릭에 대한 자세한 설명은 TypeScript 공식 홈페이지의 [문서](http://www.typescriptlang.org/docs/handbook/generics.html)에서 볼 수 있다.</p>

__모듈__
<p>큰 앱을 개발함에 있어서 모듈화는 중요한 개념 중 하나이며, 모듈화를 통해 다음과 같은 이점을 얻을 수 있다.</p>
- 전역 스코프 분리
- 캡슐화
- 재사용성
- 의존성 관리
```ts
/* exporter.ts */
var sayHi = function(): void {
    console.log("Hello!");
}

export = sayHi;
```
```ts
/* importer.ts */
import sayHi = require('./exporter');
sayHi();
```
<p>다음은 두 ts 파일을 컴파일 하는 명령어이다. tsc 명령어에 추가적으로 변수가 들어가야 하는데 amd 는 require.js 형식으로 모듈을 빌드하기 위한 값이다.</p>
```sh
$ tsc --module amd *.ts
```
<p>모듈에 대한 자세한 설명은 TypeScript 공식 홈페이지의 [문서](http://www.typescriptlang.org/docs/handbook/modules.html)에서 볼 수 있다.</p>

__앰비언트__
<p>TypeScript 컴파일러는 기본적으로 정의되지 않은 변수 접근 시 에러를 발생시킨다.<br />document, window 처럼 브라우저에서 미리 정의된 객체나 jQuery 와 같은 외부 라이브러리를 사용하려면, 앰비언트를 선언해야 한다.<br />이 때, 타입은 명시하지 않으며, 컴파일러는 앰비언트로 선언된 변수를 'any' 타입으로 추정한다.<br />다음 예제는 document 와 jQuery 를 사용하기 위해 앰비언트 선언을 하는 코드이다.</p>
```ts
declare var document;
document.title = 'Hello, TypeScript!';
/**
 * TypeScript 컴파일러는 'lib.d.ts' 라이버러리를 포함하고 있는데,
 * 이 라이브러리에는 DOM 과 같은 내장 자바스크립트 라이브러리에 대한 선언이 들어있다.
 * (따로 document 에 대한 앰비언트를 선언하지 않아도 된다.)
 */
```
```ts
declare var $;
```

