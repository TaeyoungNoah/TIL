# URI와 웹 브라우저 요청 흐름

### URI(Uniform Resourse Identifier)

> URI 는 로케이터(**L**ocator), 이름(**N**ame) 또는 둘 다 추가로 분류될 수 있다
>
> URI(Resourse를 식별한다) 안에 URL(Resourse가 어디에 있는지), URN(Resourse의 이름이 뭔지)(잘 안씀) 이 있다



#### [URI 단어 뜻]

- Uniform : 리소스 식별하는 통일된 방식
- Resourse : 자원, URL로 식별할 수 있는 모든 것(제한 X)
- Identifier : 다른 항목과 구분하는데 필요한 정보



#### [URL, URN 단어 뜻]

- URL - Locator : 리소스가 있는 위치를 지정
- URN - Name : 리솟에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않는다.
- urn:isbn:83849834 (어떤 책의 isns URN)
- URN 이름만으로 실제 리소스를 찾을 수 있는 방법 보편화 X
- **앞으로 URI를 URL과 같은 의미로 간주**



#### [URL 전체 문법]

- scheme://[userinfo@]host[:port][/path][?query][#fragment]

- https://www.google.com:433/search?q=hello&hI=ko



- scheme

> - 주로 프로토콜(= 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙) 사용
> - 프로토콜 ex) http, https(= HTTP Secure : http에 보안 추가), ftp
> - http는 80포트, https는 443 포트를 주로 사용 (포트는 생략 가능)

- userinfo

> URL에 사용자정보를 포함해서 인증
>
> 거의 사용 X

- host

> 호스트 명
>
> 도메인명 또는 IP주소를 직접 사용 가능

- PORT

> 접속 포트
>
> 일반적으로 생략

- path

> 리소스의 경로, 계층적 구조
>
> /home/file1.jpg     or    /members/100

- query

> key=value 형태
>
> ? 시작, &로 파라미터 추가 가능 ?keyA=valueA&keyB=valueB
>
> query parameter, query string 등으로 불림, 웹서버에 제공하는 파라미터, 문자형태



***



### 웹 브라우저 요청 흐름

> https://www.google.com:433/search?q=hello&hI=ko



https://**www.google.com**:433/

1. DNS 조회, PORT 조회

2. HTTP 요청 메시지 생성

> GET /search?q=hello&hI=ko HTTP/1.1
>
> Host: www.google.com

3. SOCKET 라이브러리를 통해 전달

> - A : TCP/IP 연결 (IP,PORT)
>
> - B : 데이터 전달

4. TCP/IP 패킷 생성, HTTP 메시지 포함

5. 전달!

6. 구글 서버에 도착하면 껍데기를 다 까서 **HTTP메시지를 얻어내 해석을 함**
7. HTTP 응답 메시지

> HTTP/1.1 200 OK
>
> Content-Type : text/html;charset=UTF-8
>
> Content-Length : 3423
>
>  
>
> <html>
>
> ​	<body>...</body>
>
> </html>

8. 전달!
9. 나에게 도착하면 껍데기를 다 까서 **HTTP 응답 메시지를 얻어내 해석을 함**
