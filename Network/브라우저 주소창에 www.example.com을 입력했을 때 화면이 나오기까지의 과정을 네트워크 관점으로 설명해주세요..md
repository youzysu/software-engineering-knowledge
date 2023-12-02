1. 브라우저 로컬 캐시 확인
	- 로컬 캐시로 저장된 기존 리소스 존재 여부 확인 [(웹 브라우저 캐시 전략)]()
2. 입력한 URL의 IP 주소 탐색
	1. DNS 레코드의 캐시 확인
	2. ISP의 DNS 서버를 통한 서버 IP 주소 조회
3. 클라이언트-서버 TCP 연결 [(3-way handshake)]()
4. 클라이언트에서 서버로 HTTP Request 메시지 전송
5. 서버에서 요청 처리 후 HTTP Response 메시지 전송
6. 브라우저는 응답 받은 HTML을 Critical Rendering Path를 거쳐 화면 출력 [(브라우저 렌더링 과정)](/Web/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%20%EB%A0%8C%EB%8D%94%EB%A7%81%20%EA%B3%BC%EC%A0%95%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94..md)
	- 서버는 루트(/) 요청에 대해 서버의 루트 폴더에 존재하는 `index.html`을 응답
7. 클라이언트-서버 TCP 연결 종료 [(4-way handshake)]()
	- HTTP/1.1은 기본적으로 keep-alive 속성으로 TCP 연결 유지
	- 헤더 설정 및 타임아웃 등의 상황에 따라 이후에 TCP 연결 종료

### DNS 레코드의 캐시 확인
1. 브라우저 캐시 확인
	- 브라우저에서 이전에 방문한 웹사이트에 대해 일정 기간 동안 유지하는 DNS 레코드 저장소에서 탐색
2. OS 캐시 확인
	- OS에 system call을 통해 OS에서 관리하는 DNS 캐시 테이블에서 탐색 (ex. ipconfig/displaydns)
3. 라우터 캐시 확인
	- 라우터와 통신하여 라우터의 DNS 레코드 캐시에서 탐색
4. ISP 캐시 확인
	- ISP의 DNS 레코드 캐시에서 탐색 (→ 캐시 결과가 없으면 ISP의 DNS 서버에서 DNS 쿼리를 통해 해당 도메인을 호스팅하는 서버의 IP 주소 조회)

### ISP의 DNS 서버를 통한 서버 IP 주소 조회
#### DNS Recursion Process
- ISP의 DNS 서버에서 DNS 쿼리를 통해 해당 도메인을 호스팅하는 서버의 IP 주소 조회
- DNS 서버가 클라이언트의 DNS 쿼리에 대한 응답을 찾을 때, 필요한 경우 계속해서 다른 DNS 서버에 쿼리를 보내는 과정
- 해당 도메인의 호스팅 서버 IP 주소를 찾을 때까지 계속해서 하위 DNS 서버에게 쿼리 요청

#### 단계
1. 루트 DNS 서버 쿼리
	- 최상위 도메인에 대한 정보 요청
	- 최상위 도메인(TLD, Top Level Domain) 서버의 IP 주소 응답
2. Top Level Domain(TLD) DNS 서버 쿼리
	- Domain Name에 대한 정보 요청
	- 해당 도메인의 네임서버 정보 응답
3. Domain Name Server 쿼리: 
	- 도메인 이름에 대한 정보 요청
	- 최종적으로 도메인에 대한 IP 주소 응답
4. 클라이언트에게 응답: 
	- DNS 서버는 최종적으로 얻은 IP 주소를 클라이언트에게 반환
	- 캐시에 저장하여 동일한 쿼리가 들어왔을 때 위의 앞선 Recursion 프로세스 없이 바로 반환

## 관련 개념

### DNS
- Domain Name System
- 계층형, 분산형 데이터베이스 시스템
- DNS 서버 간 계층적인 구조
- 호스트의 도메인 이름을 호스트의 IP 주소로 변환하여 라우팅 정보 제공

### [TLD](https://developer.mozilla.org/ko/docs/Glossary/TLD)