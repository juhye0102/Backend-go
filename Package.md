# 패키지

- 패키지란?

  - 코드를 구조화하고 재사용하기 위한 단위로, 다른 언어의 모듈이나 라이브러리와 비슷한 개념임.
  - 모든 Go 프로그램은 패키지로 구성되어 있고, 한 패키지에서 다른 패키지를 임포트 할 수 있음.
  - 패키지와 디렉터리의 이름은 같아야하고, 같은 패키지에 있는 소스 파일은 모두 같은 디렉터리에 있어야 함.

- Go 코드 컨벤션

  - 일반적으로 패키지 이름은 소문자로 지음.
  - 소스 파일 하나로 구성된 패키지는 패키지 이름과 소스 파일 이름을 같게 함.

- 패키지 종류

  - 패키지는 크게 두 가지로 나눌 수 있음.
    - 명령 프롬프트에서 명령을 내려 실행할 수 있는 실행 가능한 프로그램
    - 다른 프로그램에서 호출하여 사용할 수 있도록 연관된 기능의 코드 묶음인 라이브러리

  1. 실행 가능한 프로그램
     - 패키지 이름이 main이면 Go는 실행 가능한 프로그램으로 인식함.
     - main 패키지를 빌드하면 main 패키지의 main()함수를 찾아서 실행시켜주는 디렉터리 이름의 실행파일이 생성됨.
  2. 라이브러리
     - main 외의 패키지는 모두 라이브러리로 만들어짐.
     - 커스텀 패키지를 임포트할 때는 $GOPATH/src 디렉터리 기준의 경로로 해야 함.

- 패키지 사용 예시

  ```go
  // $GOPATH/src/package_practice/lib/lib.go
  package lib
  
  func IsDigit(c int32) bool {
      return '0' <= c && c <= '9'
  }
  ```

  ```go
  // $GOPATH/src/package_practice/pkg/main.go
  package main
  
  import (
      "fmt"
      "package_practice/lib"
  )
  
  func main() {
      fmt.Println(lib.IsDigit('1'))
      fmt.Println(lib.IsDigit('a'))
  }
  ```

  - 실행결과
    - true
    - false