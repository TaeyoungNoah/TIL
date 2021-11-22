# HTTP 상태코드

### 상태코드

> 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능

- 1xx (Informational): 요청이 수신되어 처리중
- 2xx (Successful): 요청 정상 처리
- 3xx (Redirection): 요청을 완료하려면 추가 행도 필요
- 4xx (Client Error): 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함 



#### 만약 모르는 상태 코드가 나타나면?

- 클라이언트가 인식할 수 없는 상태코드를 서버가 반환하면?
- 클라이언트는 상위 상태코드로 해석해서 처리
- 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨
- ex)

> - 299 ??? → 2xx (Successful)
> - 451 ??? → 4xx (Client Error)
> - 599 ??? → 5xx (Server Error)



***



### 1xx (Informational)

> 요청이 수신되어 처리중

- 거의 사용하지 않으므로 생략



***



### 2xx (Successful)

> 클라이언트의 요청을 성공적으로 처리

- 200 OK
- 201 Created

- 202 Accepted
- 204 No Content



#### 200 OK

> 요청
>
> GET /members/100 HTTP/1.1
>
> Host: localhost:8080
>
>  

> 응답
>
> HTTP/1.1 **200 OK**
>
> Content-Type: application/json
>
> Content-Length: 34
>
>  
>
> {
>
> ​	"username": "young",
>
> ​	"age": 20
>
> }



#### 201 Created

> 요청 성공해서 새로운 리소스가 생성됨

> 요청
>
> POST /members HTTP/1.1
>
> Content-Type: application/json
>
>  
>
> {
>
> ​	"username": "young",
>
> ​	"age": 20
>
> }

> 응답
>
> HTTP/1.1 **201 Created**
>
> Content-Type: application/json
>
> Content-Length: 34
>
> **Location: /members/100** (생성된 리소스는 응답의 Location 헤더 필드로 식별)
>
>  
>
> {
>
> ​	"username": "young",
>
> ​	"age": 20
>
> }



#### 202 Accepted

> 요청이 접수되었으나 처리가 완료되지 않음

- 배치 처리 같은 곳에서 사용

> ex) 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리함



#### 204 No Content

> 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음

- ex) 웹 문서 편집기에서 save 버튼 (굳이 응답으로 매번 결과를 받을 필요가 없음)
- save 버턴의 결과로 아무 내용이 없어도 된다
- save 버튼을 눌러도 같은 화면을 유지해야 한다
- 결과 내용이 없어도 204 메시지 (2xx)만으로 성공을 인식할 수 있다



***



### 3xx (Redirection)

> 요청을 완료하기 위해 유저 에이전트의 추가 조치 필요

- 300 Multiple Choices
- 301 Moved Permanently
- 302 Found
- 303 See Other
- 304 Not Modified
- 307 Temporary Redirect
- 308 Permanent Redirect



#### 리다이렉션 이해

- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동 (리다이렉트)
- 자동 리다이렉트 흐름

> 1. 요청
>
> GET **/event** HTTP/1.1
>
> Host: localhost:8080
>
>  
>
> 2. 응답
>
> HTTP/1.1 301 Moved Permanently
>
> **Location: /new-event**
>
>  
>
> 3. **자동 리다이렉트**
> 4. 요청
>
> GET **/new-event** HTTP/1.1
>
> Host: localhost:8080
>
>  
>
> 5. 응답
>
> HTTP/1.1 200 OK
>
> ....

**[종류]**

- 영구 리다이렉션 - 특정 리소스의 URI가 영구적으로 이동

> - ex) /members → /users
> - ex) /evemt  → /new-event

- 일시 리다이렉션 - 일시적인 변경

> - 주문 완료 후 주문 내역 화면으로 이동
> - PRG: Post/Redirct/Get

- 특수 리다이렉션

> - 결과 대신 캐시를 사용



#### [영구 리다이렉션]

> 301, 308

- 리소스의 URI가 영구적으로 이동
- 원래의 URL을 사용X, 검색 엔진 등에서도 변경 인지
- **301 Moved Permanently**

> - **리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(거의)**

- **308 Permanent Redirect**

> - 301과 기능은 같음
> - **리다이렉트시 요청 메서드와 본문 유지 (처음 POST를 보내면 리다이렉트도 POST 유지)**



**[영구 리다이렉션 - 301]**

1. 요청 (URL: /event)

> **POST** /event HTTP/1.1
>
> Host: localhost:8080
>
>  
>
> **name=hello&age=20** (**메시지 존재**)

2. 응답

> HTTP/1.1 301 Moved Permanently
>
> **Location: /new-event**

3. 자동 리다이렉트 (**GET으로 변경**)

4. 요청 (URL: /new-event)

> **GET** /new-event HTTP/1.1
>
> Host: localhost:8080
>
>  
>
> (**메시지 제거: 등록하려고했던 정보를 다시 입력해야함ㅠ)**

5. 응답

> HTTP/1.1 200 OK
>
> ....



**[영구 리다이렉션 - 308]**

1. 요청 (URL: /event)

> **POST** /event HTTP/1.1
>
> Host: localhost:8080
>
>  
>
> **name=hello&age=20** (**메시지 존재**)

2. 응답

> HTTP/1.1 301 Moved Permanently
>
> **Location: /new-event**

3. 자동 리다이렉트 (**POST 유지**)

4. 요청 (URL: /new-event)

> **POST** /new-event HTTP/1.1
>
> Host: localhost:8080
>
>  
>
> **name=hello&age=20** (**메시지 유지 : 301의 번거로운 문제점 해결 / **
>
> **하지만 실무적으로 보았을 때는 어차피 새로운 경로로 바뀌면 입력해야하는 내부 정보도 다 바뀌게 되어있음. 그렇기때문에 실무에서는 거의 다 301로 진행)**

5. 응답

> HTTP/1.1 200 OK
>
> ....



#### [일시적인 리다이렉션]

> 302, 307, 303

