- 함수를 매개변수로 전달하기

  - 함수를 변수의 값으로 사용 가능.
  - 함수를 다른 함수의 매개변수로 넘길 수 있음.
  - 매개변수로 전달된 함수를 함수 내에서 호출함.

  ```go
  // 매개변수로 함수를 전달하는 예시 코드
  package main
  
  import "fmt"
  
  func callback(y int, f func(int, int)) {
      f(y, 2) // add(1, 2)를 호출
  }
  
  func add (a, b int) {
      fmt.Println("%d + %d = %d", a, b, a+b)
  }
  
  func main() {
      callback(1, add)
  }
  
  // 실행결과
  // 1 + 2 = 3
  ```

- strings.IndexFunc

  - 함수를 매개변수로 전달하는 것은 Go의 기본 라이브러리에서도 흔히 볼 수 있음.
  - 대표적인 예는 strings 패키지의 IndexFunc 함수임.

  ```go
  func IndexFunc(s string, f func(rune) bool) int
  ```

  - 문자열 s에서 f의 수행 결과가 true인 첫 번째 인덱스를 반환함.

  ```go
  package main
  
  import (
  	"fmt"
  	"strings"
  	"unicode"
  )
  
  func main() {
      // 한글이 처음으로 나타나는 인덱스 반환
  	f := func(c rune) bool {
  		return unicode.Is(unicode.Hangul, c) // c가 한글이면 true를 반환
  	}
  	fmt.Println(strings.IndexFunc("Hello, 월드!", f))
  	fmt.Println(strings.IndexFunc("Hello, world!", f))
  }
  ```