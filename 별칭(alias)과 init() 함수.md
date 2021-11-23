# 별칭(alias)과 init() 함수

------

- 별칭 (alias)

  - Go에서는 패키지 이름에 별칭을 줄 수 있음.

    - 주로 패키지 이름이 길거나 중복되는 패키지가 있을 때 별칭을 사용.

      ```go
      // 아래 코드는 lib 패키지를 별칭을 사용하여 임포트한 코드임.
      
      package main
      
      import (
      				"fmt"
      				mylib "package_practice/lib"
      )
      
      func main() {
      				fmt.Println(mylib.IsDigit('1'))
      				fmt.Println(mylib.IsDigit('a'))
      }
      ```

  - 또한 Go에서는 임포트한 패키지를 사용하지 않으면 컴파일 에러가 발생함.

    - 패키지 이름에 빈 식별자(_)로 별칭을 주면 컴파일 에러 발생 X.

    - 디버깅 할 때 유용하게 쓰이는 기능임.

      ```go
      package main
      
      import (
      				"fmt"
      				_ "package_practice/lib"
      )
      
      func main() {
      					fmt.Println("lib 패키지 사용 잠시 제거")
      
      					//fmt.Println(mylib.IsDigit('1'))
      					//fmt.Println(mylib.IsDigit('1'))
      }
      ```

- init() 함수

  - init() 함수는 패키지가 로드될 때 가장 먼저 호출되는 함수임.

    - 패키지의 초기화 로직이 필요할 때 선택적으로 사용하면 됨.

    ex)

    ```go
    package main
    
    import (
    		"fmt"
    		"package_practice/lib"
    )
    
    var v rune
    
    func init() {
    		v = '1'
    }
    
    func main() {
    		fmt.Println(lib.IsDigit(v))
    }
    
    // 실행결과
    // true
    ```

- init() 함수 실행 순서

  - Go 프로그램은 항상 main() 함수로 시작됨.
    - main 패키지가 다른 패키지를 임포트 하고 있으면 임포트된 패키지를 먼저 불러옴.
    - 임포트된 패키지가 다른 패키지를 임포트하고 있으면 관련된 패키지를 먼저 불러옴.
      - 따라서 임포트된 패키지를 모두 불러온 후 main() 함수가 실행됨.

  