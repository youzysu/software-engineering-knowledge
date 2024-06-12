https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Virtual-DOM/

# 1. 브라우저 로딩 과정

## 1. HTML 파싱 -> DOM 트리 생성
- 브라우저는 주소창에 입력한 URL의 서버에 요청하여 받은 HTML 파일을 렌더링한다.
- html 코드는 byteStream(8bit의 데이터 형태)으로 전송된다.
- HTML Parsing: 브라우저는 전달받은 byteStream을 문자로 변환한다.
- 토큰화: 토큰과 비교하여 변환된 문자가 html 코드인지 확인하고 분석하는 과정 
- `<!DOCTYPE html>, <html>, <head>, <meta>`와 같은 태그와 lang="ko" 와 같은 속성 및 속성값으로 html 코드를 확인하고 분석한다.
- DOM 트리 생성: 토큰화 과정을 통해 노드를 생성한다.

## 2. CSS 파싱 -> CSSOM 트리 생성
- CSS Object Model
- HTML 파일과 마찬가지로 전달받은 데이터를 문자로 변환하여 토큰을 기반으로 분석하여 노드를 생성하여 CSSOM을 만든다.

## 3. 렌더 트리 생성
- DOM & CSSOM을 합쳐서 렌더링을 위한 트리를 생성한다.
- display: none 속성이 적용된 요소는 렌더 트리에 포함되지 않는다.

## 4. 레이아웃 Reflow
- DOM과 CSSOM을 바탕으로 요소의 위치, 크기를 계산한다.
- 요소의 크기나 좌표와 같은 정보를 담은 레이아웃 트리를 생성한다.
- 레이아웃 관련 속성: position, left, top, right, bottom, width, height, margin, padding, border, display, float, overflow, font-, text, ...

## 5. 페인트 Repaint
- 렌더 트리를 따라 페인트 기록을 생성한다.
- 모든 시각적인 요소를 그린다. 요소를 렌더링 하는 순서, 페이지를 구성하는 레이어 등
- 페인트 관련 속성: background, box-shadow, border-radius, color, outline, ...
- 레이아웃에 영향이 없다면 리플로우 없이 리페인트만 실행된다.

## 6. Composite
- 레이어를 스크린에 픽셀로 표현한다. 
- 레이어를 하나로 합성해서 페이지를 완성한다.
- transform, opacity 속성은 리플로우, 리페인트 단계를 거치지 않고 적용된다.
- GPU가 관여할 수 있는 속성
- DOM 트리를 변경하지 않도록 설계되어 있다.
- 렌더링 성능에 좋다.

## 4. VirtualDOM에 대한 고찰

- 바닐라 자바스크립트에서 RealDOM을 VirtualDOM처럼 사용해보기

- `중요한 점은 DOM을 메모리상에만 올려놓고 값을 변경하는 작업을 하는 것이다.`
  - 렌더링이 발생하지 않는다.
  - 메모리에 참조중인 값만 변경한다.
  - `render` : 인자로 받은 state를 기반으로 템플릿 리터럴로 HTML을 생성
  - `realNode` : 메모리 상에서 cloneNode 메서드로 새로운 DOM node를 생성
  - 이후에 실제 HTML에 이 realNode를 추가/교체할 때만 렌더링이 발생하게 된다.

## 바인딩

> MVVM의 핵심은 ViewModel, 더 정확히는 Binder에 있다. 그리고 Binding에는 두 가지 방식이 있다. (MVVM 시스템 만들기)

1. Template Scan : Vue나 Angular에서 사용한다.
2. Template과 State의 결합 : React에서 사용한다.
   - Data와 연결되어있는 view를 만들고, binder가 view를 꼭 껴안아 가지고 있는 방식이다.

## 렌더링 최적화

> 댓글: 불필요한 렌더링 방지를 위해 requestAnimationFrame 같은 API를 사용하여 한 프레임의 시간 동안 발생하는 변화를 모아서 처리해주기