---
title: 타입 스크립트 스터디 1주차
date: 2019-02-08 13:53:49
tags:
- typescript
- study
categories:
- typescript
---

# 타입 스크립트 스터디 1주차

## 환경 구축

https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html 

## Basic Types

### boolean

* true/false 값을 가지는 가장 기본적인 타입

```typescript
let isDone : boolean = false;
```

### number

* 자바스크립트나 typescript 둘 다 floating point values
* 10, 16, 8, 2 진수 전부 지원
* target을 ES5로 할 경우 8,2진수는 ES5에서 지원안되기 때문에 10진수 전환
* 8,2진수의 경우 ECMAscript 2015+ 부터 지원

```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b10;
let octal: number = 0o71;
```

### string

* ", '를 사용하여 표시
* 텍스트 데이터를 참조하기 위해서 string 타입을 사용

```typescript
let color: string = 'blue';
color = 'red';
```

* ``를 활용해서 template strings 를 사용할 수 있다(ES2015+ 에서 지원)

```typescript
let fullName: string = 'Dongwoo Seo';
let age: number= 38;
let sentence : string = `Hello, my name is ${fullName}.

I'll be ${age + 1} years old next year.
`;
```

### array

* javascript 와 같이 배열을 지원
* 배열을 구성하는 방법으로 2가지를 지원

```typescript
//첫 번째 방법
let list1: number[] = [1,2,3,4,5];

//두 번째 방법
let list2: Array<number> = [1,2,3,4,5];
```

### tuple

* 배열 중에서 각 element 의 type을 정하고 싶을 때 사용
* element의 타입이 굳이 같을 필요는 없음.

```typescript
let x: [string, number];

x = ['hello', 10];
x = [10, 'hello']; //Error tuple 설정이 달라서
x = ['hello', 10, 1, 3, 'efwefwefwf']; //Error 2개만 설정했는데 2개 이상아리서 에러

console.log(x[0].substr(1));
console.log(x[1].substr(1));        // Error 숫자에 없는 메소드

x[3] = "world"; // OK, 'string' can be assigned to 'string | number' --> 실제로는 에러?
console.log(x[5].toString()); // OK, 'string' and 'number' both have 'toString' --> 실제로는 에러?
x[6] = true; // Error, 'boolean' isn't 'string | number' 

```

### enum

* c# 언어와 비슷하게 숫자 대신 문자로 구성

```typescript
enum Color {Red, Green, Blue};
let c: Color = Color.Red;
```

* 기본적으로 0부터 시작하지만 아래와 같이 변경 가능

```typescript
enum Color {Red = 1, Green, Blue};  //1부터 시작
enum Color {Red = 1, Green = 2, Blue = 4};  //각 값을 수동으로 지정도 가능
```

* 숫자를 프로퍼티로 할 경우 Text값을 반환

```typescript
enum Color {Red = 1, Green, Blue};  //1부터 시작

console.log(Color.Red); //1
console.log(Color[Color.Red]); //Red
```

### any

* application이 동작하기 전 까지 타입을 선언하지 못 하는 경우가 발생할 수 있음.
* 그렇게 때문에 dynamic type이 필요함. 이럴 경우 `any` 타입을 사용
* `any` 타입을 선언할 경우 컴파일 시 type check를 하지 않음

```typescript
let notSure: any = 4;

notSure = 'sdqwwdqwdqwdwqd';
notSure = false;

```

* `any`와 비슷하게 `Object` 를 사용할 수도 있을 것 같지만 `Object` 로 가져올 경우 `Object` 에만 있는 메소드만 사용이 가능

```typescript

let notSure:any = 4;
notSure.toFixed();      

let notSure2:Object = 4;
notSure2.toFixed();     //Error, Object에는 toFixed라는 메소드가 없음
//toFixed https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed
```
* 배열을 표시할 때 각각이 다른 타입일 경우에도 사용 가능

```typescript
let list: any[] = [1, true, 'sdqwdqwdqwdqwdqwd'];

console.log(list[0]); //1
list[1] = 3231232213;
```

### void

* return 값이 없는 경우에 사용

```typescript
function warnUser(): void {
    console.log("This is my warning message");
}
```

* void type은 함수에서만 유용하다. (변수 선언 시에는 null 또는 undefined 만 가능)

### null 과 undefined

* 타입이름으로 가지고 있지만 그것 스스로는 쓸일이 없음
* 기본적으로 null, undefined의 경우 다른 타입의 서브 타입으로 쓰인다.(number type에 값이 없는 경우 undefied 사용 등등)
* `--strictNullChecks` flag 를 사용할 경우 `null`, `undefined`은 명시적으로 선언된 곳이나 `void`에서만 사용가능
* 위에 같이 설정할 경우 `null`, `undefined` 로 발생할 수 있는 버그를 줄일 수 있기 때문에 설정하는 것을 추천

