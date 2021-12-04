# 인터페이스 임베딩

- 인터페이스 임베딩이란?

  - 구조체와 마찬가지로 인터페이스도 다른 인터페이스를 임베딩 할 수 있음.
  - 임베디드된 인터페이스의 메서드 서명을 직접 갖고 있는 것과 같은 효과를 줌.

- 인터페이스 임베딩 예시

  - Coaster, fmt.Stringer를 임베딩한 Itemer 인터페이스를 정의.

  ```go
  type Itemer interface {
  		Coaster
  		fmt.String
  }
  ```

  - Itemer 인터페이스로 사용하려면 다음과 같은 메서드를 가지고 있어야 함.
    - Coster 인터페이스에 정의된 Cost() 메서드
    - fmt.Stringer 인터페이스에 정의된 String() 메서드
  - Itemer를 임베디드 필드로 가진 Order 구조체를 정의.
    - Cost() 메서드 → tax를 적용한 최종 가격 계산
    - String() 메서드 → 전체 정보를 담는 문자열을 만듦.

  ```go
  type Order struct {
      Itemer
      taxRate float64
  }
  
  func (o Order) Cost() float64 {
      return o.Itemer.Cost() * (1.0 + o.taxRate/100)
  }
  
  func (o Order) String() string {
      return fmt.Sprintf("Total price: %.0f(tax rate: %.2f)\\n\\tOrder detail: %s", o.Cost(), o.taxRate, o.Itemer.String())
  }
  ```

  - Order 구조체의 Itemer 필드에는 다음과 같은 타입들의 값을 할당할 수 있음.

    - Item
    - DiscountItem
    - Rental
    - Items

    ```go
    func main() {
    		shoes := Item{"Sports Shoes", 30000, 2}
    				eventShoes := DiscountItem{shoes, 10.00}
    				videos := Rental{"Interstellar", 1000, 3, Days}
        items := Items{shoes, eventShoes, videos}
        
        order1 := {Items, 10.00}
        order2 := {video, 5.00}
    }
    
    // 실행결과
    Total price: 128700(tax rate: 10.00)
            Order detail: 3 items.total: 117000
            -[Sports Shoes] 60000
            -[Sports Shoes] 60000 => 54000(10% DC)
            -[Interstellar] 3000
    Total price: 3150(tax rate: 5.00)
            Order detail: [Interstellar] 3000
    ```