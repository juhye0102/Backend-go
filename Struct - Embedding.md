# 구조체 임베딩

- 보편적인 코드 재사용

  - 보편적으로 상속을 통해 코드를 재사용하고, 객체 지향 언어 대부분은 상속을 지원함.
  - 상속 관계가 깊어지면 클래스들이 거대한 트리 구조가 되어 여러 문제를 유발함.
  - 디자인 패턴에서는 상속보다 조합을 강조함.

- Go에서의 상속/조합

  - 상속은 여러 문제가 있기 때문에 Go에서는 상속을 없앰.
  - 사용자 정의 타입을 조합하여 구조체를 정의하는 방식으로 객체를 재사용함.
  - 사용자 정의 타입을 구조체 필드로 지정하는 것을 임베딩이라고 함.

  ```go
  type 타입명 struct {
  		타입1 // 임베디드 필드
  		타입2 // 임베디드 필드
  		...
  }
  
  type 타입1 struct {
  		...
  }
  
  type 타입2 struct {
  		...
  }
  ```

- 임베디드 필드

  - . 연산자로 임베디드 필드의 내부 필드에 바로 접근할 수 있음.
  - 임베디드 필드의 내부 필드와 이름이 같은 필드가 있을 때는 임베디드 필드의 타입도 적어주어야 함.

  ```go
  package main
  
  import "fmt"
  
  type Option struct {
  		name string
  		value string
  }
  
  type emItem struct {
  		name string
  		price float64
  		quantity int
  		Option // 임베디드 필드
  }
  
  func main() {
  		shoes := emItem{
  				name: "Sports Shoes",
  				price: 300000,
  				quantity: 2,
  				Option: Option{"color", "red"},
  		}
  
  		fmt.Println(shoes)
  
  		// name 필드에 접근
  		fmt.Println(shoes.name)
  
  		// 임베디드 필드인 Option 구조체의 내부 필드인 value에 접근
  		fmt.Println(shoes.value)
  
  		// 임베디드 필드인 Option 구조체의 내부 필드인 name에 접근
  		// Item 구조체에 이름이 같은 필드가 있으므로 Option 타입을 명시
  		fmt.Println(shoes.Option.name)
  }
  ```

- 메서드 재사용

  - 구조체 임베딩 시 임베디드 필드가 포함된 구조체에서 임베디드 필드에 정의된 메서드를 사용할 수 있음.

    1. emItem 구조체에 Cost() 메서드를 정의함.
    2. emltem을 DiscountItem의 임베디드 필드로 확장함.
    3. main 함수에서 DiscountItem 타입인 eventShoes를 선언함.
    4. DiscountItem에서 Item 타입에 정의된 Cost() 메서드를 호출함.

    ```go
    package main
    
    import "fmt"
    
    type emItem struct {
    		 name string
    		price float64
    		quantity int
    }
    
    func (t emItem) Cost() float64 {
    		return t.price * float64(t.quantity)
    }
    
    type DiscountItem struct {
    		emItem
    		discountRate float64
    }
    
    func main() {
    		shoes := emItem{"Women's Walking Shoes", 30000, 2}
    
    		eventShoes := DiscountItem{emItem{"Sport Shoes", 50000, 3",10.00}
    
    		fmt.Println(shoes.Cost())
    		fmt.Println(eventShoes.Cost())
    }
    ```

- 메서드 오버라이딩(Overriding)

  - 임베디드 필드에 정의된 메서드를 오버라이딩(Overriding) 할 수도 있음.

  ```go
  func (t DiscountItem) Cost() float64 {
  		return t.emItem.Cost() * (1.0-t.discountRate/100)
  }
  
  func main() {
  		shoes := emItem{"Women's Walking Shoes", 30000, 2}
  
      eventShoes := DiscountItem{emItem{"Sport Shoes", 50000, 3},10.00}
  
      fmt.Println(shoes.Cost())               // 60000
      fmt.Println(eventShoes.Cost())          // 135000
      fmt.Println(eventShoes.emItem.Cost())   // 150000
  }
  ```

  - emItem 타입에 정의된 Cost() 메서드와 이름이 같은 메서드를 DiscountItem타입에 추가함.
  - Item의 Cost() 메서드는 오버라이딩 됨.

- 오버로딩(Overloading)

  - Go는 이름이 같은 메서드를 생성할 수 없으므로 오버로딩을 지원하지 않음.

  - 따라서 매개 변수가 다르더라도 메서드 이름을 다르게 지정해야 함.

  - 기본 라이브러리에 있는 string.Reader 타입은 매개변수에 따라 다른 메서드 세 개를 제공함.

    ```go
    func (r *Reader) Read(b []byte) (n int, e error)
    func (r *Reader) ReadByte(b byte) (b byte, e error)
    func (r *Reader) ReadRune(ch rune) (ch rune, size int, e error)
    ```