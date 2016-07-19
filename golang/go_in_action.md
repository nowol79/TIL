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
- 패키지 내에서만 접근 가능하도록 하려면 소문자 변수 사용, 외부에서 참고하려면 대분자로 시작하는 변수 사용
- Go의 모든 변수들은 값에 의해 전달된다. (call by value만 존재한다.)
- defer로 예약된 작업은 함수가 panic 상태에 빠져 예상치 못하게 종료되더라도 반드시 실행된다.
- Go routine으로 많이 사용하는 익명함수의 경우 closure 함수와 유사한 기능을 한다. 
- 빈 구조체 타입
   - 타입은 필요하지만 타입의 상태를 관리할 필요가 없는 경우에 유용하다.
   ```
   type AAA struct {}
   ```

# 참고
- [golang](https://golang.org)
- [gopheracademy blog](https://blog.gopheracademy.com/)

# golang 유틸
- [orgalorg](https://github.com/reconquest/orgalorg)
- [caddy http/2 webserver](https://github.com/mholt/caddy)
- [awesome go](https://github.com/avelino/awesome-go)
