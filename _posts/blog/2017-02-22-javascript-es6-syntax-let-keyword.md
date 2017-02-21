---
title: javascript es6 syntax 정리 let keyword
layout: single
tags: [javascript, es6, study]
comments: true
---

## 1. let keyword
: 블록스코프 변수를 선언하는 키워드, 선언과 동시에 값을 할당할 수 있다.

### 함수 스코프 변수 선언
: var 키워드로 선언한 변수를 함수 스코프 변수(function scoped variable)라고 하며, 함수 밖에 선언한 함수 스코프는 전역범위에서 참조할 수 있다.

- 예제
{% highlight javascript linenos %}
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
{% endhighlight %}

----------
    결과:
    20
    10
    15
    15 //undefined 가 아님.


### 블록 스코프 변수 선언
: let 키워드로 선언한 변수. 함수 밖에 선언하면 전역 접근 가능하다. 블록 안에 선언하면 자신과 같은 레벨, 혹은 하위 레벨에서만 접근 가능하며, 블록 밖에선 접근 할 수 없다.

- 예제
{% highlight javascript linenos %}
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
{% endhighlight %}

----------
    결과:
    20
    10
    15
    ReferenceError: Can't find variable: local_value2


