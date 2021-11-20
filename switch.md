# switch

- 기본 형태

```go
switch 값 {
		case 조건1:
				...
		case 조건2:
				...
		default:
				...
}
```

- if문과 마찬가지로 여는 괄호는 switch와 같은 줄에 있어야 함.
- 또한 콤마','를 이용하여 case 뒤에 값을 여러 개 쓸 수 있음.
- Go에서는 다른 언어와 다르게 기본으로 case 구문 마지막에 break가 실행됨.

```go
var i int = 1

switch i (
		case 0: // i가 0일 때는 아무것도 수행하지 않음
		case 1: // i가 1일 때만 수행됨.
				func()
)
```

- Go는 fallthrougt를 이용하여 case 구문에서 다음 case 구문으로 넘어가게 할 수 있음.
- 하지만 다음 case로 넘겨서 한 번에 처리하는 경우보다는 case에 조건을 여러 개 넣어 처리하는 것이 좋음.

```go
var i int = 1

switch i (
		case 0:
				fallthrough
		case 1: / i가 1일 때와 2일 때 모두 수행함
				func()
)
```

- if문과 마찬가지로 switch문에서도 초기화 구문을 사용할 수 있음.
- 초기화 구문에 선언된 변수는 switch문 내에서만 사용할 수 있음.

```go
switch a, b := x[i], y[j]; {
		case a < b:
				fmt.Println("a는 b보다 작음")
		case a == b:
				fmt.Println("a는 b와 같음")
		case a > b:
				fmt.Println("a는 b보다 큼")
}
```