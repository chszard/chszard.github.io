---
layout: single
title: Go Chapter 3. 문자열 및 자료구조
categories: blog
---

## 3.1 문자열  

### 3.1.1 유니코드 처리  

- Go 언어의 소스는 UTF-8* 로 되어있음. (유니코드 인코딩의 한 방식으로, 한 유니코드 문자를 표현할 떄 필요한 바이트 수가 가변적)  

```go
    for i, r := range "가나다" {
        fmt.Println(i, r)
    }
    fmt.Println(len("가나다"))  
    /* 결과
    0 44032
    3 45208
    6 45796
    */
```

### 3.1.2 Example 테스트
- 예시

```go
    package hangul
    var {
        start = rune(44032) // "가"의 유니코드 포인트
        end = rune(55204)   // "힣" 다음 글자의 유니코드 포인트
    }
    func HasConsonantSuffix(s string) bool {
        numEnds := 28
        result := false
        for _, r := range s {
            if start <= r && r < end {
                index := int(r-start)       // 몇 번째 한글인지 계산
                result = index%numEnds != 0 // 
            }
        }
    }
```


```go
    package hangul
    import "fmt"
    func ExampleHasConsonantSuffix() {
        fmt.Println(HasConsonantSuffix("Go 언어"))
        fmt.Println(HasConsonantSuffix("Go 언어 입니다"))
        fmt.Println(HasConsonantSuffix("Go 언어네요"))
        // Output:
        // I want somthing
        // Output comment 위의 내용에서 기대되는 값이면 true, 아니면 false
    }
    // go test {실행파일명}
```


### 3.1.3 바이트 단위 처리
- 바이트 단위로 동작할 수도 있다.  

```go
    func Example_printBytes(){
        s := "가나다"
        for i := 0; i < len(s); i++ {
            fmt.Printf("%x:", s[i])
        }
        fmt.Println()
        // Output:
        // {원하는 테스트 결과}
    }
```
- 문자열을 바이트 단위의 슬라이스로 변환할 수도 있다. Go 언어의 byte형은 1바이트 정수형. 
- runed은 int32의 별칭, byte는 unint8 의 별칭.
- 형변환은 []byte(s) 로 문자열을 byte 슬라이스로 형변환 가능하다.
- 8비트 정수 슬라이스에서 문자열로 변환시엔 string(b)
- 어떤 문자들이 들어있는지를 중시한다면 string, 실제 바이트 표현이 중요시되면 []byte 권장 

### 3.1.4 패키지 문서
- [외부 표준 라이브러리 경로]: https://golang.org/pkg  
- 오픈소스 코드들의 주석은 자동으로 수집되어 http://godoc.org 에 나타남.  

### 3.1.5 문자열 잇기
- 문자열을 잇는 것은 두 문자열을 이어붙인 문자열을 새로 만드는 것.
- + 연산을 하면 된다.
- Go의 문자열은 문자열에 대한 포인터와 비슷하다.  
```go
    func strCat() {
        s := "abc"
        ps := &s
        s += "def"
        fmt.Println(s)
        fmt.Println(*ps)
        // Output:
        // abcdef
        // abcdef
    }
```
- Sprint 계열 함수나 string.join 등을 사용해도 가능하다.  

### 3.1.6 문자열을 숫자로
- "5" 라는 문자열을 int("5")로 변환하게 되면 형변환이 일어나는 것이 아니라 유니코드의 코드 포인트 숫자로 변환된다.
- 문자열을 숫자로 변환: strconv.Atoi(), strconv.ParseInt, strconv.ParsFloat 등
- 숫자를 문자열로 변환: strconv.Itoa(), strconv.FormatInt() 등
- 다른 방법은 fmt 패키지의 함수들을 이용하면 된다.
```go
    var num int
    fmt.Sscanf("57", "%d", &num)

    var s string
    s = fmt.Sprint(3.14)
    s = fmt.Sprintf("%x", 13402077)
```
 

## 3.2 배열과 슬라이스
- 배열과 슬라이스 모두 연속된 메모리 공간을 순차적으로 이용하는 자료구조.
- 주로 슬라이스를 이용하여 간접적으로 배열을 이용하는 경우가 많다.

