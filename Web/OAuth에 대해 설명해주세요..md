## 개념
- Open Authorization
- **접근 권한 위임**을 위한 **개방형 표준 프로토콜** (표준화된 인증 방식)
- 사용자가 어떤 웹사이트 상의 자신의 정보에 대해 다른 애플리케이션의 **접근 권한을 부여**하기 위해 사용
- 다른 애플리케이션에서 유저의 계정 정보 공유

## 구성 요소
### 1. 사용자 User
- 서비스 제공자와 클라이언트 서비스를 사용하는 계정을 가지고 있는 개인
- Resource Owner
### 2. 클라이언트 Client Application
- OAuth를 사용하여 서비스 제공자에게 요청하는 애플리케이션
### 3. 서비스 제공자 Service Provider
- OAuth를 통해 접근을 지원하는 Open API를 제공하는 애플리케이션
- Authorization & Resource Server
### 4. Client Secret
- 서비스 제공자에게 클라이언트를 인증하기 위한 키Key
### 5. 접근 요청 코드 Access Code
- 클라이언트가 유저에게 **접근 권한을 인증받기 위해** 필요한 정보가 담긴 코드
### 6. 접근 토큰Access Token
- 유저에게 **접근 권한을 인증받은 후** 클라이언트가 유저의 정보에 접근하기 위한 키를 포함한 값

## 인증 방식
1. 클라이언트에서 서비스 제공자로 이동하여 사용자 인증을 수행 (ID/Password 입력)
2. 서비스 제공자로부터 유저에게 Access Code 발급
3. 서비스 제공자에서 redirect_uri로 등록해둔 클라이언트로 이동
	- 서비스 제공자가 발급한 Access Code가 URI에 포함된다.
4. 클라이언트가 Access Token 요청
5. 서비스 제공자가 Access Token을 발급
6. 클라이언트는 발급된 Access Token을 이용하여 유저 정보 접근 요청 가능

## 참고 자료
- [OAuth](https://ko.wikipedia.org/wiki/OAuth)
- [OAuth 2.0 개념과 동작원리](https://hudi.blog/oauth-2.0/)