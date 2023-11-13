#### 관련 질문
- HTTP가 무엇인가요?
- HTTP Request Method 중 GET과 POST를 비교하여 설명해주세요.
- HTTP Status Code에 대해 설명해주세요.

## 개념
- HTTP는 HyperText Transfer Protocol의 약자로, 서버-클라이언트 모델을 기반으로 Request와 Response를 통해 정보를 주고받을 수 있는 웹 통신 프로토콜입니다.
### 설명
- 클라이언트에서 HTTP Request를 보내면 서버는 HTTP response를 클라이언트에 보내는 방식입니다.
#### [HTTP Request 구조]
1. METHOD, 경로Path, HTTP version
2. Header
3. Body
#### [HTTP Response 구조]
1. HTTP version, 상태 코드Status Code, 상태 메시지Status message
2. Header
3. Body

## 특징
### 1. 무연결성 Connectionless
- HTTP는 서버에 요청을 보내고 응답을 받으면 연결을 끊어버리는 Connectionless 특성을 갖습니다.
- 많은 사람들이 요청을 보내더라도 동시 접속을 최소화하여 더 많은 요청을 처리할 수 있습니다.
### 2. 무상태성 Stateless
- 연결을 유지하지 않기 때문에 클라이언트의 이전 상태를 알 수 없는 Stateless 특성을 갖습니다.

## HTTP Request Method
#### GET
- GET을 사용하여 해당 리소스를 조회하고 접근하여 도큐먼트에 대한 자세한 정보를 가져옵니다.
#### POST
- POST를 사용하여 서버에 데이터를 전송하여 새로운 리소스를 생성합니다.
- 동일한 POST 요청을 여러번 전송하면 동일한 리소스가 여러번 생성되는 부작용이 있습니다.
#### PUT
- PUT을 사용하여 리소스를 만들거나 기존 리소스를 업데이트합니다. 기존 리소스를 대체하거나 기존 리소스에 새로운 내용을 추가합니다.
- 동일한 PUT 요청을 여러번 전송해도 그 결과는 항상 동일합니다.
#### PATCH
- PATCH를 사용하여 기존 리소스의 일부분을 업데이트합니다.
- 동일한 PATCH 요청을 여러번 전송하는 경우 결과가 항상 동일하지 않을 수 있습니다.
#### DELETE
- DELETE를 사용하여 리소스를 삭제합니다.

## HTTP Response Status Code

### 2XX 요청 성공 관련
|상태코드|의미|설명|
|:-:|:-:|-|
|200| OK|클라이언트의 요청을 정상적으로 수행|
|201| Created|요청을 성공적으로 수행하여 새로운 리소스 생성|

### 3XX 리다이렉션 관련
|상태코드|의미|설명|
|:-:|:-:|-|
|301|Moved Permanently|요청한 리소스의 URI가 변경된 경우 변경된 URI를 응답해야 함|
|304|Not Modified|캐시 목적으로 사용되며, 응답이 수정되지 않아 계속해서 캐시된 응답을 사용함을 의미|

### 4XX 클라이언트 에러 관련
|상태코드|의미|설명|
|:-:|:-:|-|
|400|Bad Request|클라이언트의 요청이 부적절한 경우|
|401|Unauthorized|인증되지 않은 클라이언트가 보호된 리소스를 요청한 경우|
|403|Forbidden|접근할 권리가 없는 리소스를 요청한 경우|
|404|Not Found|요청받은 리소스를 찾을 수 없는 경우|
|405|Method Not Allowed|해당 리소스에서 불가능한 METHOD로 요청한 경우|

### 5XX 서버 에러 관련
|상태코드|의미|설명|
|:-:|:-:|:-:|
|500|Internal Server Error|요청에 대해 서버가 처리 방법을 모르는 경우|
|502|Bad Gateway|서버에서 예상하지 못한 에러가 발생한 경우 <br /> (ex. 예외 처리를 하지 않은 오류 발생)|

## 참고 자료
- [MDN HTTP 상태 코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)