### 3.2.1 배열
- 연속된 메모리 공간을 순차적으로 이용하는 자료구조  
```go
    package array

    import "fmt"

    func Example() {
            fruits := [3]string{"사과", "바나나", "토마토"}
            for _, fruit := range fruits {
                    fmt.Printf("%s는 맛있다.\n", fruit)
            }
            // Output:
            // 사과는 맛있다.
            // 바나나는 맛있다.
            // 토마토는 맛있다.
    }
```
- 이렇게 가변적으로 넣을수도 있다. [...]string{"사과", "바나나", "토마토"}

### 3.2.2 슬라이스
- 길이와 용량을 갖고 있고 길이가 변할 수 있는 구조.
- 빈 슬라이스에는 nil 값이 들어감.
- n개의 빈 슬라이스는 slices := make([]string, n) 으로 만들 수 있으며 해당 자료형의 기본값이 들어간다.(문자열은 "", 정수형은 0).
- 슬라이스를 잘라낼 경우 예시  
```go
    package slice

    import "fmt"

    func Example_slicing() {
            nums := []int{1, 2, 3, 4, 5}
            fmt.Println(nums)
            fmt.Println(nums[1:3])
            fmt.Println(nums[2:])
            fmt.Println(nums[:3])
            // Output:
            // [1 2 3 4 5]
            // [2 3]
            // [3 4 5]
            // [1 2 3]
    }    
```
- 파이썬의 슬라이스와 다른점은 인덱스에 음수를 사용하지 못한다. 
  대신 slices[:len(fruits) -1] 로 사용이 가능하다.
- 범위가 넘어갈 경우 에러가 발생한다.

### 3.2.3 슬라이스 덧붙이기
- 슬라이스는 덧붙일 수 있으며, 각각의 값을 가변인자 늘어놓듯이 늘어놓을 수 있다.
```go
    func Example_append() {
        f1 := []string{"사과", "바나나", "토마토"}
        f2 := []string{"포도", "딸기"}
        f3 := append(f1, f2...)     // 이어붙이기
        f4 := append(f1[:2], f2...) // 토마토를 제외하고 이어붙이기

        fmt.Println(f1)
        fmt.Println(f2)
        fmt.Println(f3)
        fmt.Println(f4)
        // Output:
        // [사과 바나나 토마토]
        // [포도 딸기]
        // [사과 바나나 토마토 포도 딸기]
        // [사과 바나나 포도 딸기]
}
```

### 3.2.4 슬라이스 용량
- 슬라이스는 연속된 메모리 공간을 사용하는 것이라서 용량에 제한이 있다.
- 용량이 부족한 경우 기존의 슬라이스 전체를 복사하여 새로운 메모리 공간에 쓰여진다.
- 슬라이스의 뒷 부분을 잘라낼 경우 길이는 줄지만 용량은 그대로, 앞 부분을 잘라낼 경우는 길이와 용량이 같이 줄어든다.
- 용량도 미리 지정해서 생성할 수 있다. nums := make([]int, 3, 5)
- 미리 공간을 예약해 놓을 때 사용한다.
```go
    func Example_sliceCap() {
            nums := []int{1, 2, 3, 4, 5}

            fmt.Println(nums)
            fmt.Println("len:", len(nums))
            fmt.Println("cap:", cap(nums))
            fmt.Println()

            sliced1 := nums[:3]
            fmt.Println(sliced1)
            fmt.Println("len:", len(sliced1))
            fmt.Println("cap:", cap(sliced1))
            fmt.Println()

            sliced2 := nums[2:]
            fmt.Println(sliced2)
            fmt.Println("len:", len(sliced2))
            fmt.Println("cap:", cap(sliced2))
            fmt.Println()

            sliced3 := sliced1[:4]
            fmt.Println(sliced3)
            fmt.Println("len:", len(sliced3))
            fmt.Println("cap:", cap(sliced3))
            fmt.Println()

            nums[2] = 100
            fmt.Println(nums, sliced1, sliced2, sliced3)
            // Output:
            // [1 2 3 4 5]
            // len: 5
            // cap: 5
            //
            // [1 2 3]
            // len: 3
            // cap: 5
            //
            // [3 4 5]
            // len: 3
            // cap: 3
            //
            // [1 2 3 4]
            // len: 4
            // cap: 5
            //
            // [1 2 100 4 5] [1 2 100] [100 4 5] [1 2 100 4]
    }
```

### 3.2.5 슬라이스 내부 구현
- 슬라이스는 시작주소, 길이, 용량을 갖고있는, 배열을 가리키고 있는 구조체 이다.
- 여러 슬라이스가 동일한 배열을 공유할 수 있으며, 복사가 일어날 경우 주소가 변경된다.