### never

* 절대로 발생하지 않는 값의 타입?
* 예로 never는 항상 에러가 발생하거나 리턴이 될 수 없는 함수
* 변수의 경우는 true값이 나올 수 없는 변수
* never는 모든 타입의 서브 타입이며 할당이 가능
* 하지만 다른 모든 타입은 never를 할당 받을 수 없다.

```typescript
// Function returning never must have unreachable end point
function error(message: string): never {
    throw new Error(message);
}

// Inferred return type is never
function fail() {
    return error("Something failed");
}

// Function returning never must have unreachable end point
function infiniteLoop(): never {
    while (true) {
    }
}

const a:never = 100;
```

### Object

* Object는 non-primitive type

```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

### Type assertions

* 다른 언어에서는 type casting과 비슷
* 다른 언어와 다른 점은 컴파일러에서 type casting에 맞게 변경해주는 것이 아니고 개발자가 이건 내가 알고 있으니 이렇게 처리해 하는 방식으로 컴파일러에게 알려주는 것
* 그래서 컴파일 시에 잘못된 값이더라도 에러가 발생하지 않음

```typescript

// 첫 번째 angle-bracket을 사용하는 방법
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

// 두 번째 as를 사용하는 방법
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

### let

* var 대신 let을 사용하자!
* 왜?
* http://hacks.mozilla.or.kr/2016/03/es6-in-depth-let-and-const/


## Variable Declaration

### Variable Declarations

* ECMAscript 2015+부터 `let` 과 `const`를 이용해서 변수를 선언하는 방법이 새로 소개가 되었음
* `let` 은 몇몇 개념 상으로 `var` 와 비슷하지만 몇몇 단점을 보완해서 나온 새로운 개념
* `const`와 `let`의 차이는 재할당 가능 여부.`const`는 재할당이 불가능하지만, `let`은 재할당이 가능하다.
* typescript에서도 `const`와 `let` 을 지원
* 여기서는 우리가 `var`를 왜 쓰지 않아야 하는지에 대해서 좀 더 자세하게 설명


### var

* ECMAscript 5까지 사용했던 변수 선언 방법
```typescript
var a = 10;
function f() {
    var a= 10;
    return function g() {
        var b = a + 1;
        return b;
    }
}

var g = f();

g();        //결과 값은 11

function f2() {
    var a = 1;

    a = 2;

    var b = g();
    a = 3;

    return b;

    function g() {
        return a;
    }
}

f2();       //결과 값은 2
```

#### scope rules

* `var`는 다른 언어들과 다른 특이한 scope를 가지고 있다(function scoping)

```typescript
function f(shouldInitialize: boolean) {
    if (shouldInitialize) {
        var x = 10;
    }

    return x;
}

f(true); // 10
f(false); // undefined
```

* 이러한 scoping은 몇가지 문제를 야기 시킬 수 있다.
    * 동일한 변수를 여러 번 선언하더라도 에러가 아님
    * 변수 캡쳐 상의 문제
    ```typescript
    function sumMatrix(matrix: number[][]) {
        var sum = 0;
        for (var i = 0; i < matrix.length; i++) {
            var currentRow = matrix[i];
            for (var i = 0; i < currentRow.length; i++) {
                sum += currentRow[i];
            }
        }

        return sum;
    }

    for (var i = 0; i < 10; i++) {
        setTimeout(function() { console.log(i); }, 100 * i);
    }

    //결과는 전부 10

    for (var i = 0; i < 10; i++) {
        (function(i) {
            setTimeout(function() { console.log(i); }, 100 * i);
        })(i);
    }

    // 이런식으로 해야 1,2,3,4,5,6,...
    ```

### let

* var 선언 시 위와 같은 문제가 있을 수 있어서 scoping 전환이 필요

```typescript
let hello = 'hello!!!'; //사용하는 방법은 비슷
```

#### block-scoping

* 일반적인 프로그램과 같은 block-scoping 을 가진다.

```typescript
function f(input: boolean) {
    let a = 100;

    if (input) {
        let b = a + 1;
        return b;
    }

    return b;   //Error
}
```

* let은 var와 다르게 hoisting 이 발생하지 않는다.

```typescript
a++;    //error (temporal dead zone 이라고 함)
let a;
```

#### Re-declarations and Shadowing

