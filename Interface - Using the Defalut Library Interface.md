# 기본 라이브러리 인터페이스의 사용

- fmt.String

  - Go의 기본 라이브러리인 fmt 패키지에서 Println() 함수를 정의하는 부분임.

  ```go
  func Println(a ...interface{}) (n int, err error) {
  		return FPrintln(os.Stdout, a...)
  }
  
  func FPrintln(w io.Writter, a ..interface{}) (n int, err error) {
  		...
  		// fmt.Stringer 인터페이스 타입일 때 String() 메서드의 결괏값을 출력
  }
  ```

  - os.Stdout을 첫 번째 매개 변수로 해서 FPrintln() 함수로 전달함.
  - FPrintln() 함수 내부에서는 Fmt.String 인터페이스로 문자열을 출력함.

  ```go
  // fmt 패키지에서 String 인터페이스를 정의하는 부분
  type Stringer interface {
  		String() string
  }
  ```

  - fmt.String 인터페이스에 정의된 String() 메서드를 가지면 기본 출력 명령인 fmt.Println() 함수로 출력될 문자열을 지정할 수 있음.

  ex)

  - Item에 String() 메서드를 추가하여 fmt.Println() 함수로 출력.
  - Item의 String() 메서드는 fmt.Sprintf()로 각 요소의 문자열을 조합해서 만듦.

  ```go
  func (t Item) String() string {
  				return fmt.Sprintf("[%s] %0.f", t.name, t.Cost())
  }
  
  func main() {
  				shoes := Item{"Sports Shoes", 30000, 2}
  				
  				fmt.Println(shoes)
  }
  
  // 실행결과
  [Sports Shoes] 60000
  ```

- io.Writer

  - io의 Writer 인터페이스와 fmt의 Fprintln() 함수는 Go 프로그래밍 시 자주 쓰임.

    - io.Writer 인터페이스

      ```go
      type Writer interface {
      		// 매개변수 p를 받아 byte 길이와 에러 상태 반환 메서드
      		Write)(p []byte) (n int, err os.Error)
      }
      ```

    - fmt.Fprintln() 함수

      ```go
      // 첫 번째 매개변수 w에 두 번째 매개변수 a를 전달하는 기능을 함.
      func FPrintln(w io.Writer, a ...interface{}) (n int err error)
      ```

  - 인터페이스 활용

    - 매개변수로 받은 io.Writer에 msg를 fmt.FPrintln(g)로 출력하는 함수를 정의함.

    ```go
    func handle(w io.Writer, msg string) {
    		fmt.Fprintln(w, msg)
    }
    ```

    - io.Writer로 os.Stdout을 전달하여 msg를 명령 프롬프트에 출력함

    ```go
    func main() {
    		msg := []string{"This", "is", "an", "example", "of", "io.Writer"}
    
        for _, s := range msg {
            time.Sleep(100 * time.Millisecond)
            handle(os.Stdout, s)
        }
    }
    ```

    - handle() 함수에 다른 io.Writer를 전달.
    - main함수에서 HTTP 웹 서버를 동작시키고 io.Writer로 http.ResponseWriter를 전달함.

    ```go
    func main() {
    		http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    				handle(w, r.URL.Path[1:])
    		})
    
    		fmt.Println("start listening on port 4000")
    		http.ListenAndServe(":4000", nil)
    }
    ```

    - fmt 패키지와 http 패키지는 연결고리 X
    - 전달하는 값에 Write() 메서드가 정의되어 있으면, io.Writer로 사용할 수 있음.

- 정리

  - 모듈 간 연계를 쉽게 할 수 있고, 동적인 느낌으로 프로그래밍을 할 수 있음.
  - 인터페이스에 정의된 메소드는 코드 패턴의 컨벤션(관습) 역할을 하고, 컨벤션을 따라 하면 다른 모듈과 쉽게 호환할 수 있음.
  - 컴파일러의 컨벤션 보장을 받으며 동적 코딩을 할 수 있음.