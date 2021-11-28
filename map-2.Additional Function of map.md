# map의 추가적인 기능

------

- 값 찾기

  - 인덱스 연산자([])로 특정 키에 대한 값을 얻을 수 있음.
  - 키가 없을 때는 값으로 지정한 타입의 제로값을 반환함.

  ```go
  numberMap := map[string]int{}
  numberMap["zero"] = 0
  numberMap["one"] = 1
  numberMap["two"] = 2
  fmt.Println(numberMap["zero"])   // 0
  fmt.Println(numberMap["one"])    // 1
  fmt.Println(numberMap["two"])    // 2
  fmt.Println(numberMap["three"])  // 0
  
  // 키가 three인 요소의 값이 실제로 0인지, 아니면 없어서 제로값을 반환했는지 알 수 없음.
  // []연산자의 두 번째 매개변수인 bool 값을 이용해서 확인할 수 있음.
  
  if v, ok := numberMap["three"]; ok {
  		fmt.Println("'three' is in numberMap. value:", v)
  } else {
  		fmt.Println("'three' is not in numberMap")
  }
  ```

- 요소 추가, 수정, 삭제

  - 맵에 새 키/값을 추가하는 구문은 기존 키의 값을 수정하는 구문과 같음.

  - 모두 [] 연산자를 사용함. (myM ap[key] = value)

    - 키에 해당하는 요소 X → 새로운 요소로 추가됨.
    - 키에 해당하는 요소 O → 기존 값을 수정함.

    ```go
    numberMap = map[int]string{}
    numberMap[1] = "one"
    numberMap[2] = "two"
    fmt.Println(numberMap)  // map[1:one 2:two]
    
    numberMap[3] = "three"
    fmt.Println(numberMap)  // map[1:one 2:two 3:three]
    numberMap[3] = "삼"
    fmt.Println(numberMap)  // map[1:one 2:two 3:삼]
    ```

  - 요소를 제거할 때 → delete() 함수 사용

  ```go
  delete(numberMap, 3)
  fmt.Println(numberMap)  // map[1:one 2:two]
  ```