# 버퍼드 채널(buffered channel)

- 버퍼드 채널이란?

  - 채널은 지정한 크기의 버퍼를 가질 수 있는데, 그 버퍼를 가진 채널을 의미함.

  - 채널을 생성할 때 버퍼의 크기를 make의 두 번째 매개변수로 전달하면 버퍼드 채널을 만들 수 있음.

    ```go
    ch := make(chan int, 2)
    ```

- 버퍼드 채널의 동작

  - 버퍼드 채널은 비동기 방식으로 동작함.
    - 채널이 꽉 찰 때까지 채널로 메세지를 계속 전송할 수 있음.
    - 채널이 빌 때까지 채널로부터 메세지를 계속 수신해 올 수 있음.
  - 버퍼드 채널 사용 예시

  ```go
  package main
  
  import "fmt"
  
  func main() {
  		c := make(chan int, 2)
  		c <- 1
  		c <- 2
  		c <- 3
  		fmt.Println(<-c)
  		fmt.Println(<-c)
  		fmt.Println(<-c)
  }
  ```

  - 위 코드는 버퍼가 꽉 참에도 불구하고 계속 송신하여 다음과 같은 에러가 발생함.

  ```go
  fatal error: all goroutines ar asleep - deadlock!
  ```

  - 위 코드를 다음과 같이 수정하면 정상적으로 실행됨.

  ```go
  package main
  
  import "fmt"
  
  func main() {
      c := make(chan int, 2)
      c <- 1
      c <- 2
      go func() {
          c <- 3
      }
  
      fmt.Println(<-c)
      fmt.Println(<-c)
      fmt.Println(<-c)
  }
  
  // 실행결과
  1
  2
  3
  ```

  - 채널 c에 세 번째 값을 전송하는 부분을 별도의 고루틴으로 동작시킴.
  - 고루틴은 채널 c에 값을 전송할 수 있을 때까지 대기하다가, 채널에 들어온 첫 번째 값을 수신해가는 즉시 바로 채널에 값을 전송함.