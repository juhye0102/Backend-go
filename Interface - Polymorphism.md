# 다형성

- 다형성이란?

  - 다양한 타입의 객체가 같은 메세지를 통해 다양한 방식으로 동작하게 하는 것을 말함.
  - 다른 언어에서는 서브 타이핑이나 메서드 오버로딩 등으로 다형성을 지원함.
  - Go는 인터페이스로 다형성을 지원함.

  인터페이스의 사용으로 타입이나 메서드의 구현 방식과 관계없이 다양한 값을 같은 형태로 다룰 수 있음.

- 인터페이스를 통한 다향성

  - 하나의 인터페이스로 서로 다른 구조체 세 개를 같은 방식으로 처리.

    - Cost() float64 메서드 서명을 가진 Coster 인터페이스를 만듦.
    - Coster 인터페이스를 매개 변수로 받아 Cost()를 출력해주는 displayCost() 함수를 만듦.

    ```go
    type Coster interface {
    		Cost() float64
    }
    
    func displayCost(c Coster) {
    		fmt.Println("cost:", c.Cost())
    }
    ```

    - Item DiscountItem 타입을 정의하고 각 타입에 Cost() float64 메서드를 정의함.

    ```go
    type Item struct {
    		name string
    		price float64
    		quantity int
    }
    
    func (t Item) Cost() float64 {
    		return t.price * float64(t.quantity)
    }
    
    type DiscountItem struct {
    		Item
    		discoundRate float64
    }
    
    // 메서드 오버라이딩
    func (t DiscountItem) Cost() float64 {
    		return t.Item.Cost() * (1.0 - t.discountRate/100)
    }
    ```

    - Item과 DiscountItem 타입은 Cost() float64 메서드를 가지므로 Coster 인터페이스로 사용할 수 있음.
    - 두 구조체 다 displayCost() 함수를 통해 Cost()를 출력함.

    ```go
    func main() {
    		shoes := Item{"Sports Shoes", 30000, 2}
    		eventShoes := DiscountItem{shoes, 10.00}
    
    		displayCost(shoes) 
    		dispalyCost(eventShoes)
    }
    ```

    - Rental 타입을 새로 추가하고 Rental 타입에도 Cost() 메서드를 정의함.

    ```go
    type Rental struct {
    		name string
    		feePerday float64
    		periodLength int
    		RentalPeriod
    }
    
    type RentalPeriod int
    
    const (
        Days RentlaReriod = iota
        Weeks
        Months
    )
    
    func (p RentalPeriod) ToDays() int {
        switch p {
            case Weeks:
                return 7
            case Months:
                return 30
            default:
                return 1
        }
    }
    
    func (r Rental) Cost() float64 {
        return r.feePerday * float64(r.ToDays() * r.periodLength)
    }
    ```

    - main 함수에서 Item과 Rental 타입 값을 생성함.
    - 생성한 값을 displayCost() 함수에 전달하여 Cost()를 출력함.

    ```go
    func main() {
    		shoes := Item{"Sports Shoes", 30000, 2}
    		videos := Rental{"Interstellar", 1000, 3, Days}
    
    		fmt.Println(displayCost(shoes))
    		fmt.Println(displayCost(videos))
    }
    ```

- 제네릭 컬렉션

  - Go에서는 배열, 슬라이스, 맵에 정해진 타입 값만 담을 수 있음.
  - 타입을 인터페이스로 지정하면 인터페이스를 충족하는 타입 값은 어떤 값이라도 담을 수 있음.

  ### ex) Items 타입을 Coster 인터페이스의 슬라이스로 정의함.

  ```go
  type Items []Coaster
  ```

  - Coaster 인터페이스로 사용할 수 있는 타입은 모두 담을 수 있음.
  - Items에도 Cost() 메소드를 추가하면 Items를 Coaster 인터페이스로 사용할 수 있음.

  ```go
  func (ts Items) Cost() (s float64) {
  		for _, t := range ts {
  				s += t.Cost()
  		}
  		return 
  }
  ```

  - Item, DIscountItem, Rental 모두 Items 슬라이스 하나에 담을 수 있게 됨.
  - 모두 Cost() 메서드를 가지고 있으므로 Coster 인터페이스로 사용할 수 있음.

  ```go
  func main() {
      shoes := Item{"Sports Shoes", 30000, 2}
      eventShoes := DiscountItem{shoes, 10.00}
      videos := Rental{"Interstellar", 1000, 3, Days}
      
      items := Items[shoes, eventShoes, videos]
  
      display(shoes)          // cost: 60000
      display(eventShoes)     // cost: 54000
      display(videos)         // cost: 3000
      display(items)          // cost: 117000
  }
  ```