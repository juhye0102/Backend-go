# 정보 은닉

- Go에서의 접근 제어

  - Go는 식별자의 첫 번째 문자의 대소문자로 public과 private을 구분함.
  - 구조체의 필드도 대소문자로 구분함.
    - 대문자로 시작하는 필드(exported field) → 패키지 외부에서 접근 가능
    - 소문자로 시작하는 필드(non-exported field) → 패키지 내부에서만 접근 가능
  - 대부분의 클래스 기반 객체 지향 언어에서는 지정된 방식으로 내부 필드에 접근하게 됨.
    - getter로 내부 정보를 얻어오고 setter로 내부 필드의 값을 변경함.
    - 객체에 잘못된 값을 할당하거나 비공개 정보가 노출되는 것을 막을 수 있음.
    - Go에서도 이와 같은 방식으로 내부 정보를 보호함.

- 생성 함수

  - 구조체를 생성할 때 초깃값을 지정하지 않으면 제로값으로 초기화함.

  - 종종 규칙에 따라 초깃값을 원하는 값으로 지정해야 할 때가 있음.

  - Go의 구조체는 생성자를 지원하지 않지만, 구조체 생성을 위한 함수를 만들어 대체할 수 있음

    ```go
    type Item struct {
    		name string 
    		price float64
    		quantity int
    }
    
    func NewItem(name string, price float64, quantity int) *Item {
    		if price <= 0 || quantity <= || len(name) == 0 {
    				return nil
    		}
    		return &Item{name, price, quantity}
    }
    ```

  - Item 타입은 public로, 내부 필드는 외부에서 값을 수정하지 못하는 private로 정의함.

  - 항상 유효한 *Item이 생성되도록 NewItem() 함수를 제공함.

  ### Go 코드 컨벤션

  - 생성 함수의 이름은 New로 시작하도록 하고, 패키지명과 타입명이 같을 때는 New() 로 지정함.

  - ex code

    - item package

      ```go
      package item
      
      type Item struct {
      		name string
      		price float64
      		quantity int
      }
      
      func New(name string, price float64, quantity int) *Item {
      		if price <= 0 || quantity <= 0 || len(name) == 0 {
      				return nil
      		}
      		return &Item{name, price, quantity}
      }
      ```

    - main package

      ```go
      package main
      
      import (
      		"fmt"
      		"/lib/item"
      )
      
      func main() {
      		shirts := item.New("Men's Slim-Fit Shirts", 25000, 3)
      		...
      }
      ```

- getter & setter

  - private 필드에 getter와 setter를 제공하면 항상 유효한 값이 할당되게 할 수 있음.

  ```go
  func (t *Item) Name() string {
  	return t.name
  }
  
  func (t *Item) SetName(n string) {
  	if len(n) != 0 {
  		t.name = n
  	}
  }
  
  func (t *Item) Price() float64 {
  	return t.price
  }
  
  func (t *Item) SetPrice(p float64) {
  	if p > 0 {
  		t.price = p
  	}
  }
  
  func (t *Item) Quantity() int {
  	return t.quantity
  }
  
  func (t *Item) SetQuantity(q int) {
  	if q > 0 {
  		t.quantity = q
  	}
  }
  ```

  - getter, setter 사용

    ```go
    shirts.SetPrice(30000)
    shirts.SetQuantity(2)
    
    fmt.Println("name:", shirts.Name())
    fmt.Println("price:", shirts.Price())
    fmt.Println("quantity:", shirts.Quantity())
    ```

  ### Go 코드 컨벤션

  - getter 메서드는 보통 필드명과 같은 이름으로 지음. (ex. name 필드 → Name() 메서드)
  - setter 메서드는 보통 Set필드명으로 지음. (ex. name 필드 → SetName(n string) 메서드)