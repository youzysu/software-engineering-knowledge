#### 관련 질문

- REST API가 뭔가요? (개념, 특징)
- REST API의 구성은 어떤 것이 있나요? (구성 요소)
  - HTTP 요청 METHOD에 대해 설명해주세요.
  - HTTP 응답 상태 코드에 대해 설명해주세요.
- REST API를 설계할 때 무엇이 중요할까요? (설계 원칙)

## 개념

- [REST 정의] REST란 REpreseantational State Transfer를 의미하며, 웹 서비스를 위한 아키텍처 모델입니다.
- 웹 어플리케이션끼리 리소스를 주고받는 통신 체계에 대해 범용적인 스타일을 규정해둔 아키텍처로, 다양한 클라이언트와 서버 사이의 통신을 가능하게 합니다.
- [API 정의] API란 Application Programming Interface를 의미하며, 소프트웨어 시스템과 통신하기 위해 따라야 하는 규칙을 정의한 인터페이스입니다.
- [REST API 정의] REST API는 REST 아키텍처 스타일을 따르는 API를 의미합니다.
- 대부분의 일반적인 REST API는 HTTP 프로토콜을 기반으로 구현되며, 이를 통해 클라이언트와 서버 간의 통신을 처리합니다.
- [장점] REST API는 네트워크를 통해 자원을 다루는 표준화된 방법을 제공하여 분산 환경에서의 통신과 자원 관리를 용이하게 합니다.
- 이를 통해 클라이언트와 서버 간에 유연하고 효율적으로 통신할 수 있습니다.

### 특징

#### 1. 균일한 인터페이스 Uniform Interface

- 통일된 표준 형식으로 리소스 조작에 필요한 정보를 전송해야 합니다.

#### 2. 무상태 Stateless

- 모든 요청은 이전의 요청과 독립적으로 수행되며, 따라서 작업을 위한 상태 정보를 따로 관리하지 않습니다.

#### 3. 캐시 가능성 Cacheable

- 서버 응답 시간을 개선하기 위해 일부 응답을 저장하는 캐싱을 지원합니다.
- HTTP 프로토콜 표준에서는 Last-Modified 태그나 E-Tag를 이용하여 캐싱을 구현할 수 있습니다.

#### 4. 계층화 시스템 Layered System

- 서버는 다중 계층으로 구성될 수 있습니다. 보안, 로드 밸런싱, 암호화 계층을 추가하여 유연한 구조를 만들 수 있습니다.
- 또한 PROXY, GATEWAY 같은 네트워크 기반의 중간 매체를 사용할 수 있습니다.

#### 5. 자체 표현성 Self-descriptiveness

- REST API 그 자체로 의미를 지니고 있어 무엇을 표현하는지 쉽게 이해할 수 있습니다.

## 구성 요소

### 1. 자원 Resource

- 모든 자원은 고유한 식별자를 갖고, 이 식별자를 통해 자원에 접근할 수 있습니다.
- URI은 리소스를 고유하게 나타내는 식별자 역할을 하며, 이를 엔드포인트라고도 부릅니다.

### 2. 행위 Verb

- HTTP METHOD를 이용하여 자원에 대해 수행해야 하는 작업 행위를 정의합니다.
- 주요 HTTP METHOD로는 GET, POST, PUT, PATCH, DELETE가 있습니다. ([HTTP Request Method](/Network/HTTP%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94..md#http-request-method))

### 3. 표현 Representations

- 형식이 지정된 리소스를 의미하며, 클라이언트가 요청하는 자원의 형식은 다양할 수 있습니다.
- 대표적으로 JSON, XML, HTML이 있습니다.

## 설계 원칙

### 1. URI는 리소스 자체를 표현해야 합니다.

- 리소스는 Collection과 Document로 표현할 수 있습니다.
  - Collection은 Document들의 집합이고 Document는 문서, 객체로 볼 수 있습니다.
  - Collection은 복수를 사용하고, 해당 Collection 안의 특정 Document를 의미하도록 URI를 표현합니다.
  - 예를 들어, `/players/13`은 players라는 Collection과 13을 의미하는 Document로 URI를 표현한 것입니다.

### 2. 자원에 대한 행위는 METHOD로 표현합니다.

- 리소스에는 동사로 행위에 대한 표현을 하지 않고, 명사를 사용하여 리소스 자체를 표현해야 합니다.
- 리소스 조회, 생성, 수정 등 표현하고자 하는 행위에 적합한 METHOD를 사용해야 합니다.

### 3. REST API는 요청에 대한 응답 상태 코드도 명확하게 정의해야 합니다. ([HTTP 응답 상태 코드](/Network/HTTP%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94..md#http-response-status-code))

## 설계 참고 사항

- 슬래시 구분자(/)를 사용하여 계층 관계를 나타냅니다.
  - 리소스 간 연관 관계는 `/리소스명/리소스ID/연관 관계에 있는 다른 리소스명`과 같이 표현합니다.
  - 예를 들어, 일반적으로 소유 관계일 때 `/users/{user-id}/devices`와 같이 표현합니다.
- URI 마지막 문자로 슬래시(/)를 포함하지 않습니다.
- 가독성을 위해 하이픈`-`을 이용할 수 있고, 언더바`_`는 URI에 사용하지 않습니다.
- URI에 대문자를 사용하지 않고 소문자가 적합합니다.
- 파일 확장자는 URI에 포함시키지 않고, Header의 Accept를 사용합니다.

## 참고자료

- [Principled Design of the Modern Web Architecture, Roy Fieldiing, 2002](https://ics.uci.edu/~taylor/documents/2002-REST-TOIT.pdf)
- [AWS RESTful API란 무엇인가요?](https://aws.amazon.com/ko/what-is/restful-api/)
- [Microsoft RESTful 웹 API 디자인](https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/api-design?fbclid=IwAR3TZPok-d2vsIwMyguAGAzfJS8LK5qITS9a2PE5YeaJBtNsUCrtiFDfg74)
- [REST API 제대로 알고 사용하기](https://meetup.nhncloud.com/posts/92)
