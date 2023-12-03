## 개념
#### REST API
- REST API는 REST 아키텍처 스타일을 따르는 API
#### REST: REpreseantational State Transfer
- 클라이언트와 서버 간 리소스를 주고받는 통신 체계에 대해 범용적인 스타일을 규정해둔 아키텍처
#### API: Application Programming Interface
- 소프트웨어 시스템과 통신하기 위해 따라야 하는 규칙을 정의한 인터페이스

### 장점
- 네트워크를 통해 자원을 다루는 표준화된 방법 제공 → 분산 환경에서의 통신과 자원 관리 용이함
- 클라이언트와 서버 간 유연하고 효율적 통신

## 특징

#### 1. 균일한 인터페이스 Uniform Interface
- 통일된 표준 형식으로 리소스 조작에 필요한 정보를 전송

#### 2. 무상태 Stateless
- 모든 요청은 이전의 요청과 독립적으로 수행되며, 따라서 작업을 위한 상태 정보를 따로 관리하지 않는다.

#### 3. 캐시 가능성 Cacheable
- 서버 응답 시간을 개선하기 위해 일부 응답을 저장하는 캐싱을 지원
- HTTP 프로토콜 표준에서는 Last-Modified 태그나 E-Tag를 이용하여 캐싱 구현 가능

#### 4. 계층화 시스템 Layered System
- 서버는 다중 계층으로 구성
- 보안, 로드 밸런싱, 암호화 계층을 추가하여 유연한 구조
- PROXY, GATEWAY 같은 네트워크 기반의 중간 매체를 사용 가능

#### 5. 자체 표현성 Self-descriptiveness
- REST API 그 자체로 의미를 지니고 있어 무엇을 표현하는지 쉽게 이해 가능

## 구성 요소

### 1. 자원 Resource
- URI은 리소스를 고유하게 나타내는 식별자 역할 (엔드포인트)
- 고유한 식별자를 통해 자원에 접근 가능

### 2. 행위 Verb
- 자원에 대해 수행해야 하는 작업 행위
- HTTP METHOD를 이용하여 정의: GET, POST, PUT, PATCH, DELETE ([HTTP Request Method](/Network/HTTP%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94..md#http-request-method))

### 3. 표현 Representations
- 형식이 지정된 리소스
- 대표적인 자원 형식: JSON, XML, HTML

## 설계 원칙

### 1. URI는 리소스 자체를 표현해야 한다.
- 리소스에는 동사로 행위에 대한 표현을 하지 않는다. 
- 명사를 사용하여 리소스 자체를 표현해야 한다.
#### Collection과 Document로 표현
- Collection: Document들의 집합
	- Collection은 복수를 사용
	- Document: 문서, 객체
- URI는 Collection 안의 특정 Document를 의미하도록 표현
- ex. URI 예시 `/players/13`
	- Collection: players
	- Document: 13

### 2. 자원에 대한 행위는 METHOD로 표현한다.
- 표현하고자 하는 행위에 적합한 METHOD를 사용 (리소스 조회, 생성, 수정 등)

### 3. 요청에 대한 응답 상태 코드를 명확하게 정의한다. ([HTTP 응답 상태 코드](/Network/HTTP%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94..md#http-response-status-code))

## 설계 참고 사항
- 슬래시 구분자(/)를 사용하여 계층 관계를 나타낸다.
  - 리소스 간 연관 관계는 `/리소스명/리소스ID/연관 관계에 있는 다른 리소스명`과 같이 표현
  - 일반적으로 소유 관계일 때 `/users/{user-id}/devices`와 같이 표현
- URI 마지막 문자로 슬래시(/)를 포함하지 않는다.
- 가독성을 위해 하이픈`-`을 이용할 수 있고, 언더바`_`는 URI에 사용하지 않는다.
- URI에 대문자를 사용하지 않고 소문자만 사용한다.
- 파일 확장자는 URI에 포함시키지 않고, Header의 Accept를 사용한다.

## 참고자료
- [Principled Design of the Modern Web Architecture, Roy Fieldiing, 2002](https://ics.uci.edu/~taylor/documents/2002-REST-TOIT.pdf)
- [AWS RESTful API란 무엇인가요?](https://aws.amazon.com/ko/what-is/restful-api/)
- [Microsoft RESTful 웹 API 디자인](https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/api-design?fbclid=IwAR3TZPok-d2vsIwMyguAGAzfJS8LK5qITS9a2PE5YeaJBtNsUCrtiFDfg74)
- [REST API 제대로 알고 사용하기](https://meetup.nhncloud.com/posts/92)

#### 관련 질문

- REST API가 뭔가요? (개념, 특징)
- REST API의 구성은 어떤 것이 있나요? (구성 요소)
  - HTTP 요청 METHOD에 대해 설명해주세요.
  - HTTP 응답 상태 코드에 대해 설명해주세요.
- REST API를 설계할 때 무엇이 중요할까요? (설계 원칙)