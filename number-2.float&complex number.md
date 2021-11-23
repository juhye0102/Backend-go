# 실수와 복소수

- 실수

  - 실수 타입
    1. float32
    2. float64
  - 표기법
    1. 소수 표기법 : 1.0, 1.1, -1.1, ...
    2. 지수 표기법 : 1.12345E3, 1,12345-e3
  - 표현 가능 범위
    1. float32 : 소수점 7자리
    2. float64 : 소수점 15자리
  - 실수 연산을 위한 다양한 함수가 있는 math 패키지가 있음.

- 복소수

  - 복소수(complex number) 타입은 complex64와 complex128이 있음.
  - 각각의 복소수 타입의 표현할 수 있는 범위
    - complex64 : 32비트의 실수부와 허수부
    - complex128 : 64비트의 실수부와 허수부
  - 복소수는 내장 함수 complex() 로 생성하거나 리티럴 표기법으로 직접 변수에 할당할 수 있음.
  - math/cmplx 패키지를 이용하여 다양한 연산 가능.

  ```go
  c1 := 1 + 2i                // complex128
  c2 := complex64(3 + 4i)     // complex64
  c3 := complex(5, 6)         // complex128
  
  fmt.Println(c1, real(c1), imag(c1))
  fmt.Println(c2, real(c2), imag(c2))
  fmt.Println(c3, real(c3), imag(c3))
  
  // 실행결과
  // (1+2i) 1 2
  // (3+4i) 3 4
  // (5+6i) 5 6
  ```