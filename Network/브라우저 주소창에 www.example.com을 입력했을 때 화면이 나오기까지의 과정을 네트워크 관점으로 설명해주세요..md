1. 유저가 브라우저에 URL을 입력하면 **해당 도메인에 대한 캐시 여부를 확인**합니다.
	- [파일 캐시] 해당 URL에 대해 **캐시된 파일**이 있는지 확인하고, 캐시된 파일이 있으면 네트워크 요청을 보내지 않고 해당 파일로 로드합니다.
	- [DNS 캐시] 캐시된 파일이 없으면 DNS 캐시를 확인하여 해당 도메인에 대해 캐시된 IP 주소가 있는지 확인하고, DNS 서버 요청을 보내지 않고 서버 IP 주소로 요청을 보냅니다.
2. 브라우저는 DNS를 통해 해당 도메인의 서버IP 주소를 조회합니다.
3. 서버 IP 주소로 HTTP Request 메시지를 전송합니다.
	- HTTP Request 메시지를 생성합니다.
	- HTTP Request Header를 추가하여 TCP/IP Packet을 생성합니다.
	- 해당 Packet은 전기 신호로 변환되어 랜선을 통해 목적지인 서버 IP에 도달합니다.
4. 서버에서 전달 받은 요청을 처리하고 HTTP Response 메시지를 전송합니다.
	- 서버에 도착한 Packet은 메시지로 복원되어 서버에서 요청을 처리합니다.
	- 요청에 대한 HTTP Response 메시지를 생성합니다.
	- 마찬가지로 HTTP Response Header를 추가하고 TCP/IP Packet을 생성하여 클라이언트 IP로 전송합니다.
5. 브라우저는 서버가 응답한 HTML(`index.html`) 파일을 기반으로 Critical Rendering Path를 거쳐 화면을 출력합니다. [(브라우저 렌더링 과정)](/Web/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%20%EB%A0%8C%EB%8D%94%EB%A7%81%20%EA%B3%BC%EC%A0%95%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94..md)
	- 서버는 루트(/) 요청에 대해 서버의 루트 폴더에 존재하는 `index.html`을 클라이언트로 응답합니다.

#### 연관 개념
- [TCP]()
- URI