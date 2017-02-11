---
layout: single
title: Go Chapter 4. function
categories: blog
---
  
- 내부적으로 서브루틴은 주로 스택으로 구현함.
- Go 언어는 값에 의한 호출(Call by value) 만을 지원한다. 함수 밖의 변수의 값을 변경하려면 해당 값이 들어 있는 주소값을 넘겨받아서 그 주소에 있는 값을 변경하여 참조에 의한 호출 (Call by reference) 와 비슷한 효과를 낼 수 있다.   


## 4.1 값 넘겨주고 넘겨 받기

### 4.1.1 값 넘겨주기   
```go
func ReadFrom(r io.Reader, lines *[]string ) error {
// []string 자료형이 아닌 *[]string 을 사용한 이유는 함수가 lines 변수의 값을 변경하기 위해.
/* []string 자료형이 넘어간다면 슬라이스(배열에 대한 포인터, 길이, 용량) 값이 넘어가며, 세 값을 담고 있던 변수와는 연관성이 없다. */
```

```go
package main

import (
    "fmt"
)


func AddOne(nums []int) {   // 값이 아닌 배열포인터, 길이, 용량이 전달된다.
    for i := range nums {
        nums[i]++           // 포인터가 가리키고 있는 배열을 변경한다.
    }
}

func ExampleAddOne() {
    n := []int{1, 2, 3, 4}  
    AddOne(n)               // 호출 뒤 값의 변경이 일어난다.
    fmt.Println(n)
    // Output:
    // [2 3 4 5]
}

func main() {
    ExampleAddOne()
}
```

### 4.1.2 둘 이상의 반환값   
- 둘 이상의 반환값을 둘 수 있다.   

```go
// 에러값 하나 반환.
func WriteTo(w io.Writer, lines []string) error {}

// 에러값 두개 반환. 괄호로 자료형을 둘러 싸야 한다.    
func WriteTo(w io.Writer, lines []string) (int64, error) {
    return n, err // 반환값
}

 // 에러값만 받음.
_, err = WriteTo(w, lines)
```

### 4.1.3 에러값 주고받기   
- Go의 관례상 에러는 가장 마지막 값으로 반환한다.
- 패닉 이라는 개념이 있어서 다른언어의 exception 과 비슷한 역할을 한다. 다만 특수한 상황에 쓰기 때문에 일반적으로 error 값을 돌려받아 이용한다.   

```go
// error 값을 받아서 처리할 경우 반복적인 코드양이 많아질 수 있다.
// 다음과 같이 if 문을 이용할 수 있다.
if err := Myfunc(); err != nill {
    ...
}
```

- 예외가 발생한 곳과 처리할 수 있는 곳이 다른 경우가 많다.
- Go 의 방식으로는 예외를 현재 문맥에서 처리할 수 없는 경우 해당 에러를 그대로 반환할 수 있다.   

```go
if err := Myfunc(); err != nill {
    return nil, err
}
```

- 새로운 에러를 생성해야 하는 경우에는 가장 간단한 방법으로 __erros.New__, __fmt.Errorf__ 를 이용할 수 있다.  

```go
return errors.New("message")
return fmt.Errorf("message number: %d", count)
```

### 4.1.4 명명된 결과 인자   
- Go 에서는 돌려주는 값들을 넘겨받는 인자와 같은 형태로 쓸 수 있다.   
- 넘겨받는 인자들은 넘어온 값들로 초기값이 설정되는데, 돌려주는 인자들은 기본값으로 초기화 된다.   
- 명명된 결과 인자를 사용해서 이득을 보지 않는 경우 되도록 사용하지 않는 편이 좋다.   

```go
func WriteTo(w io.Writer, linkes []string) (n int64, err error) {
    for _, line := range lines {
        var nw int
        nw, err := fmt.Fprintln(w, line)
        n += int64(nw)
        if err != nill {
            return
        }
    }
    return
}
```

### 4.1.5 가변인자   
- 넘겨받을 수 있는 인자의 개수가 정해져 있지 않는 함수를 만들려면 슬라이스에 담아 넘길 수 있다.   

```go
func f(w io.Wirter, nums []int) {
    ...
}

func main() {
    ...
    f(w, []int{x, y, z})
}
```

- 슬라이스로 갖고 있는 자료를 가변인자를 두고 있는 함수로 넘길 수 있다. 호출시에 점 셋을 붇이면 슬라이스를 넘길 수 있다.   

```go
lines := []string{"hello", "word", "Go language"}
WriteTo(w, lines...)
```

## 4.2 값으로 취급되는 함수
- Go 언어에서 함수는 일급시민(First-class citizen)으로 분류된다.
- 함수 역시 값으로 변수에 담길 수 있고, 다른 함수로 넘기거나 돌려받을 수 있다.

### 4.2.1 함수 리터럴
```go
// 함수이름, 함수의 자료형, 코드
func add(a, b int) int{
    return a + b
}

// 함수의 값만 표현, 함수 리터럴(Function literal), dlraudgkatn
func (a, b int) int{
    return a + b
}
```

```go
func printHello() {
    fmt.Println("Hello!")
}


func Example_funcLiteral() {
    // 함수 리터럴 시작, 변수에 담지 않고 함수 리터럴 호출
    func() {
        fmt.Println("Hello!")
    }()
}

func Example_funcLiteralVar() {
    // 변수에 담아 호출 
    printHello := func() {
        fmt.Println("Hello!")
    }
    printHello()
}
```

### 4.2.2 고계 함수(higher-order function)
- 함수를 넘기고 받음, 함수를 넘기고 받는 함수이기 때문에 더 고차원적인 함수.   

```go
func ReadFrom(r io.Reader, f func(line strung)) error {
    scanner := bufio.NewScanner(r)
    for scanner.Scan() {
        f(scanner.Test())
    }
    if err := scanner.Err(); err !=nil {
        return err
    }
    return nil
}

// 두 번째 인자에 함수 리터럴을 넣어서 호출함
func ExampleReadFrom_Print() {
    r := strings.NewReader("bill\ntom\njane\n")
    err := ReadFrom(r, func(line string) {
        fmt.Println("(", line, ")")
    })
    if err != nil {
        fmt.Println(err)
    }
    // Output:
    // ( bill )
    // ( tom )
    // ( jane )
}
```


### 4.2.3 클로저
- 외부에서 선언한 변수를 함수 리터럴 내에서 마음대로 접근할 수 있는 코드를 의미한다.

```go
func ExampleReadFrom_append() {
    r := strings.NewReader("bill\ntom\njane\n")
    var lines []string
    err := ReadFrom(r, func(line string) {
        lines = append(lines, line)
    })
    if err != nil {
        fmt.Println(err)
    }
    fmt.Println(lines)
    // Output:
    // [ bill tom jane ]
}
```
### 4.2.4 생성기

### 4.2.5 명명된 자료형

### 4.2.6 명명된 함수형

### 4.2.7 인자 고정

### 4.2.8 패턴의 추상화

### 4.2.9 자료구조에 담은 함수


## 4.3 메서드

### 4.3.1 단순 자료형 메서드

### 4.3.2 문자열 다중 집합

### 4.3.3 포인터 리시버

### 4.3.4 공개 및 비공개


## 4.4 활용

### 4.4.1 타이머 활용하기

### 4.4.2 path/file 패키지

## 4.5 마치며
