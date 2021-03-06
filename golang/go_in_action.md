# 저자
- [William Kennedy](https://www.goinggo.net/)
- [Brian Ketelsen](https://brianketelsen.com/about)
- [Erik St. Martin](https://github.com/erikstmartin)

# 건더기
- 이 책을 통해서 golang의 철학이 배우고 싶다. 왜.. 만들어 졌는지..어떤 목적으로 구글은 프로그래밍 언어를 다시 쓸 생각을 했는지..
- 기존 언어로는 풀지 못하는 장벽이 있었을 듯..

## 1장
- 타입, 함수, 상수 및 인터페이스 존재
- init() 함수는 main 함수가 호출되기 이전에 호출된다.
- import 시 빈 식별자('_')를 사용할 경우 import패키지의 init()함수만 호출한다. 

  ```
   import (
      _"github.com/nowol79/test/aa"
   )
  ```
  
  패키지에 정의된 식별자를 직접적으로 사용하지 않더라도 가져오기 선언을 유지해야 할 경우 사용한다. 
  실제 main.go에서 해당 패키지의 변수를 사용하지 않으면 컴파일 에러는 낸다. 이때 빈 식별자를 활용한다. 
- 패키지 내에서만 접근 가능하도록 하려면 소문자 변수 사용, 외부에서 참고하려면 대분자로 시작하는 변수 사용
- Go의 모든 변수들은 값에 의해 전달된다. (call by value만 존재한다.)
- defer로 예약된 작업은 함수가 panic 상태에 빠져 예상치 못하게 종료되더라도 반드시 실행된다.
- Go routine으로 많이 사용하는 익명함수의 경우 closure 함수와 유사한 기능을 한다. 
- 빈 구조체 타입
   - 타입은 필요하지만 타입의 상태를 관리할 필요가 없는 경우에 유용하다.
   ```
   type AAA struct {}
   ```

## 2장
- RSS search 소스코드 분석을 통한 go 언어 

## 3장
- go 패키지
- go 도구들

## 4장
- go에서 제공하는 data collection 종류는 배열, slice, map 세 가지 데이터 구조를 제공
- 배열 : 동일한 타입의 원소가 연속된 블록에 저장되는 고정된 길이의 데이터
   - 함수에 배열을 전달하는 것은 메모리와 성능 면에서 높은 비용이 소모된다. 함수간 변수를 전달할 때는 항상 그 값이 전달
   - 따라서 배열 변수를 전달하는 것은 그 크기와 상관없이 배열 전체를 복사하여 함수에 전달하는 것을 의미
   - 따라서 배열변수를 함수에 전달할 때는 포인터로 전달, 다만 포인터로 전달되면 변경되는 값이 공유가 된다는 점 
- slice : 동적 배열의 개념, 필요에 따라 크기 늘리거나 줄일 수 있음
   - 내부구조 : slice는 세 개의 필드로 구성, 포인터+슬라이스 길이 + 최대용량
   - 생성 및 초기화
     - ```slice := make([]string, 5)``` (길이와 용량을 5개의 원소에 지정)
     - ```slice := make([]int, 3, 5)``` (길이는 3개, 최대 용량은 5개의 원소로 지정)
     - ```slice := make([]int, 5, 3)``` (길이보다 작은 크기의 용량을 가지는 배열은 생성할 수 없다.)
     - ```slice := []string{99:""}``` (길이 및 용량을 100으로 설정한 slice)
- 배열과 slice 선언 차이
   -```array := [3]int{10,20,30}``` vs ```slice := []int{10, 20, 30}```
   - []연산자에 값을 지정하면 배열이 생성되고, 지정하지 않으면 slice가 생성
- nil slice vs 빈 slice
   - ```var slice []int``` 초기화 코드를 선언하지 않으면 nil slice가 생성된다. 
     - ```| nil pointer | 0 - 길이 | 0 - 최대용량 |```
   - ```slice := make([]int, 0)``` or ```slice := []int{}``` 빈 정수 슬라이스
      - ```| 메모리주소 pointer | 0 - 길이 | 0 - 최대용량 |```
 - slice :=[]int{10,20,30,40,50}, newSlice := slice[1:3] 이걸 그림으로 나타낼 수 있어야 하는데, slice와 newSlice는 내부 5개자리 배열을 동일하게 참조한다. 다만 newSlice는 index 1부터 참조가 가능하고 길이는 2 용량은 4가 된다.
 - 따라서 newSlice는 index [0]값을 접근할 방법이 없다. 또한 slice, newSlice가 내부 배열을 공유 하기 때문에 slice의 변경이 newSlice에도 영향을 준다는 거
 - append 함수를 이용해 slice의 길이와 용량 확장하기
   ```slice := []int{10, 02, 30, 40}```
   ```newSlice := append(slice, 50)```
   - newslice 가 참조하는 내부 배열의 용량은 원래 크기의 두 배로 확장된다. 
   - append함수는 내부 배열의 용량을 확장할 때 1000보다 작은 경우 용량의 두 배, 1000개를 넘어서면 25%씩 확장된다. 
 - for range 키워드를 이용해서 슬라이스 반복하기
    ```
    slice := []int{10,20,30,40}
    
    for index, value := range slice {
      fmt.Printf("인덱스: %d 값: %d\n", index, value)
    }
    ```
   slice 반복문 range 키워드는 index, value 두 개 값 리턴, 중요한 것은 range 키워드가 레퍼런스값을 전달 하는게 아니고 복사본을 생성한다. 
   ```
   slice := []int{10, 20, 30, 40, 50}
   for index, value := range slice {
                fmt.Printf("value:%d address value: %x slice address: %x\n",
                        value, &value, &slice[index])
        }
   $ ./main
   value:10 address value: c82000a308 slice address: c82000e2a0
   value:20 address value: c82000a308 slice address: c82000e2a8
   value:30 address value: c82000a308 slice address: c82000e2b0
   value:40 address value: c82000a308 slice address: c82000e2b8
   value:50 address value: c82000a308 slice address: c82000e2c0

   ```
   예제에서 보면, range를 통해서 가져온 value의 값은 변해도 주소는 동일하다. 즉, ```index|value```로 구성된 range에 값과 인덱스를 복사해 넣는 것이다. 또한 value의 주소는 slice의 주소와는 별개로 따른 공간에 복사를 하는 것을 알 수 있다. 

## 5장. 

## 6장. 동시성
 - go에서 동시성이란 함수들을 다른 함수들과 독립적으로 실행할 수 있는 기능, 이때 함수를 고루틴으로 생성하면 이 함수를 야약된 후 논리 프로세스가 여력이 있을 때 실행되는 독립적인 작업단위로 취금. 
 - Go 런타임 스케쥴러 : 모든 고루틴을 관리/스케줄러는 운영체제를 기반으로 동작하며 운영체제의 스레드를 고루틴을 실행하는 논리 프로세서에 바인딩 된다. 
 - 스케줄러는 어떤 고루틴이어떤 논리 프로세서상에서 얼마나 오래 실행중인지를 추적하는데 필요한 모든 것들을 관리한다.
 - 프로세스와 쓰레드를 구분할 수 있나? 
   - 프로세스는 애플리케이션 실행 단위 (메모리, 핸들, 스레드..등의 자원을 포함하는 단위)
   - 쓰레드는 프로세스에서 cpu를 선점해서 동작하는 실행단위
   - 운영체제는 스레드를 물리적 프로세스에 등록, Go 런타임 스케줄러는 고루틴을 논리적 프로세스에 통해 실행할 수 있도록 등록

# 참고
- [golang](https://golang.org)
- [gopheracademy blog](https://blog.gopheracademy.com/)

# golang 유틸
- [orgalorg](https://github.com/reconquest/orgalorg)
- [caddy http/2 webserver](https://github.com/mholt/caddy)
- [awesome go](https://github.com/avelino/awesome-go)
