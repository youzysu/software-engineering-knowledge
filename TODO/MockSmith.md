mock을 쉽게 만드는 것

mock data,
mock handler,

schema

BE DB schema 서버 스키마 (변경이 전파되도록)

DB Schema -> Response -> 

**contract schema 가 기준이 된다.**

자동 생성을 기본으로, 수동 수정 (override)

---

mockAPI 생성을 편하게

contract를 기반으로 handler를 만들도록
ex. createGetHandler(contract, mockData 기반 - devTools로 입력 받아서 override도 가능하도록)

devTools 에서 msw 네트워크 요청/응답을 가져올 수 있어야 함