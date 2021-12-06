- 채널이란?

  - 채널(channel)은 고루틴끼리 정보를 교환하고 실행의 흐름을 동기화하기 위해 사용함.

- 채널의 생성과 사용

  - 채널은 일반 변수를 선언하는 것과 똑같이 선언하고, make() 함수로 생성함.

  - 채널을 정의할 때는 chan 키워드로 채널을 통해 주고받을 데이터 타입을 지정함.

  - 채널의 타입을 interface{}로 지정하면 타입에 상관없이 주고 받을 수 있음.

    ```go
    // 채널 변수 선언 후 make() 함수로 채널 생성
    var ch chan string
    ch = make(chan string)
    
    // make() 함수로 채널 생성 후 바로 변수에 할당
    done := make(chan string)
    ```

    - 채널로 값을 주고 받을 때는 <- 연산자를 사용함.

    - <-를 채널 변수 오른쪽에 사용하면 채널에 값을 전송함.

    - <-를 채널 변수 왼쪽에 사용하면 채널로 부터 값을 수신함.

    - 채널에 값을 전송하거나 수신할 때 채널이 준비되지 않으면 준비될 때 까지 대기함.

      ```go
      ch <- "msg" // ch 채널에 "msg" 전송
      m := <-ch   // ch 채널로부터 메세지 수신
      ```

- 고루틴 & 채널을 사용한 예시 코드

  ```go
  package main
  
  import (
  	"fmt"
  	"time"
  )
  
  func main() {
  	fmt.Println("main 함수 시작", time.Now())
  
  	done := make(chan bool)
  	go long(done)
  	go short(done)
  
  	<-done
  	<-done
  	fmt.Println("main 함수 종료", time.Now())
  }
  
  func long(done chan bool) {
  	fmt.Println("long 함수 시작", time.Now())
  	time.Sleep(3 * time.Second) // 3초 대기
  	fmt.Println("long 함수 종료", time.Now())
  	done <- true
  }
  
  func short(done chan bool) {
  	fmt.Println("short 함수 시작", time.Now())
  	time.Sleep(1 * time.Second) // 1초 대기
  	fmt.Println("short 함수 종료", time.Now())
  	done <- true
  }
  
  // 실행결과
  
  main 함수 시작 2019-11-20 23:33:37.830138 +0900 KST m=+0.000053453
  short 함수 시작 2019-11-20 23:33:37.830284 +0900 KST m=+0.000199975
  long 함수 시작 2019-11-20 23:33:37.830325 +0900 KST m=+0.000240986
  short 함수 종료 2019-11-20 23:33:38.832817 +0900 KST m=+1.002722606
  long 함수 종료 2019-11-20 23:33:40.830825 +0900 KST m=+3.000711925
  main 함수 종료 2019-11-20 23:33:40.830855 +0900 KST m=+3.000741883
  ```

  - 함수로 done 채널을 전달하고, 각 함수에서는 처리를 완료한 후 done 채널로 완료 메세지를 전달함.
  - 메인 함수에서는 done 채널로부터 메세지를 받고 프로그램을 종료함.

- 여전히 남아있는 교착상태의 위험

  - 동시성을 처리할 때 sync 패키지의 저수준 기능이 아니라, 채널을 사용한다 해도 교착상태 위험은 여전히 남아있음.

  - 서로 데이터를 주고 받는 고루틴이 있을 경우 교착상태에 빠질 수 있음.

    - 고루틴1은 고루틴2로부터 데이터를 받아 처리한 후 결과를 고루틴2로 보냄.
    - 고루틴2도 데이터를 받아 처리한 다음, 다시 고루틴1로 전달하는 상황.

  - Go는 교착상태와 경쟁상태를 테스트하기 위해 Race Detector를 제공함.

    - 사용 방법

      ```go
      go test -race mypkg
      go run -race mysrc.go
      go build -race mypkg
      go install -race mypkg
      ```