* var의 경우는 여러 번 다시 선언해도 에러 없이 진행 이런 경우 잘못하면 버그를 발생할 여지가 많음.
* let의 경우는 한번 이상 제 선언할 경우 에러 발생
* Shadowing 은 중첩된 스코프에서 기존의 변수 이름을 사용하는 것
```typescript
function f(condition, x) {
    if (condition) {
        let x = 100;
        return x;
    }

    return x;
}

function sumMatrix(matrix: number[][]) {
    let sum = 0;
    for (let i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];
        for (let i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
        }
    }

    return sum;
}
```
* 하지만 명확한 코드를 위해서 하지 않는 것이 좋다.

#### block-scoped variable capturing

* `let`, `const`의 경우는 block scoping 을 가지기 때문에 block 생성 시에 새로운 scope를 가진다.
* `var` 같은 경우는 function scoping 이기 때문에 function 선언 시에 새로운 scope를 가지기 때문에 위에 발생한 문제를 해결하기 위해서 IIFE를 활용했다.
* `let`, `const`는 IIFE(Immediately Invoked Function Expression[[링크]](https://developer.mozilla.org/ko/docs/Glossary/IIFE))를 활용하지 않더라고 아래와 같이 각 변수를 capturing 해서 사용할 수 있다.

```typescript
function theCityThatAlwaysSleeps() {
    let getCity;

    if (true) {
        let city = "Seattle";
        getCity = function() {
            return city;
        }
    }

    return getCity();
}

for (let i = 0; i < 10 ; i++) {
    setTimeout(function() { console.log(i); }, 100 * i);
}
```

### const

* `let`과 거의 비슷하니 재활당이 되지 않는다.

```typescript
const numLivesForCat = 9;
```

* immutable과 햇깔릴 수 있으나 재할당이 안되는 것이지 object의 property 값을 추가하거나 변경하는 것은 가능하다.

```typescript
const numLivesForCat = 9;
const kitty = {
    name: "Aurora",
    numLives: numLivesForCat,
}

// Error
kitty = {
    name: "Danielle",
    numLives: numLivesForCat
};

// all "okay"
kitty.name = "Rory";
kitty.name = "Kitty";
kitty.name = "Cat";
kitty.numLives--;
```

* 타입 스크립트에서는 특정 프로퍼티를 readonly로 설정할 수 있는데, 이것은 다음 시간에..

### let vs const

* 될 수 있으면 `const` 를 활용
* 재할당이 필요한 경우에만 `let`을 활용하자.
* 변수 선언 시에 이게 재할당이 필요한지 아닌지 한번 고민해보는 습관을 가져보자

### Destructuring

* 지금 특징들은 ECMAscript 2015+ 이후에 사용가능한 것들
```typescript

let input = [1,2,3,4,5];

let [a,b] = input;        //a=1, b=2;
[a,b] = [b,a];              //변수 교환;
let [c,...etc] = input;     //c=1, etc = [2,3,4,5]
let [,d,e,,f] = input;      //d=2, e=3, f=5

let o = {g:'foo', h:12, i:'bar'};

let {g,h} = o;              //g='foo', h=12
let {g, ...etc2} = o;       //g='foo', etc2={h:12, i:'bar'}
let {g:g1, h:h1} = o;       //g1='foo', h1=12
let {g, k=100} = o;         //g='foo', k=100 (기본값 설정)

type C = { a: string, b?: number }
function f({ a, b }: C): void {
    // ...
}

function f({ a, b } = { a: "", b: 0 }): void {
    // ...
}
f(); // 좋아요, 기본값 { a: "", b: 0 }

function f({ a, b = 0 } = { a: "" }): void {
    // ...
}
f({ a: "yes" }); // 좋아요, 기본값 b = 0
f(); // 좋아요, 기본값은 { a:"" }이며 이 경우 기본값은 b = 0입니다.
f({}); // 오류, 인수를 제공하려면 'a'가 필요합니다.
```

### Spread

* 동일한 이름의 property인 경우 가장 오른 쪽 것으로 할당됨
* 열거 가능한 property 값만 spread 가능(method는 불가)
* 타입 스크립트 컴파일러의 경우 일반 함수의 매개변수를  spread 로 허용하지 않음(지원하는 것 같은데.. 어떤 경우인지 확인 필요)

```typescript
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];

let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { ...defaults, food: "rich" };     //food: 'rich'

let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { food: "rich", ...defaults };     //food: 'spicy'

class C {
  p = 12;
  m() {
  }
}

let c = new C();
let clone = { ...c };
clone.p; // 12
clone.m(); // 오류!



```












## 소스

https://github.com/otawang/typescript-study-source/blob/master/src/chapter1/chapter1.ts


## 참고자료

https://poiemaweb.com/es6-block-scope
https://www.typescriptlang.org/docs/handbook/variable-declarations.html
https://www.typescriptlang.org/docs/handbook/basic-types.html

https://typescript-kr.github.io/pages/Variable%20Declarations.html
https://typescript-kr.github.io/pages/Basic%20Types.html
