- 매개변수 전달 방식

  > Go는 값에 의한 호출(call by value)이 기본임.

- 값에 의한 호출(call by value)

  - 값에 의한 호출은 함수를 호출할 때 매개변수 값을 복사해서 함수 내부로 전달함.
  - 함수 내부에서는 전달된 매개변수의 본래 값을 변경할 수 없음.
  - 함수 내부에 본래 값을 변경하기 위해서는 & 연산자로 변수의 메모리 주소를 전달해야 함.

  ```go
  package main
  
  import "fmt"
  
  func inc(i int) {
  // inc 함수 내부에서 다루는 i는 main에서 첫 줄에 선언한 i가 아니기 때문에 적용이 되지 X
  		i = i + 1
  }
  
  func main() {
  		i := 10
  		inc(i)
  		fmt.Println(i)
  }
  
  // 실행 결과
  // 10
  ```

- 참조에 의한 호출(call by reference)

  - 참조에 의한 호출은 매개변수로 매개변수의 메모리 주소를 받음.
  - 함수의 매개변수 타입을 포인터로 지정하면 변수의 값이 아닌 변수의 메모리 주소가 전달됨.
  - 포인터로 메모리 주소에 접근하면 매개변수로 전달된 인수의 본래 값을 변경할 수 있음.

  ```go
  package main
  
  import "fmt"
  
  func inc(i *int) {
  		*i = *i + 1
  }
  
  func main() {
  		i := 10
  		inc(i)
  		fmt.Println(i)
  }
  
  // 실행결과
  // 11
  ```