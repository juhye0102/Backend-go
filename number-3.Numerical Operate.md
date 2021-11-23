# 숫자 연산

------

- 숫자 연산의 기본적인 규칙

  - 숫자 연산(산술 연산, 비교 연산)은 타입이 같은 숫자끼리만 할 수 있음.
  - 타입이 다른 숫자끼리 연산하려면 반드시 타입을 변환해주어야 함.

  ```go
  // 타입이 다른 숫자끼리 연산하려고 하면 컴파일 에러가 발생함.
  
  i := 100000
  j := int16(10000)
  k := uint8(100)
  
  fmt.Println(i + j)  // 컴파일 에러
  fmt.Println(i + k)  // 컴파일 에러
  fmt.Println(j > k)  // 컴파일 에러
  
  // 실행 결과
  
  # command-line-arguments
  ./main.go:20:16: invalid operation: i + j (mismatched types int and int16)
  ./main.go:21:16: invalid operation: i + k (mismatched types int and uint8)
  ./main.go:22:16: invalid operation: j > k (mismatched types int16 and uint8)
  ```

- 타입 캐스팅

  - 반드시 같은 타입으로 변환한 후에 연산을 해 주어야 함.

  ```go
  i := 100000
  j := int16(10000)
  k := uint8(100)
  
  fmt.Println(i + int(j)) // 110000
  fmt.Println(i + int(k)) // 100100
  
  // 타입 변환 연산은 오류를 발생시키지 않음.
  // 원래 값보다 작은 범위를 다루는 타입으로 변환 시, 실제 값은 기대했던 결과와 다를 수 있음.
  ```

  ```go
  i := 100000
  
  fmt.Println(int16(i)) // -31072
  fmt.Println(uint8(i)) // 160
  
  // 타입 변환을 하려면 변환할 수 있는 값인지 먼저 점검한 후에 변환해야 함.
  ```

  ```go
  // 타입 변환을 위한 안전한 코드
  
  package main
  
  import (
  	"fmt"
  	"math"
  )
  
  func main() {
  	fmt.Println(intToUint8(100))
  	fmt.Println(intToUint8(1000))
  }
  
  func intToUint8(i int) (uint8, error) {
  	if 0 <= i && i <= math.MaxUint8 {
  		return uint8(i), nil
      }
      // fmt.Errorf() -> 문자열을 기반으로 error를 만들어 반환해줌
  	return 0, fmt.Errorf("%d cannot convert to unit8 type", i)
  }
  
  // 실행결과
  // 100 <nil>
  // 0 1000 cannot convert to uint8 type
  ```

- 증감 연산자 (++, - -)

  - Go에서 증감 연산자는 후치 연산으로만 사용되고 반환 값은 없음.
  - 증감 연산자의 과도한 사용으로 코드의 가독성이 떨어지는 것을 막기 위해서임.

  ```go
  ++x             // 컴파일에러
  y = x++         // 컴파일에러
  print(x++)      // 컴파일에러
  print(++x)      // 컴파일에러
  if x++ > 0 {    // 컴파일에러
  		...
  }
  ```

