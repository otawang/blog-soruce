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

## 소스

https://github.com/otawang/typescript-study-source/blob/master/src/chapter1/chapter1.ts
