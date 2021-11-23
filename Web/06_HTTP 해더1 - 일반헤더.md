# HTTP 헤더1 - 일반헤더

### HTTP 헤더

- header-field = field-name ":" OWS field-value OWS  (OWS: 띄어쓰기 허용)

- field-name은 대소문자 구문 없음

> GET /search?q=hello&hl=ko HTTP/1.1
>
> **Host: www.google.com**

> HTTP/1.1 200 OK
>
> **Content-Type: text/html;charset=UTF-8**
>
> **Content-Length: 3434**
>
>  
>
> <html>
>
> ​	<body>...</body>
>
> </html>



**[용도]**

- HTTP 전송에 필요한 모든 부가정보

> ex) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버정보, 캐시관리정보 ...

- 표준 헤더가 매우 많음

> https://en.wikipedia.org/wiki/List_of_HTTP_header_fields

- 필요시 임의의 헤더 추가 기능

> helloworld: hihi



**[HTTP 헤더]**

> 분류 - RFC2616(과거)

- 헤더 분류

> - General 헤더: 메시지 전체에 적용되는 정보, ex) Connection: else
> - Request 헤더: 요청 정보, ex) User-Agent: Mozilla/5.0 (Macintosh; ..)
> - Response 헤더: 응답 정보, ex) Server: Apache
> - Entity 헤더: 엔티티 바디 정보, ex) Content-Type: text/html, Content-Length: 3423

- 메시지 본문(message body)은 엔티티 본문(entity body)을 전달하는데 사용
- 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
- 엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보 제공

> - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등

>HTTP/1.1 200 OK
>
>Content-Type: text/html;charset=UTF-8
>
>Content-Length: 3434   (엔티티 헤더)
>
> 
>
><html>
>
>​	<body>...</body>
>
></html> (메시지 본문, 엔티티 본문)

- RFC2616 폐기 → 2014 RFC7230~7235 등장

- RFC723x 변화

> - 엔티티(Entity) → 표현(Representation)
> - Representation = representation Metadata + Representation Data
> - 표현 = 표현 메타데이터 + 표현 데이터



**[HTTP BODY]**

> message body - RFC7230 (최신)

- 메시지 본문(message body)을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드(payload)
- **표현**은 요청이나 응답에서 전달할 실제 데이터
- **표현 헤더는 표현 데이터**를 해석할 수 있는 정보 제공

> - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등

- 참고: 표현 헤더는 표현 메타데이터와, 페이로드 메시지를 구분해야 하지만, 여기서는 생략

> HTTP/1.1 200 OK
>
> Content-Type: text/html;charset=UTF-8
>
> Content-Length: 3434  (표현 헤더)
>
>  
>
> <html>
>
> ​	<body>...</body>
>
> </html> (메시지 본문, 표현 데이터)



***



### 표현

- Content-Type: 표현 데이터의 형식
- Content-Encoding: 표현 데이터의 압축 방식
- Content-Language: 표현 데이터의 자연 언어
- Content-Length: 표현 데이터의 길이

- **표현 헤더는 전송, 응답 둘 다 사용**



#### Content-Type

> 표현 데이터의 형식

> HTTP/1.1 200 OK
>
> **Content-Type: text/html;charset=UTF-8**
>
> Content-Length: 3434 
>
>  
>
> <html>
>
> ​	<body>...</body>
>
> </html>

- 미디어 타입, 문자 인코딩

- ex)

> - text/html; charset=utf-8
> - application/json
> - image/png





#### Content-Encoding

> 표현 데이터 인코딩

> HTTP/1.1 200 OK
>
> Content-Type: text/html;charset=UTF-8
>
> **Content-Encoding: gzip**
>
> Content-Length: 3434  (표현 헤더)
>
>  
>
> ....

- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
- ex)

> - gzip
>
> - deflate
>
> - identity





#### Content-Language

> 표현 데이터의 자연 언어

> HTTP/1.1 200 OK
>
> Content-Type: text/html;charset=UTF-8
>
> **Content-Language: ko**
>
> Content-Length: 3434  (표현 헤더)
>
>  
>
> ....

- 표현 데이터의 자연 언어를 표현
- ex)

> - ko
> - en
> - en-US





#### Content-Length

> 표현 데이터의 길이

