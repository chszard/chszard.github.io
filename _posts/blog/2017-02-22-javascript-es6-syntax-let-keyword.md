---
title: javascript es6 syntax 정리 let keyword
layout: single
tags: [javascript, es6, study]
comments: true
---

## let keyword
: 블록스코프 변수를 선언하는 키워드, 선언과 동시에 값을 할당할 수 있다.

### 함수 스코프 변수 선언
: var 키워드로 선언한 변수를 함수 스코프 변수(function scoped variable)라고 하며, 함수 밖에 선언한 함수 스코프는 전역범위에서 참조할 수 있다.

- 예제

```javascript
var global_value = 20;

function functionScope() {

    console.log(global_value);

    var local_value = 10;

    console.log(local_value);

    if (true) {
        var local_value2 = 15;
        console.log(local_value2);
    }

    console.log(local_value2);
}

functionScope();
```

----------
    결과:
    20
    10
    15
    15 //undefined 가 아님.


### 블록 스코프 변수 선언
: let 키워드로 선언한 변수. 함수 밖에 선언하면 전역 접근 가능하다. 블록 안에 선언하면 자신과 같은 레벨, 혹은 하위 레벨에서만 접근 가능하며, 블록 밖에선 접근 할 수 없다.

- 예제

```javascript
let global_value = 20;

function blockScope() {

    console.log(global_value);

    let local_value = 10;

    console.log(local_value);

    if (true) {
        let local_value2 = 15;
        console.log(local_value2);
    }

    console.log(local_value2);
}

blockScope();
```

----------
    결과:
    20
    10
    15
    ReferenceError: Can't find variable: local_value2


### 변수 재선언
: 이미 var로 선언한 변수를 다시 var 로 선언하면 덮어쓴다.

- 예제

```javascript
var a = 0;
var a = 1;
console(a);

function testFunction(){
    var b = 2;
    bar b = 5;
}
testFunction()
```

----------
    결과:
    1
    5


: let의 경우 TypeError 예외가 발생한다.

- 예제

```javascript
let a = 0;
let a = 1; // Uncaught SyntaxError: Identifier 'a' has already been declared
```


: 함수 안에서 접근 가능한 변수명과 동일한 이름을 가진 변수를 선언하면, 사용한 키워드에 따라 가리키는 대상이 달라진다.

- 예제

```javascript
var a = 1;
let b = 2;
console.log(a);
console.log(b);

function testFunction(){
    var a = 3;
    let b = 4;
    
    console.log(b);

    if (true) {
        var a = 5;
        let b = 6;
        console.log(a);
        console.log(b);
    }
}

testFunction();
```

----------
    결과:
    1
    2
    4
    5
    6


### 결론
: ES6 지원 브라우저일 경우 let 을 쓰는 편이 모호한 스코프를 한정하여 버그를 줄이는데 도움을 줄 수 있으나, 하위 호환성 을 위해 var를 상황에 맞게 사용하자.


[reference: Lerning ECMAScript6](https://www.packtpub.com/web-development/learning-ecmascript-6)