- 메서드

  ------

  - 메서드는 사용자 정의 타입 값에 호출할 수 있는 특별한 함수임.
  - 리시버 타입 변수에 메서드 호출 → 변수가 메서드 내부로 전달 → 메서드 내부에서 사용 가능.

  ------

  - 리시버 값 전달 방식

    - 함수와 마찬가지로 메서드는 값에 의한 호출이 기본 방식임.
    - 메서드 내부에서 리시버 변수의 값을 변경하려면 변수의 메모리 주소를 전달해야 함.
    - 참조에 의한 호출로 주소를 전달하려면 리시버 변수 타입에 * 를 사용하여 포인터로 지정해야 함.

    ```go
    type quantity int
    
    func (q quantity) greaterThan(i int) bool {
    		return int(q) > i
    }
    
    func (q *quantity) increment() { *q++ }
    
    func (q *quantity) decrement() { *q-- }
    
    func main() {
    		q := quantity(3)
    		q.increment()
    		fmt.Printf("Is q(%d) greater than %d? %t\\n", q, 3, q.greaterThan(3))
    		q.decrement()
    		fmt.Printf("Is q(%d) greater than %d? %t\\n", q, 3, q.greaterThan(3))
    }
    
    // 실행결과
    Is q(4) greater than 3? true
    Is q(3) greater then 3? false
    ```

    - 필드를 많이 갖는 구조체라면 복사하기에 시스템 리소스가 많이 소요될 것임.
    - 리시버 변수의 값을 변경하지 않아도 리시버를 포인터로 지정하는 습관을 들이는 것이 좋음.

  - 리시버 변수 생략

    - 메서드 내부에서 리시버 변수를 사용하지 않을 때도 있음.

    ```go
    type rect struct {
    		width float64
    		height float64
    }
    
    func (rect) new() rect {
    		return rect{}
    }
    
    func main() {
    		r := rect{}.new()
    		fmt.Println(r)
    }
    ```

    - 메서드 내부에서 리시버 변수를 사용하지 않는다면 메서드 정의시 리시버 변수를 생략할 수 있음.

  - 메서드 함수 표현식

    - 메서드도 일급 객체이기 때문에 변수에 할당할 수 있고 함수의 매개변수로 전달할 수 있음.
    - 메서드의 함수 표현식은 메서드의 리시버를 첫 번째 매개변수로 전달하는 함수임.

    ```go
    // rect와 area(), resize() 메서드를 함수 표현식으로 변환하여 사용
    
    package amin
    import "fmt"
    
    type myRect struct {
    				width, height float64
    }
    
    func (r myRect) area() float64 {
    				return r.width * r.height
    }
    
    func (r *myRect) resize(w, h float64) {
    				r.width += w
    				r.height += h
    }
    
    func main() {
    				r := myRect{3, 4}
    				fmt.Println("area:", r.area())  // area: 12
    				
    				r.resize(10, 10)
    				fmt.Println("area:", r.area())  // area: 182
    				
    				/* area() 메서드의 함수 표현식
    				서명: func(rect) float64 */
    				areaFn := myRect.area
    				
    				/* resize() 메서드의 함수 표현식
    				서명: func(*rect, float64, float64) */
    				resizeFn := (*myRect).resize
    				
    				fmt.Println("area:", areaFn(r))  // area: 182
    				resizeFn(&r, -10, -10)
    				fmt.Println("area:", areaFn(r))  // area: 12
    				}
    ```

    - 함수 표현식으로 사용한 두 메서드

      - areaFn 메서드 → func(rect) float64

        → 첫 번째 매개변수로 rect를 받음

      - resizeFn 메서드 → func(*rect, float64, float64)

        → 첫 번째 매개변수로 *rect를 받고, 두 번째와 세 번째 매개변수로 float64를 받음.

    - 메서드 자체를 다른 함수의 매개변수로 전달해야 할 때 메서드를 함수 표현식으로 변환하여 전달함.