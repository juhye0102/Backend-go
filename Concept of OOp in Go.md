# 객체 표현 방식

------

- 대부분 언어에서의 객체

  - 객체 기반 언어에서는 상태와 동작을 표현하는 것을 객체라고 함.
  - 보통 하나의 클래스에 상태와 동작을 정의하고, 이를 인스턴스화해서 객체로 사용함.
  - 객체는 상태와 동작을 가짐.

- Go에서의 객체

  - 객체 기반 언어 대부분은 하나의 클래스에 상태와 동작을 모두 표현함.
  - Go는 상태를 표현하는 '타입'과 동작을 표현하는 '메서드'를 분리하여 정의함.
  - 타입과 메서드를 이어주는 명확한 연결고리는 없음.

- 사용자 정의 타입과 메서드

  - 사용자 정의 타입

    - Go에는 기본 타입 외에 사용자가 타입을 직접 정의할 수 있는 사용자 정의 타입이 있음.
    - 일반적으로는 구조체와 인터페이스를 사용자 정의 타입으로 지정해서 씀.
    - 기본 타입이나 함수 서명(signature)을 사용자 정의 타입으로 지정해서 쓰기도 함.

  - 메서드

    - 메서드는 사용자 정의 타입과 함수를 바인딩시키는 방식으로 정의함.
    - 메서드를 정의할 때는 어떤 타입과 연결할지 리시버(receiver)를 지정해줌.
    - 리시버 지정 → 타입과 메서드 연결 → 특정 사용자 정의 타입에 대한 동작 표현 가능.

    ```go
    func (리시버명 리시버타입) 메서드명(매개변수) (반환타입 또는 반환값 ) {
    		// body
    }
    ```

  - Go의 객체 표현

  ```go
  // Item 타입은 name, price, quantity 필드로 현재 상태를 나타냄.
  // Cost() 메서드로 동작을 표현함.
  
  package main
  
  import (
  				"fmt"
  )
  
  // 사용자 사용 타입 정의(구조체)
  type Item struct {
  				name string
  				price float64
  				quantity int
  }
  
  // Item 타입에 Cost() 메서드 정의
  func (t Item) Cost() float64 {
  				return t.price * float64(t.quantity)
  }
  
  func main() {
  				// 아이템 값 생성
  				shirt := Item{
  								name:     "Men's Slim-Fit Shirt",
  								price:    25000,
  								quantity: 3,
  				}
  
  				// Cost() 메서드로 가격 출력
  				fmt.Println(shirt.Cost())
  }
  		
  ```

  - Item 타입을 구조체로 정의했고 안에 name, price, quantity 필드를 정의함.
  - Item 타입을 함수의 리시버로 지정하여 Cost() 는 Item의 메서드가 됨.