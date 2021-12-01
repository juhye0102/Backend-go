# 사용자 정의 타입

------

- 사용자 정의 타입

  - 사용자 정의 타입(custom type)은 Go의 기본 타입 외에 추가로 타입을 정의할 때 사용함.
  - type이라는 키워드를 사용하여 생성함.

  ```go
  type 타입명 타입명세
  ```

  - 타입명은 패키지나 함수 내에서 유일해야 함.
  - 타입 명세 목록
    - Go의 기본 타입 (string, int, slice, map, channel 등)
    - 구조체
    - 인터페이스
    - 함수 서명

- 사용자 정의 타입의 종류

  - 주로 구조체나 인터페이스를 정의할 때 사용자 정의 타입을 사용함.

  - 기본 타입이나 함수 서명을 사용자 정의 타입으로 지정해서 쓰기도 함.

  - 기본 타입을 사용자 정의 타입으로 사용.

    ### 1. 기본 타입을 사용자 정의 타입으로 사용

    - 코드 문맥에 맞는 의미를 부여하기 위해 기본 타입을 사용자 정의 타입으로 정의함.

    ```go
    type quantity int
    type dozen []quantity
    
    var d dozen
    for i:=quantity(1); i<=12; i++ {
    		d = append(d, i)
    {
    fmt.Println(d)
    
    // 실행결과
    // [1 2 3 4 5 6 7 8 9 10 11 12]
    ```

    - 기본 타입을 매개변수로 받는 함수에 사용자 정의 타입을 매개변수로 전달하려면 기본 타입으로 변환해야 함.
    - 반대의 경우에도 기본 타입을 사용자 정의 타입으로 변환하여 전달해야 함.

    ```go
    type quantity int
    
    func main() {
    		var q quantity = 3
    		display(int(q))
    }
    
    func display(i int) {
    		fmt.Println(3) // 3
    }
    ```

  - 함수 서명을 사용자 정의 타입으로 사용.

    ### 2. 함수 서명을 사용자 정의 타입으로 사용

    - 코드 문맥에 맞는 의미 부여를 위해 함수 서명의 사용자 정의 타입의 정의도 유용함.

    ```go
    type quantity int
    type costCalculator func(quantity, float64) float64
    
    func describe(q quantity, price float64, c costCalculator) {
    fmt.Printf("quantity: %d, price: %0.0f, cost: %0.0f\\n", q, price, c(q, price))
    }
    
    func main() {
    		var offBy10Percent, offBy1000Won costCaluculator
    
    		offBy10Percent = func(q quantity, price float64) float64 {
    				return float64(q) * price * 0.9
    		}
    
    		offBy1000Won = func(q quantity, price float64) float64 {
    				return float64(q) * price - 1000
    		} 
    		
    		describe(3, 10000, offBy10Percent)
    		describe(3, 10000, offBy1000Won)
    }
    
    // 실행결과
    // quantity: 3, price: 10000, cost:27000
    // quantity: 3, price: 10000, cost:29000
    ```

    - costCalculator과 서명이 일치하는 함수를 만들었고, 이를 describe() 함수의 매개변수로 전달함.
    - 기본 서명인 func(quantity, float64) float64보다 costCalculator 타입의 사용
      - 더 가독성이 높아짐.
      - 의미도 분명하게 전달됨.

  - 구조체

    - 구조체는 여러 속성을 묶어서 하나의 타입으로 정의할 때 사용함.
    - ex ) rect

    ```go
    type rect struct {
    		width float64
    		height float64
    }
    
    func (r rect) area() float64 {
    		return r.width * r.height
    }
    
    func main() {
    		r := rect{3, 4}
    fmt.Println("area:", r.area())
    }
    ```

    - struct 키워드로 구조체를 정의했고, 구조체 내부에 필드명 필드타입 형태로 필드를 나열함.
    - area() 메서드를 정의한 후 main 함수에서 rect 타입을 사용함.

  - 인터페이스

    - Go의 인터페이스는 메서드의 묶음임.
    - 인터페이스에 정의된 메서드와 서명이 같은 메서드가 정의된 타입은 인터페이스로 사용할 수 있음.

    ```go
    type shaper interface {
    		area() float64
    }
    
    func describe(s shaper) {
    		fmt.Println("area:", s.area())
    }
    
    func main() {
    		r := rect{3, 4}
    		describe(r)
    }
    ```

    - shaper 인터페이스와 rect 타입 사이에는 아무런 연걸 고리가 없음.
    - rect타입이 shaper 인터페이스에 정의된 메서드와 형태가 같은 메서드를 가진 것만으로도 rect 타입을 shaper 인터페이스로 사용할 수 있음.