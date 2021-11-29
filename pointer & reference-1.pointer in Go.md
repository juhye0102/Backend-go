# 포인터와 참조 타입

------

- Go에서의 포인터란?

  - C & C++ vs Python & C# vs Go
    - C와 C++ 에서는 포인터로 메모리 주소를 직접 제어할 수 있음.
      - → 코드의 가독성과 안정성을 떨어뜨림.
    - C#이나 Python는 포인터를 사용하지 않고 객체 참조 방식으로 메모리에 접근함.
    - Go는 포인터와 참조 타입을 모두 제공함.
  - 참조 타입 → 슬라이스, 맵, 채널, 함수, 메서드
  - → Go에서 포인터는 값에 접근하는 수단일 뿐, 주소 값을 직접 변경할 수는 없음.

- 포인터 생성과 초기화

  - 포인터 변수는 타입 앞에 * 연산자를 표기하여 선언함.

  ```go
  type rect struct{w, h float64}
  
  var pRect *rect
  var pInt *int
  var pFloat *float64
  var pComplex *complex128
  ```

  - 포인터의 선언 방식

    - 주소 연산자(&)로 특정 값의 메모리 주소를 포인터 변수에 할당함.

      - 변수 앞에 주소 연산자(&)를 붙이면 변수의 주소를 알아낼 수 있음. → 그 주소 값을 변수에 할당해서 사용 가능.

      ```go
      var p *int
      i := 1
      p = &i
      fmt.Println(i)  // 1
      fmt.Println(&i) // 0xc00001a1e8
      fmt.Println(*p) // 1
      fmt.Println(p)  // 0xc00001a1e8
      ```

      - 선언과 동시에 초기화하는 코드를 작성할 수도 있음.

      ```go
      type rect struct {
      		w, h float64
      }
      
      var i int = 1
      var p *int = &i
      var s *rect = &rect{1, 2}
      
      fmt.Println(p)
      fmt.Println{s)
      
      // 실행결과
      // 0xc00001a1d0
      &{1 2}
      ```

    - new()함수로 메모리를 초기화 한 후 포인터 변수에 할당

      - new 함수 사용 → 매개변수로 전달한 타입에 맞는 메모리 공간 초기화 → 주소 반환

      - → 반환된 메모리 주소를 포인터 변수에 할당해서 사용 가능.

        - 정수 포인터를 new()로 생성

        ```go
        p := new(int)
        *p = 1
        fmt.Println(p)  // 0xc00001a1d8
        fmt.Println(*p) // 1
        ```

        - 구조체 포인터를 new()로 생성

        ```go
        r := new(rect)
        r.w, r.h = 3, 4
        fmt.Println(r)  // &{3, 4}
        fmt.Println(*r) // {3, 4}
        ```