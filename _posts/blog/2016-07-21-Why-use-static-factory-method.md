---
layout: single
title: Why use static factory method?
excerpt: "Why use static factory method?"
categories: blog
tags: [java, effective-java]
comments: true
share: true
modified: 2016-07-21T01:10:50
---
```
Rule 1. 생성자 대신 정적 팩터리 메서드를 사용할 수 없는지 생각해보자.
```

### 장점

#### 1. 생성자와는 달리 정적 팩토리 메서드에는 이름이 있다.  

#### 2. 생성자와는 달리 호출할 때마다 새로운 객체를 생성할 필요는 없다.  

#### 3. 생성자와는 달리 반환값 자료형의 하위 자료형 객체를 반환할 수 있다는 점.  

#### 4. 형인자 자료형(parameterized type) 객체를 만들 때 편하다.  


### 단점

#### 1. public 이나 protected로 선언된 생성자가 없으므로 하위 클래스를 만들 수 없다.

#### 2. 정적 펙토리 메서드가 다른 정적 메서드와 확연히 구분이 되지 않는다.

##### 2.1 정적 메소드 이름을 특정한 컨벤션에 맞춰 작성한다.
   
    * valueOf
    * of
    * getInstance
    * newInstance
    * getType
    * newType


### Summary
- static factory method 는 public constructor 와 용도가 서로 다르며 차이와 장단점을 이용하는 것이 중요하다.

출처: 이펙티브 자바