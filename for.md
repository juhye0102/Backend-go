# for

- for

  Go에서는 모든 반복문을 for문으로 작성함.

  - 기본 문법

  ```go
  for 초기화구문; 조건식; 후속작업 {
  			...
  }
  ```

  - 초기화 구문과 후속 작업은 생략할 수 있고, 각각은 서로 세미콜론으로 구분됨.

  - 생략했을 경우에는 조건식이 true가 될 때까지 내부 코드를 반복하는데, 다른 언어의 while문 처럼 동작함.

  - for문의 전체적인 프로세스 실행순서

    - 반복문을 시작할 때, 초기화 구문을 실행.
    - 조건식을 확인한 후, 조건식이 true가 될 때까지 내부 코드를 반복하여 수행.
    - 각 반복 작업 후에는 후속 작업을 실행.

    ```go
    // 특정 횟수만큼 반복하는 작업은 다음과 같이 작성함.
    for i:=0; i<COUNT; i++ {
    			...
    }
    ```

- break & continue

  - for문을 강제 종료해야 할 경우 break 키워드를 사용.
  - 현재 수행하는 작업을 건너 뛰고 다음 반복 작업을 수행해야 할 때에는 continue 키워드를 사용.

  ```go
  // 100부터 0사이의 홀수를 출력하는 코드
  i := 100
  
  for {
  		if i < 0 {
  				break
  		}
  		if i%2==0 {
  				continue
  		}
  
  		fmt.Println(i)
  }
  ```

- for 레이블 사용

  - for문 앞에 콜론(:)으로 끝나는 문자가 있으면 레이블로 인식.
  - continue, break, 레이블을 함께 사용하면 반복문을 유연하게 제어할 수 있음.

  ```go
  x := 7
  table := [][]int{{1, 5, 9}, {2, 6, 5, 13}, {5, 3, 7. 4}}
  
  LOOP:
  for row:=0; row<len(table); row++ {
      for col:=0; col<len(table[row]); col++ {
          if table[row][col] == x {
              fmt.Println("found %d(row:%d, col:%d)\\n", x, row, col)
              // LOOP로 지정된 for문을 빠져나옴
              break LOOP
          }
      }
  }
  
  // 실행결과
  // fount 7(row:2, col:2)
  ```

  - continue에도 레이블 사용 가능.
  - 반복문 여러 개가 중첩될 때 레이블을 사용하면 가독성이 높은 코드를 작성할 수 있음.