### 3.2.6 슬라이스 복사
```go
    func Example_sliceCopy(){
        src := []int{30, 20, 50, 10, 40}
        dest := make([]int, len(src))
        for i := range src {
            dest[i] = src[i]
        }
        fmt.Println(dest)
    }
    // 위 함수는 다음과 같이 변경이 가능하다.
    copy(dest, src)
```

- copy() 함수의 경우 인자 둘 중 작은 쪽을 기준으로 복사가 일어나기 때문에 덜 복사될 수 있다.
```go
    if n:= copy(dest, src); n != len(src) {
        fmt.Println("복사가 덜 됐습니다.")
    }

    //슬라이스 전체를 복사하려면 다음과 같이 사용해야 한다.
    src := []int{30, 20, 50, 10, 40}
    dest := make([]int, len(src))
    copy(dest, src)
```
- append 함수로 간단히 복사된 슬라이스를 얻을 수 있다.
- dest := src 의 경우 배열 포인터, 길이, 용량이 복사된다. 따라서 한쪽의 값을 변경하면 다른쪽도 변경이 일어난다.

### 3.2.7 슬라이스 삽입 및 삭제
- 추후

### 3.2.8 스택
- 후입선출구조(LIFO: Last In First Out)를 갖고 있는 자료구조.
- Go 표준 라이브러리에 따로 스택구조가 들어있지 않다.
```go
    package eval

    // Eval returns the evaluation result of the given expr.
    import (
            "strconv"
            "strings"
    )

    // The expression can have +, -, *, /, (, ) operators and
    // decimal integers. Operators and operands should be
    func Eval(expr string) int {
            var ops []string
            var nums []int
            pop := func() int {
                    last := nums[len(nums)-1]
                    nums = nums[:len(nums)-1]
                    return last
            }
            reduce := func(higher string) {
                    for len(ops) > 0 {
                            op := ops[len(ops)-1]
                            if strings.Index(higher, op) < 0 {
                                    // 목록에 없는 연산자이므로 종료
                                    return
                            }
                            ops = ops[:len(ops)-1]
                            if op == "(" {
                                    // 괄호를 제거하였으므로 종료
                                    return
                            }
                            b, a := pop(), pop()
                            switch op {
                            case "+":
                                    nums = append(nums, a+b)
                            case "-":
                                    nums = append(nums, a-b)
                            case "*":
                                    nums = append(nums, a*b)
                            case "/":
                                    nums = append(nums, a/b)
                            }
                    }
            }
            for _, token := range strings.Split(expr, " ") {
                    switch token {
                    case "(":
                            ops = append(ops, token)
                    case "+", "-":
                            // 덧셈과 뺄셈 이상의 우선순위를 가진 사칙연산 적용
                            reduce("+-*/")
                            ops = append(ops, token)
                    case "*", "/":
                            // 곱셈과 나눗셈 이상의 우선순위를 가진 것은 둘 뿐
                            reduce("*/")
                            ops = append(ops, token)
                    case ")":
                            // 닫는 괄호는 여는 괄호까지 계산을 하고 제거
                            reduce("+-*/(")
                    default:
                            num, _ := strconv.Atoi(token)
                            nums = append(nums, num)
                    }
            }
            reduce("+-*/")
            return nums[0]
    }
```

- 실행
```go
    package eval

    import "fmt"

    func EvalTest() {
            fmt.Println(Eval("8"))
            fmt.Println(Eval("2 + 4"))
            fmt.Println(Eval("1 - 5 + 6"))
            fmt.Println(Eval("3 * ( 1 + 2 * 3 ) / 2"))
            fmt.Println(Eval("3 * ( ( 1 + 1 ) * 3 ) / 4"))
            // Output:
            // 5
            // 3
            // 2
            // 9
            // 18
    }
``` 

//분량상 다음주로
## 3.3 맵
### 3.3.1 맵 사용하기
### 3.3.2 집합
### 3.3.3 맵의 한계

## 3.4 입출력
### 3.4.1 io.Reader와 io.Writer
### 3.4.2 파일 읽기
### 3.4.3 파일 쓰기
### 3.4.4 텍스트 리스트 읽고 쓰기
### 3.4.5 그래프의 인접 리스트 읽고 쓰기

## 3.5 마치며


[reference book]: http://www.aladin.co.kr/shop/wproduct.aspx?ItemId=78786120