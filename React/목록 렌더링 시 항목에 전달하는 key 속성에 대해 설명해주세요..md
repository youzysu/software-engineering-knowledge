### key가 필요한 이유
- 리액트에서 리스트를 렌더링할 때 형제 요소 사이에서 **특정 항목을 고유하게 식별**
- 고유한 key를 제공하면 해당 항목이 리스트 상에서 위치가 변경되더라도 리액트는 그 아이템을 식별하여 효과적으로 상태 추적 가능

### 역할
- 목록에 변경 사항이 생길 때 key 속성을 기반으로 해당 항목의 상태 변화를 추적하여 변경된 부분만 적절하게 UI를 업데이트

### 규칙
1. key는 같은 목록에 속하는 형제 요소 간에 고유해야 한다.
2. key는 변경되지 않아야 한다. 
	- 렌더링 시에 동적으로 key를 생성하면 매번 렌더링마다 key가 변경되기 때문에 모두 다 재렌더링된다.

### 관련 질문
#### 배열의 index를 key로 사용
- 리액트에서 key 속성을 명시적으로 전달하지 않는 경우 기본적으로 배열의 index를 key로 사용
- 리스트 항목의 순서나 중간에 새로운 항목 추가 등의 변경이 일어나지 않는 경우에는 배열의 index를 key로 사용 가능
- 만약 변경이 일어난다면 기존 항목의 배열 내의 위치index가 변경될 수 있기 때문에 리액트가 상태 추적을 할 수 없어 의도치 않은 버그가 발생 가능