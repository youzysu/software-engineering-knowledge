- URL (Uniform Resource Locator): 인터넷의 리소스를 가리키는 표준이름, 브라우저가 정보를 찾는데 필요한 **리소스의 위치**
	- 스킴: 리소스에 어떻게 접근하는지 (어떤 프로토콜로)
	- 서버의 위치: 리소스가 어디에 호스팅 되어 있는지
	- 리소스의 경로: 서버에 존재하는 로컬 리소스들 중에서 요청받은 리소스가 무엇인지
- 리소스를 일관된 방식으로 지칭 가능
- 구성 `<스킴>://<사용자 이름>:<비밀번호>@<호스트>:<포트>/<경로>;<파라미터>?<쿼리>#<프래그먼트>`
	1. 스킴: 어떤 프로토콜을 사용하여 서버에 접근하여 리소스를 가져올 수 있는지
	2. 호스트: 리소스를 호스팅하는 서버의 호스트명이나 IP 주소
	3. 포트: 리소스를 호스팅하는 서버가 열어놓은 포트번호
	4. 경로: 서버 내 리소스가 어디에 있는지
	5. 쿼리: 애플리케이션에 파라미터를 전달하는데 사용
	6. 프래그먼트: 리소스의 조각이나 일부분을 가리키는 이름