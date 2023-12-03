## HTTP
- HyperText Transfer Protocol
- 웹 상에서 정보를 전송하기 위한 통신 프로토콜
- 서버-클라이언트 모델을
- 클라이언트에서 HTTP Request를 보내면 서버는 HTTP response를 클라이언트에 보내는 방식
#### Request
1. Start Line: Method, Path, HTTP version
2. Header
3. Body

#### Response
1. Status Line: HTTP version, Status Code, Status message
2. Header
3. Body

## 특징
### 1. 무연결성 Connectionless
- 서버에 요청하고 응답을 받으면 연결을 끊는다.
- 동시 접속을 최소화하여 많은 요청 처리 가능
### 2. 무상태성 Stateless
- 연결을 유지하지 않기 때문에 클라이언트의 이전 상태를 알 수 없다. (ex. 로그인 여부)
- 이를 보완하기 위해 쿠키, 세션, 토큰 기반 인증 방식 도입

## HTTP Request Method
### 1. GET
- **리소스를 조회**할 때 사용
	- 도큐먼트에 대한 자세한 정보
- 정보 전달 위치: Body 없이 URL에 **Query String** 추가 → 리소스를 특정할 수 있는 정보 전달
- 캐시 가능: Query String을 포함하여 브라우저 히스토리에 캐시
### 2. POST
- **리소스를 생성**할 때: 서버에 데이터를 전송하여 새로운 리소스를 생성
- 요청한 데이터를 처리할 때
- 동일한 POST 요청을 여러번 전송 시 동일한 리소스가 여러번 생성된다.
- **Body**로 정보 전달
### 3. PUT
- **리소스 전체** 수정
	- 기존 리소스 대체
	- 이전에 해당 리소스가 없으면 리소스 생성
- 동일한 PUT 요청을 여러번 전송해도 항상 동일한 결과 (멱등성)
### 4. PATCH
- **기존 리소스의 일부분** 수정
- 동일한 PATCH 요청을 여러번 전송하는 경우 결과가 항상 동일하지 않을 수 있다.
### 5. DELETE
- 리소스 삭제

## HTTP Response Status Code
- 클라이언트가 보낸 HTTP 요청에 대한 서버의 응답 상태 코드
- HTTP 요청에 대한 상태 (성공/실패 여부)
- 100번대부터 500번대까지 5개의 클래스로 분류

### 1XX 정보 관련
- 클라이언트의 요청을 받고 작업을 계속 진행하는 경우

### 2XX 요청 성공 관련
- 클라이언트가 요청한 작업을 성공적으로 처리한 경우

|상태코드|의미|설명|
|:-:|:-:|-|
|200| OK|클라이언트의 요청을 정상적으로 수행|
|201| Created|요청을 성공적으로 수행하여 새로운 리소스 생성|

### 3XX 리다이렉션 관련
- 요청을 완료하기 위해 필요한 추가 작업 조치가 필요한 경우

|상태코드|의미|설명|
|:-:|:-:|-|
|301|Moved Permanently|요청한 리소스의 URI가 변경된 경우 변경된 URI를 응답해야 함|
|304|Not Modified|캐시 목적으로 사용되며, 응답이 수정되지 않아 계속해서 캐시된 응답을 사용함을 의미|

### 4XX 클라이언트 에러 관련
- 클라이언트 요청에 문제가 있는 경우

|상태코드|의미|설명|
|:-:|:-:|-|
|400|Bad Request|클라이언트의 요청이 부적절한 경우 <br /> (ex. 올바르지 않은 데이터 형식으로 요청한 경우)|
|401|Unauthorized|인증되지 않은 클라이언트가 인증이 필요한 리소스를 요청한 경우|
|403|Forbidden|클라이언트가 인증된 상태에서 접근할 권한이 없는 리소스를 요청한 경우|
|404|Not Found|요청받은 리소스를 찾을 수 없는 경우|
|405|Method Not Allowed|해당 리소스에서 불가능한 METHOD로 요청한 경우|

### 5XX 서버 에러 관련
- 클라이언트의 요청은 유효하나, 서버가 작업 수행에 실패한 경우

|상태코드|의미|설명|
|:-:|:-:|:-:|
|500|Internal Server Error|요청에 대해 서버가 처리 방법을 모르는 경우|
|502|Bad Gateway|서버에서 예상하지 못한 에러가 발생한 경우 <br /> (ex. 예외 처리를 하지 않은 오류 발생)|

# HTTPS
- HTTP 통신에서 데이터 유출 문제를 보완하기 위해 암호화를 추가한 프로토콜

## 참고 자료
- [MDN HTTP 상태 코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
#### 관련 질문
- HTTP가 무엇인가요?
- HTTP의 구조와 특징에 대해 설명해주세요.
- HTTP Request Method 중 GET과 POST를 비교하여 설명해주세요.
- HTTP Request Method 중 PUT과 PATCH를 비교하여 설명해주세요.
- HTTP Status Code에 대해 설명해주세요.
- HTTPS에 대해 설명해주세요.