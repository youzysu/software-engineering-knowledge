## 개념
- 리액트 렌더 트리 상 컴포넌트 아래 전체 트리에 일부 정보를 제공할 수 있는 방법
- props drilling으로 깊이 prop을 전달해야 하는 경우 번거로움을 해결

## 사용 방법
- `createContext`를 사용하여 context를 생성
- 생성한 context를 export하여 사용
- 자식 컴포넌트를 `<Context.Provider value={...}>`로 감싸서 전역 상태로 제공
- 해당 context를 사용하려는 하위 컴포넌트에서 `useContext` hook으로 해당 context의 value 사용 가능
- 자주 사용하는 context를 커스텀 훅으로 생성하여 사용

## 참고자료
- [Passing Data Deeply with Context](https://react.dev/learn/passing-data-deeply-with-context)