> HTTP/1.1 200 OK
>
> Content-Type: text/html;charset=UTF-8
>
> **Content-Length: 5**
>
>  
>
> hello

- 바이트 단위

- Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨 (이후 설명)



***



### 콘텐츠 협상

> 협상 (콘텐츠 네고시에이션)
>
> 클라이언트가 선호하는 표현 요청

- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어

- **협상 헤더는 요청시에만 사용**

- ex)

> 클라이언트
>
> GET /event
>
> Accept-Language: ko
>
>  
>
> (만약 서버가 한국어를 지원하는 서버라면 한국어 값을 줄 것임. /
>
> 이때 생길 수 있는 문제는 만일 한국어를 지원하지 않는 서버에서 위 메시지를 받으면
>
> 우리에게 그나마 더 익숙한 언어를 줬으면 좋겠는데 그렇지 않음)



#### 협상과 우선순위1

> Quality Values(q)

- Quality Values(q) 값 사용
- 0~1, **클수록 높은 우선순위**
- 생략하면 1
- Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7

> 1. ko-KR;q=1 (생략)
> 1. ko;q=0.9
> 1. en-US;q=0.8
> 1. en;q=0.7
>
> 이렇게 작성해서 보내면 위에 문제에서 그나마 더 익숙한 언어를 보내주게 됨

<img src="/Users/taeyoung/Library/Application Support/typora-user-images/image-20211123112200393.png" alt="image-20211123112200393" style="zoom: 33%;" />



#### 협상과 우선순위2

> Quality Values(q)

- 구체적인 것이 우선한다.
- Accept: **text/*, text/plain, text/plain;format=flowed, * / ***  (* / * 원래 공백 없는데 타이포라에서 사라짐,,)

> - text/*, text/plain, text/plain;format=flowed
> - text/plain
> - text/*
> - */ * (원래 공백 없음)



#### 협상과 우선순위3

> Quality Values(q)

