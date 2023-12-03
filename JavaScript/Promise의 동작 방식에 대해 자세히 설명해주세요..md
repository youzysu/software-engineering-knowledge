## 정의
- 비동기 작업의 성공 또는 실패와 작업의 결과값을 나타내는 객체

## 사용 방법
### 실행함수 executor
- Promise 객체를 생성할 때 실행함수executor를 전달
	- Promise가 만들어질 때 자동으로 실행
#### `resolve`와 `reject`
- 비동기 작업이 완료되면 결과에 따라 둘 중 하나가 실행
- `resolve`: 비동기 작업이 성공적으로 처리된 경우 결과값 `value`을 인자로 받는 콜백함수
- `reject`: 비동기 작업이 실패한 경우 에러 객체 `error`를 인자로 받는 콜백함수

## 내부 프로퍼티
| | state | result |
| :-: | :-: | :-: |
|의미 | 비동기 작업의 처리 상태 | 비동기 작업의 처리 결과 |
|초기값 | 대기 pending | undefined |
| 비동기 작업 성공 <br /> resolve(value) 호출 | 이행 fulfilled | value | 
|비동기 작업 실패 <br /> reject(error) 호출 | 거부 rejected | error |

## 내부 메서드
- 비동기 작업에 대한 후속 처리
### 1. then
- `인자 1` : promise가 resolve되었을 때 value를 받아 실행되는 콜백함수
- `인자 2` : promise가 reject되었을 때 error를 받아 실행되는 콜백함수
### 2. catch
- `인자` :  promise가 reject되었을 때 error를 받아 실행되는 콜백함수
### 3. finally
- `인자` : promise의 결과와 상관없이 무조건 실행되는 콜백함수
	- 작업을 마무리하는 보편적인 동작 수행