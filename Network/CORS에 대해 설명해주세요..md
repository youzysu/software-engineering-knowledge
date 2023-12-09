- 교차 출처 리소스 공유 Cross Origin Resource Sharing
- 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저를  설정하는 체제

## 출처 Origin
- 동일한 출처: URL의 Protocol(Scheme), Host, Port가 모두 동일한 경우

## 접근 권한
- 동일 출처 정책 Same Origin Policy, SOP
- 원칙적으로 동일한 출처를 가진 곳으로부터 로드된 리소스에만 접근 가능

## CORS
- 브라우저는 보안 상 동일한 출처의 리소스에만 접근하는 걸 원칙으로 한다. (SOP)
- 여러 서비스들 간에 데이터를 주고 받는 과정은 필수적
- 보안을 위해 특정 사이트에 대한 접근 허용 필요 
	- Access-Control-Allow-Origin 설정
- 합의된 출처들 간의 접근을 허용하여 리소스를 공유할 수 있도록 하는 방법 (CORS)