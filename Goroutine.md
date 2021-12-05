- 고루틴(goroutine)이란?

  - 고루틴은 Go 프로그램 안에서 독립적으로 실행되는 흐름의 단위로, 스레드와 비슷한 개념임.

  - 고루틴은 정보를 공유하는 방식이 아니라 서로 메세지를 주고 받는 방식으로 동작함.

  - 그래서 Lock으로 공유 메모리를 관리할 필요가 없고 구현도 어렵지 않음.

    ```go
    // 아래 코드와 같이 go 키워드로 함수를 실행하면 새 고루틴이 만들어짐.
    go f(x, y)
    ```

- 고루틴의 기본 예제

  ```go
  package main
  
  import (
  				"fmt"
  				"time"
  )
  
  func main() {
  				fmt.Println("main 함수 시작", time.Now())
  
  				go long()
  				go short()
  
  				time.Sleep(5 * time.Second)		// 5초 대기
  				fmt.Println("main 함수 종료", time.Now())
  }
  
  func long() {
  				fmt.Println("long 함수 시작", time.Now())
  				time.Sleep(3 * time.Second) // 3초 대기
  				fmt.Println("long 함수 종료", time.Now())
  }
  
  func short() {
  				fmt.Println("short 함수 시작", time.Now())
  				time.Sleep(1 * time.Second) // 1초 대기
  				fmt.Println("short 함수 종료", time.Now())
  }
  
  // 실행결과
  
  main 함수 시작 2019-11-20 22:41:32.910873 +0900 KST m=+0.000054475
  long 함수 시작 2019-11-20 22:41:32.911023 +0900 KST m=+0.000205031
  short 함수 시작 2019-11-20 22:41:32.911047 +0900 KST m=+0.000228702
  short 함수 종료 2019-11-20 22:41:33.911338 +0900 KST m=+1.000515126
  long 함수 종료 2019-11-20 22:41:35.911144 +0900 KST m=+3.000310800
  main 함수 종료 2019-11-20 22:41:37.911141 +0900 KST m=+5.000298169
  ```

  - 두 함수는 go 키워드로 인해 각각 새 고루틴을 생성하여 동시에 동작함.

- 고루틴 사용 시 주의해야 할 점

  - main 함수가 종료되면 실행중인 고루틴이 있어도 프로그램이 종료됨.
  - 아직 실행 중인 고루틴이 있다면 메인 함수가 종료되지 않게 해야 함.
  - 고루틴의 종료 상황을 확인할 수 있는 채널을 만들고, 종료 메세지를 대기시키는 방법을 사용함.