- 구체적인 것을 기준으로 미디어 타입을 맞춘다
- Accept: text/*;q=0.3, text/html;q=0.7, text/html;level=1, text/html;level=2;q=0.4, * / *;q=0.5



***



### 전송 방식 설명

> Transfer-Encoding / Range, Content-Range

- 단순 전송
- 압축 전송
- 분할 전송
- 범위 전송



#### 단순 전송

> Content-Length

> GET /event

> HTTP/1.1 200 OK
>
> Content-Type: text/html;charset=UTF-8
>
> **Content-Length: 3434** 
>
>  
>
> <html>
>
> ​	<body>...</body>
>
> </html>





#### 압축 전송

> Content-Encoding (용량이 많이 줄어듦)

> GET /event

> HTTP/1.1 200 OK
>
> Content-Type: text/html;charset=UTF-8
>
> **Content-Encoding: gzip**
>
> Content-Length: 521
>
>  
>
> ....





#### 분할 전송

> Transfer-Encoding (쪼개서 보내기, 받아보는대로 표시 가능)
>
> 이때 Content-Length는 보내면 안됨

> GET /event

> HTTP/1.1 200 OK
>
> Content-Type: text/text
>
> **Transfer-Encoding: chunked** 
>
>  
>
> 5
>
> Hello (묶음 1)
>
> 5
>
> World (묶음 2)
>
> 0
>
> /r/n (묶음 3)





#### 범위 전송

> Range, Content-Range
>
> 예를 들어 무언가를 전송받다가 중간에 끊김, 굳이 처음부터 받을 필요 없이 끊긴 지점 부터만 요청해도 될 때 사용

> GET /event
>
> Range: bytes=1001-2000
>
>  

> HTTP/1.1 200 OK
>
> Content-Type: text/text
>
> **Content-Range: bytes 1001-2000 /2000**
>
>  
>
> ...........



***



### 일반 정보

- **From**: 유저 에이전트의 이메일 정보

> - 일반적으로 잘 사용되지 않음
> - 검색 엔진 같은 곳에서 주로 사용
> - **요청에서 사용**



- **Referer**: 이전 웹 페이지 주소 **(진짜 많이 씀)**

> - 현재 요청된 페이지의 이전 웹 페이지 주소
> - A → B 로 이동하는 경우 B를 요청할 때 Referer:A 를 포함해서 요청
> - Referer를 사용해서 유입 경로 분석 가능
> - **요청에서 사용**
> - 참고: referer는 단어 referrer의 오타



- **User-Agent**: 유저 에이전트 애플리케이션 정보

> - user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
> - 클라이언트의 애플리케이션 정보(웹 브라우저 정보, 등등)
> - 통계 정보
> - 어떤 종류의 브라우저에서 장애가 발생하는지 파악이 가능 (개발자 입장에서 편함)
> - **요청에서 사용**



- **Server**: 요청을 처리하는 오리진 서버의 소프트웨어 정보

> - Server: Apache/2.2.22 (Debian)
> - server: nginx
> - **응답에서 사용**



- **Date**: 메시지가 생성된 날짜

> - Date: Tue, 15 Nov 1994 08:12:32 GMT
> - **응답에서 사용**



***



### 특별한 정보

- **Host**: 요청한 호스트 정보(도메인) 

> - 요청에서 사용
> - **필수** (매우 중요)
> - 하나의 서버가 여러 도메인을 처리해야 할 때
> - 하나의 IP 주소에 여러 도메인에 적용되어있을 때
>
> > - 하나의 서버 안에 여러 도메인을 한번에 처리할수 있는 실제 애플리케션이 여러개 구동될 수 있음
> > - 클라이언트가 GET /hello HTTP/1.1 로 요청을 하면 이게 어떤 도메인에서 실행해야할지 알 수 없음 (IP로만 통신을 하기 때문)
> > - 그래서 
> >
> > > GET /hello HTTP/1.1
> > >
> > > **Host: aaa.com**
> >
> > 으로 Host를 포함하여 요청하면 서버에서 받아서 도메인을 결정해 요청을 처리함



- **Location**: 페이지 리다이렉션

> - 웹 브라우저는 3xx 응답 결과에 Location 헤더가 있으면, Location 위치로 자동 이동 (리다이렉트)
> - 응답코드 3xx 에서 설명
> - 201(Created): Location 값은 요청에 의해 생성된 리소스 URI
> - 3xx(Redirection): Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴



- **Allow**: 허용 가능한 HTTP 메서드 (이런게 있구나 참고정도)

> - 405 (Method Not Allowed) 에서 응답에 포함해야함
> - Allow: GET, HEAD, PUT



- **Retry-After**: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

> - 503 (Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
> - Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
> - Retry-After: 120 (초단위 표기)



***



### 인증

- Authorization: 클라이언트 인증 정보를 서버에 전달

> - Authorization: Basic xxxxxxxxxxxxxx (인증과 관련된 값)



- WWW-Authenticate: 리소스 접근시 필ㄹ요한 인증 방법 정의

> - 리소스 접근시 필요한 인증 방법 정의
>
> - 401 Unauthorized 응답과 함께 사용
>
> - WWW-Authenticate: Newauth realm="apps", type=1 
>
>   ​                                     title="Login to (백슬레쉬)"apps(백슬레쉬)"", Basic realm="simple"



***



### 쿠키

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달



#### 쿠키 미사용

- 처음 welcome 페이지 접근

> (웹브라우저)
>
> GET /welcome HTTP/1.1
>
>  

> (서버)
>
> HTTP/1.1 200 OK
>
>  
>
> 안녕하세요. 손님

- 로그인

> (웹 브라우저)
>
> POST /login HTTP/1.1
>
> user=홍길동
>
>  

> (서버)
>
> HTTP/1.1 200 OK
>
>  
>
> 홍길동님이 로그인했습니다.

- 로그인 이후 welcome 페이지 접근

> (웹 브라우저)
>
> GET /welcome HTTP/1.1
>
>  

> (서버)
>
> HTTP/1.1 200 OK
>
>  
>
> 안녕하세요. **손님** (홍길동이 로그인을 했지만, 이 메시지에서는 홍길동인지 알아볼 길이 없음 /
>
> Why? HTTP는 **무상태 프로토콜**이기 때문)

**Stateless**

- HTTP는 무상태(Stateless) 프로토콜이다.
- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다.
- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.
- 클라이언트와 서버는 서로 상태를 유지하지 않는다.

> 위의 이유때문에 위 예시에서 로그인 이후 welcome페이지에 접근했을 때 맨 처음과 같은 응답이 나옴
>
> [해결방안 1]
>
> 모든 요청과 링크에 사용자 정보를 포함한다. → 사실상 불가능에 가까움
>
> - 모든 됴청에 사용자 정보가 포함되도록 개발 해야함(개발자 죽어남)
> - 브라우저를 완전히 종료하고 다시 열면? (가능은 하지만 너무 번거롭다)
>
> [해결방안 2]
>
> - **쿠키 사용**



#### 쿠키 사용

- 로그인

> (웹 브라우저)
>
> POST /login HTTP/1.1
>
> user=홍길동 
>
>  

> (서버)
>
> HTTP/1.1 200 OK
>
> Set-Cookie: user=홍길동 
>
>  
>
> 홍길동님이 로그인했습니다.

(쿠키 헤더로 웹 브라우저에 보내면 웹 브라우저에 있는 쿠키 저장소에 저장됨)

- 로그인 이후 welcome 페이지 접근

  그러면 웹 브라우저는 요청을 보낼때마다 쿠키 저장소를 무조건 뒤져서 쿠키 값을 무조건 꺼내서 쿠키 헤더를 만들어 서버로 보냄

> (웹 브라우저)
>
> GET /welcome HTTP/1.1
>
> Cookie: user=홍길동 
>
>  

> (서버)
>
> HTTP/1.1 200 OK
>
>  
>
> 안녕하세요. **홍길동님** 

**쿠키**

> 모든 요청에 쿠키 정보 **자동 포함**

- ex) set-cookie: **sessionId=abcde1234**; **expires**=Sat, 26-Dec-2020 00:00:00 GMT; **path=/;** **domain**=.google.com; **Secure**

- 사용처

> - 사용자 로그인 세션 관리 
>
> > 위 예시에서 '홍길동'을 바로 저장하지 않고 서버에서 세션key를 만들어서 서버에 저장해두고 그 세션id를 응답으로 보내줌. 나중에 세션id를 웹 브라우저에서 보내면 그걸 조회해서 홍길동과 매치시킴
>
> - 광고 정보 트래킹

- 쿠키 정보는 항상 서버에 전송됨

> - 네트워크 트래픽 추가 유발
> - 최소한의 정보만 사용(세선 id, 인증 토큰)
> - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage) 참고
>
> > 매번 귀찮고, 서버 너가 갖고있다가 써줘!

- 주의!

> - 보안에 민감한 데이터는 저장하면 안됨 (주민번호, 신용카드 번호 ...)



#### 쿠키 - 생명주기

> Expires, max-age

- Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT (GMT 기준으로 넣어줘야함)

> - 만료일이 되면 쿠키 삭제

- Set-Cookie: max-age=3600 (3600초)

> - 0이나 음수를 지정하면 쿠키 삭제

- 세션 쿠키: 만료 날짜를 색략하면 브라우저 종료시 까지만 유지
- 영속 쿠키: 민료 날짜를 입력하면 해당 날짜까지 유지



#### 쿠키 - 도메인

> Domain

- ex) domain=example.org
- **명시: 명시한 문서 기준 도메인 + 서브 도메인 포함**

> - domain=example.org를 지정해서 쿠키 생성
>
> > - example.org는 물론이고
> > - dev.example.org도 쿠키 접근

- **생략: 현재 문서 기준 도메인만 적용**

> - example.org 에서 쿠키를 생성하고 domain 지정을 생략
>
> > - example.org 에서만 쿠키 접근
> > - dev.example.org 는 쿠키 미접근



#### 쿠키 - 경로

> Path

- ex) path=/home
- **이 경로를 포함한 하위 경로 페이지만 쿠키 접근**
- **일반적으로 path=/ 루트로 지정**
- ex)

> - **path=/home 지정**
> - /home → 가능
> - /home/level1 → 가능
> - /home/level1/level2 → 가능
> - /hello → 불가능



#### 쿠키 - 보안

> Secure, HttpOnly, SameSite

- Secure

> - 쿠키는 http, https를 구분하지 않고 전송
> - Secure를 https인 경우에만 전송

- HttpOnly

> - XSS 공격 방지
> - 자바스크립트에서 접근 불가(document.cookie)
> - HTTP 전송에만 사용

- SameSite

> - XSRF 공격 방지
> - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송



