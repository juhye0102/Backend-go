# 클로저

------

- Go에서는 함수는 일급 객체(first-class object)이므로 변수의 값으로 사용할 수 있음.
- 다음과 같이 함수를 변수에 할당하여 변수처럼 사용할 수 있음.

```go
fplus := func(x, y int) int {
		return x + y
}
sum := fplus(3, 4)
```

- 또한 익명 함수는 변수에 할당하지 않고 다음과 같이 바로 호출할 수도 있음.

```go
func(x, y int) int {
		return x+y
}(3, 4)
```

- 위 두 코드를 보면 func 키워드 다음 함수의 이름이 표기되어 있지 않음.

------

### 클로저란?

- 익명 함수들을 클로저(closure)라고 함.
- 클로저는 선언될 떄 현재 범위에 있는 변수의 값을 캡처하고, 호출될 때 캡처한 값을 사용함.
- 클로자가 사용될 때 내부에서 사용하는 변수에 접근할 수 없더라도 선언 시점을 기준으로 해당 변수를 사용함.

```go
// 다음은 클로저를 서용한 팩토리 함수이다.
// 이 팩토리 함수는 파일의 확장자를 만들어 주는 함수를 반환한다.
package main

import (
	"fmt"
	"strings"
)

func makeSuffix(suffix string) func(string) string {
	return func(name string) string {
		if !strings.HasSuffix(name, suffix) {
			return name + suffix
		}
		return name
	}
}

func main() {
	addZip := makeSuffix(".zip")
	addTgz := makeSuffix(".tar.gz")
	fmt.Println(addTgz("go1.5.1.src"))
	fmt.Println(addZip("go1.5.1.windows-amd64"))
}

// 팩토리 함수 makeSuffix는 suffix 변수를 캡처한 클로저를 반환함.
// 이때의 suffix는 클로저가 생성될 때의 값임.
```

------