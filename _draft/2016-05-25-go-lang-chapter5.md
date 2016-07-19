---
layout: post
title: Go | Chapter 5. struct and interface
---

## 5.1 구조체
: 필드들의 모음 혹은 묶음을 구조체라고 한다. 구조체는 struct {...} 형태로 각 필드의 이름과 자료형을 나열한다.

### 5.1.1 구조체 사용법

```go
    var task = struct {
        title string
        done bool
        due *time.Time
    } {"laundry", false, nil}

    type Task struct {
        title string
        done bool
        due *time.Time
    }
```

- 변수선언

```go
    var myTask = Task{"laundry", false, nil}

    // 필드의 값 정하기
    var myTask = Task{title: "laundry", done: true}
```

### 5.1.2 const와 iota
- enum 자료형이 따로 없고 상수로 정의해서 사용한다.

```go
    // status 자료형
    type status int
```

```go
    // const
    const UNKNOWN status = 0
    const TODO status = 1
    const DONE status = 2

    // iota
    // Go's iota identifier is used in const declarations to simplify
    // definitions of incrementing numbers

    const UNKNOWN status = iota
    const TODO status = iota
    const DONE status = iota

    const {
        UNKNOWN status = iota
        TODO status = iota
        DONE status = iota
    }     
```

### 5.1.3 테이블 기반 테스트
- Go의 testing 패키지에는 asertEqual 과 같은 함수가 지원되지 않는다.
https://godoc.org/github.com/stretchr/testify/assert


### 5.1.4 구조체 내장
: Go 언어의 구조체는 여러 자료형의 필드들을 가질 수 있다는 점이 가장 중요하다.
: 구조체 내장을 이용하면 여러 구조체에 있는 필드를이 모두 합쳐진 구조체 같은 것을 만들 수 있다.
: 상속과 같은 개념은 아니다.

```go
    type Address struct {
        City string
        State string
    }

    type Telephone struct {
        Mobile string
        Direct string
    }

    type Contact struct {
        Address
        Telephone
    }

    func Example Contact() {
        var c Contact
        c.Mobile = "123-456"
        fmtPrint(c.Telephone.Mobile)
        c.AddressCity = "San Fran"
        c.State = "CA"
        c.Direct = "N/A"
        fmt.Println(c)
    }
```

## 5.2 직렬화와 역 직렬화
: 직렬화를 통하여 자료를 파일에 저장하거나 네트워크로 보낼 수 있고, 역직렬화를 통하여 이것을 복원할 수 있다.

### 5.2.1 JSON

### 5.2.2 Gob
- 언어에서 기본으로 제공하는 또 다른 직렬화 방식.
- json 과 다른점은 인코더와 디코더를 생성하고, io.Writer, io.Reader 를 넘긴다.

```go
    func Example_gob() {
        var b bytes.Buffer
        enc := gob.NewEncoder(&b)
        data := map[string]string{"N": "J"}
        if err := enc.Encode(data); err != nil {
            fmt.Println(err)
        }
        const width = 16
        for start := 0; start < len(b.Bytes()); start += width{
            end:= start + width
            if end > len(b.Bytes()) {
                fmt.Printf(?""% x\n", b.Bytes()[start:end])
            }
        }
        dec := gob.NewDecoder(&b)
        var restored map[string]string
        if err := dec.Decode(&restored); err != nil {
            fmt.Println(err)
        }
        fmt.Print(restored)
    }
```

## 5.3 인터페이스
: 인터페이스는 메서드들의 묶음으로 이 메서드들이 모두 정의되어 있는 명명된 자료형은 이 인터페이스를 구현하는 것이 된다.
: 인터페이스 이름을 붙일 때는 주로 인터페이스의 메서드 이름에 er 을 붙인다.

### 5.3.1 인터페이스의 정의
- 구조체와 매우 유사한 구조

```go
    interface {
        Method1()
        Method2(i int) error
    }

    // 인터페이스에 이름을 붙여줄 수 있다.
    type Loader interface {
        Load(filename string) error
    }

    // 구조체의 내장과 비슷한 형식으로 여러 인터페이스를 합칠 수 있다.
    type ReadWriter {
        io.Reader
        io.Writer
    }
```

### 5.3.2 커스텀 프린터

### 5.3.3 정렬과 힙

### 5.3.4 외부 의존성 줄이기

### 5.3.5 빈 인터페이스와 형 단언

### 5.3.6 인터페이스 변환 스위치

