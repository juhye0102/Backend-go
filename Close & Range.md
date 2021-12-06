- close

  - 채널에 더 이상 전송할 값이 없으면 채널을 닫을 수 있음.

  - 만약에 채널이 빈 상태가 아니라면 채널이 비워진 후에야 비로소 close가 가능함.

    ```go
    close(ch)
    ```

  - 채널을 닫은 후에 메세지를 전송하면 에러가 발생함.

  ```go
  package main
  
  import "fmt"
  
  func main() {
      ch := make(chan int, 2)
      ch<-1
      ch<-2
      go func(){ ch<-3 }()
      fmt.Println(<-ch)
      fmt.Println(<-ch)
      fmt.Println(<-ch)
      close(ch)
      ch<-4
  }
  
  // 실행결과
  
  1
  2
  3
  panic: send on closed channel
  
  goroutine 1 [running]:
  main.main()
          /Users/parkjinhong/workspace/go/src/package_practice/pkg/goroutine_practice/main.go:28 +0x4c2
  exit status 2
  ```

  - 채널의 수신자는 채널에서 값을 읽을 때 채널이 닫힌 상태인지 아닌지 두 번째 매개변수로 확인할 수 있음.

    ```go
    package main
    
    import "fmt"
    
    func main() {
        ch := make(chan int, 2)
        ch <- 1
    
        if v, ok := <-ch; ok {
            fmt.Println(v)
        }
    
        close(ch)
    
        if v, ok := <-ch; ok {
            fmt.Println(v)
        } else {
            fmt.Println("channel was closed")
        }
    }
    
    // 실행결과
    
    1
    channel was closed
    ```

- range

  - for i := range ch은 채널 c가 닫힐 때까지 반복하며 채널로부터 수신을 시도함.

  ```go
  package main
  
  import "fmt"
  
  func main() {
  		ch := make(chan int, 3)
  		
  		ch <- 1
  		ch <- 2
      ch <- 3
      go func() {
          ch<-4
          close(ch)
      }()
  
      for i := range ch{
          fmt.Println(i)
      }
  
      if v, ok := <-ch; ok {
          fmt.Println(v)
      } else {
          fmt.Println("channel was closed")
      }
  }
  
  // 실행결과
  
  1
  2
  3
  4
  channel was closed
  ```

  - for range 구문 전에 ch를 닫지 않으면 에러가 발생함.

    ```go
    package main
    
    import "fmt"
    
    func main() {
    		ch := make(chan int, 3)
    
    		ch <- 1
    		ch <- 2
    		ch <- 3
    
    		for i := range ch{
    				fmt.Println(i)
    		}
    }
    
    // 실행결과
    
    fatal error: all goroutines are asleep - deadlock!
    
    goroutine 1 [chan receive]:
    main.main()
            /Users/parkjinhong/workspace/go/src/package_practice/pkg/goroutine_practice/main.go:30 +0x398
    exit status 2
    ```

- 정리

  - 채널을 닫아주는 것은 필수가 아님.
  - for range와 같이 수신자가 채널에 더 이상 들어올 값이 없다는 것을 알아야 할 때만 채널을 닫아주면 됨.