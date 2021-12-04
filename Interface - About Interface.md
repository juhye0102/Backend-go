# 인터페이스(interface)

- 인터페이스란?

  - 인터페이스(interface)의 역할은 객체의 동작을 표현하는 것임.
  - 각 타입이 실제로 내부에 어떻게 구현되어 있는지는 말하지 않고, 단순히 동작 방식만 표현함.
  - 인터페이스의 이러한 특징은 추상 메커니즘을 제공함.
    - 매개변수로 인터페이스를 사용하는 것은 **값의 타입이 무엇인지**보다 **값이 무엇을 할 수 있는지**에만 집중하게 해줌.

- Go에서의 인터페이스

  - Go의 인터페이스는 덕 타이핑 방식을 채택함.
    - 덕 타이핑 방식 → 객체의 변수나 메서드의 집합이 객체의 타입을 결정하는 것.
    - 인터페이스에 걸맞은 요소(메서드)를 가진 타입은 인터페이스로 사용할 수 있음.
  - 인터페이스의 이러한 특징 덕분에 정적 타입 언어인 Go를 유연하게 사용할 수 있음.

- 인터페이스 정의

  - 메서드 서명을 묶어 하나의 인터페이스로 정의함.

    ```go
    type 인터페이스 명 interface {
    		메서드1 (매개변수) 반환타입
    		메서드2 (매개변수) 반환타입
    }
    ```

  - 인터페이스에 정의된 메서드와 서명이 같은 메서드를 가진 타입은 인터페이스로 사용할 수 있음.

    - area() 메서드를 가진 shaper 인터페이스를 정의함.
    - shaper 인터페이스를 매개변수로 받은 후 shaper에 있는 area() 메서드의 실행 결과를 출력하는 describe() 함수를 정의함.

    ```go
    type shaper interface {
    		area() float
    }
    
    func describe(s shaper) {
    fmt.Println("area:", s.area())
    }
    ```

    - rect 구조체와 area() 메서드를 정의함.
    - main 함수에서 rect 타입의 값을 매개변수로 전달하여 describe() 함수를 실행함.

    ```go
    type rect struct { width, height float64 }
    
    func (r rect) area() float64 {
    		return r.width * r.height
    }
    
    func main() {
    		r := rect{3, 4}
    		describe(r)
    }
    
    // 실행결과
    area: 12
    ```

    - rect 구조체와 shaper 인터페이스는 코드에서 아무런 연결 고리가 없음.
    - shaper에서 정의한 메서드를 rect 타입이 제공하면, 해당 타입을 인터페이스로 사용할 수 있음.

- Go 코드 컨벤션

  - 인터페이스 이름은 메서드 이름에 er(또는 r)을 붙여 지음.

  - 인터페이스는 짧은 단위로 만듦.

    - Go의 기본 라이브러리에도 메서드를 하나만 정의한 인터페이스가 대부분임.
    - 많아도 세 개를 넘지 않게 해야함.

  - Go의 기본 라이브러리 인터페이스

    - io  패키지의 Reader 인터페이스

    ```go
    type Reader interface {
    		Read(p []byte) (n int, err error)
    }
    ```

    - io 패키지의 Writer 인터페이스

    ```go
    type Writer interface {
    		Write(p []byte) (n int, err error)
    }
    ```

    - io 패키지의 Closer 인터페이스

    ```go
    type Closer interface {
    		Close() error
    }
    ```

- 익명 인터페이스

  - 타입을 정의하지 않고 인터페이스를 익명으로 사용할 수 있음.

  ```go
  func display(s interface{ show() }) {
  		s.show()
  }
  ```

  - display() 함수에는 show() 메서드를 가진 타입만 매개변수로 전달할 수 있음.

  ### 익명 인터페이스 사용 예제

  ```go
  type rect struct{ width, height float64 }
  
  func (r rect) show() {
      fmt.Printf("width: %f, height: %f\\n", r.width, r.height)
  }
  
  type circle struct { radius float64 }
  
  func (c circle) show() {
      fmt.Printf("radius: %f\\n", c.radius)
  }
  
  func display(s interface{ show() }) {
      s.show()
  }
  
  func main() {
      r := rect{3, 4}
      c := circle{2.5}
      display(r)          // width: 3, height: 4
      display(c)          // radius: 2.5
  }
  ```

- 빈 인터페이스

  - interface{} 타입은 메서드를 정의하지 않은 인터페이스임.
  - interface{} 에는 정의한 메서드가 없어서 어떤 값이라도 interface{}로 사용할 수 있음.
  - 함수나 메서드의 매개변수를 interface{} 타입으로 정의하면 어떤 값이든 전달받을 수 있음.

  ```go
  func main() {
      r := rect{3, 4}
      c := circle{2.5}
      display(r)              // width: 3.000000, height: 4.000000
      display(c)              // radius: 2.500000
      display(2.5)            // 2.5
      display("rect struct")  // rect struct
  }
  
  func display(s interface{}) {
      fmt.Println(s)
  }
  ```