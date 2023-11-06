#### 관련 질문
- REST API가 뭔가요?
- REST API의 구성은 어떤 것이 있나요?
- REST API를 설계하는데 중요한 것이 있을까요?

## 개념
- [REST 정의] REST란 REpreseantational State Transfer를 의미하며, 웹 서비스를 위한 아키텍처 모델입니다.
- 웹 어플리케이션끼리 리소스를 주고받는 통신 체계에 대해 범용적인 스타일을 규정해둔 아키텍처로, 다양한 클라이언트와 서버 사이의 통신을 가능하게 합니다.
- [API 정의] API란 Application Programming Interface를 의미하며, 소프트웨어 시스템과 통신하기 위해 따라야 하는 규칙을 정의한 인터페이스입니다.
- [REST API 정의] REST API는 REST 아키텍처 스타일을 따르는 API를 의미합니다. 
- 대부분의 일반적인 REST API는 HTTP 프로토콜을 기반으로 구현되며, 이를 통해 클라이언트와 서버 간의 통신을 처리합니다.
- [장점] REST API는 네트워크를 통해 자원을 다루는 표준화된 방법을 제공하여 분산 환경에서의 통신과 자원 관리를 용이하게 합니다.
- 이를 통해 클라이언트와 서버 간에 유연하고 효율적으로 통신할 수 있습니다.

## 구성 요소

### 1. 자원 Resource
- 모든 자원은 고유한 식별자를 갖고, 이 식별자를 통해 자원에 접근할 수 있습니다.
- URI은 리소스를 고유하게 나타내는 식별자 역할을 합니다.

### 2. 행위 Verb
- HTTP METHOD를 이용하여 자원에 대한 행위를 정의합니다.
- 주요 HTTP METHOD로는 GET, POST, PUT, PATCH, DELETE가 있습니다.

### 3. 표현 Representations
- 클라이언트가 요청하는 자원의 표현은 다양할 수 있습니다.
- 자원의 상태를 전달하기 위한 대표적인 형태로는 JSON, XML, HTML이 있습니다.

## 특징

### 1. 균일한 인터페이스 Uniform Interface
- 
### 2. 무상태 Stateless

### 3. 캐시 가능성 Cacheable

### 4. 자체 표현성 Self-Descriptiveness

### 5. 클라이언트-서버 구조 Client-Server Architecture

### 6. 계층화 시스템 Layered System

## 설계 원칙

1. URI는 리소스를 표현해야 한다.
2. METHOD를 통해 자원에 대한 행위를 표현한다.

## 참고자료
- [AWS RESTful API란 무엇인가요?](https://aws.amazon.com/ko/what-is/restful-api/)
- [Microsoft RESTful 웹 API 디자인](https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/api-design?fbclid=IwAR3TZPok-d2vsIwMyguAGAzfJS8LK5qITS9a2PE5YeaJBtNsUCrtiFDfg74)
- [# REST API 제대로 알고 사용하기](https://meetup.nhncloud.com/posts/92)