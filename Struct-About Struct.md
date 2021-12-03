# 구조체

------

- 구조체란?

  - 구조체(struct)는 각자의 속성을 가진 실세계의 엔티티를 표현한 것임.

  - 이름과 타입을 가진 필드(field)로 여러 속성을 표현할 수 있음.

    ```go
    struct 타입명 struct {
    		필드명1 필드타입1
    		필드명2 필드타입2
    		...
    }
    ```

  - 내장 데이터 뿐만 아니라 사용자 정의 타입도 필드로 지정할 수 있음.

  - 함수 서명의 매개변수 부분처럼 한꺼번에 묶어서 타입을 지정할 수 있음.

    ```go
    type 타입명 struct {
    		필드명1, 필드명2 필드타입
    		필드명3 필드타입 
    		...
    }
    ```

- 생성과 초기화

  - 구조체 생성 방식

    ```go
    타입{초깃값}   // 1. 구조체 리티럴로 생성
    &타입{초깃값}  // 2. 구조체 리티럴의 포인터 생성
    new(타입)     // 3. 구조체 포인터 생성
    ```

  - 구조체 리티럴로 생성

    - 기본적으로 {필드명1: 값1, 필드명2: 값2} 형태로 나열해서 초깃값을 할당하여 구조체를 생성함.

    - 초깃값을 할당하지 않은 필드는 제로값으로 초기화됨.

    - 필드명을 작성하지 않고도 필드가 정의된 순서대로 할당할 초깃값을 나열하여 초기화할 수도 있음.

      ```go
      type Item struct {
      		name string
      		price float64
      		quantity int
      }
      
      func main() {
      		shirts := Item{name: "Men's Slim-Fit Shirt", price: 25000, quantity: 3}
      		shoes := Item{name: "Sports Shoes", price: 30000}
          dress := Item{name: "Stripe Shift Dress", quantity: 2}
          phone := Item{"Amazon Fire Phone, 32GB", 21900,
          
          fmt.Println(shirts)
          fmt.Println(shoes)
          fmt.Println(dress)
          fmt.Println(phone)
      }
      
      // 실행결과
      
      {Men'S Slim-Fit Shirt 25000 3}
      {Sports Shoes 30000 0}
      {Stripe Shift Dress 0 2}
      {Amazon Fire Phone, 32GB 21900 1}
      ```

  - 구조체 리티럴의 포인터 생성

    - 구조체 리티럴 앞에 주소 연산자(&)를 붙이면 생성된 구조체의 메모리 주소를 반환함.

    ```go
    p := &Item{name: "Men's Slim-Fit Shirt", price: 25000, quantity: 3}
    
    fmt.Println(p)              // &{Men's Slim-Fit Shirt 25000 3}
    fmt.Println(p.cost())       // 75000
    ```

  - new() 함수로 구조체 포인터 반환

    - new() 함수는 제로값으로 초기화 되어 생성된 구조체의 메모리 주소가 담긴 포인터를 반환함.

      ```go
      item := new(Item)
      item.name = "Men's Slim-Fit Shirt"
      
      item.price = 25000
      item.quantity = 3
      
      fmt.Println(item)           // &{Men's Slim-Fit Shirt 25000 3}
      fmt.Println(item.cost())    // 75000
      ```

      - 구조체 포인터에도 구조체 타입에 정의된 메서드를 호출할 수 있음.

- 구조체 포인터 생성 방법 비교

  - 구조체의 포인터를 생성하는 방법

    - new(Type)

    - &Type{}

      - 두 구문 모두 메모리 공간을 제로값으로 할당해 초기화 한 후 그 메모리 주소를 반환함.

        - Type{} 구문은 선언과 동시에 초깃값이 할당된 구조체의 포인터 선언이 가능하여 활용도가 높음.

        ```go
        type rect struct {w, h, float64}
        
        r1 := rect{1, 2}
        re := new(rect)
        r2.w, r2.h = 3, 4
        r3 := &rect{}
        r4 := &rect{5, 6}
        
        fmt.Println(r1)
        fmt.Println(r2)
        fmt.Println(r3)
        fmt.Println(r4)
        
        // 실행결과
        {1 2}
        &{3 4}
        &{0 0}
        &{5 6}
        ```

- 익명 구조체

  - 구조체를 타입으로 정의하지 않고 익명으로 사용할 수 있음.

  ```go
  rects := []struct{w, h int}{{1, 2}, {}, {-1, -2}}
  for _, r := range rects {
  		fmt.Printf("%d %d\\n" r.w, r.h)
  }
  ```