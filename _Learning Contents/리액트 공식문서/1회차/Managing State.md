# Reacting to Input with State

## Describing the UI for each visual state
- a **declarative** way to manipulate the UI : describe the different states that your component can be in, and switch between them in response to the user input.
- NOT imperative : NOT manipulating UI directly
- “state machine” 유한 상태 머신(Finite State Mode, FSM) 이론을 기반으로 한다.

## Steps
1. **Identify** your component’s different visual states
	- visualize all the different “states” of the UI the user might see
1. **Determine** what triggers those state changes: Human inputs? Computer inputs?
2. **Represent** the state in memory using `useState`
3. **Remove** any non-essential state variables
4. **Connect** the event handlers to set the state

- 꼭 필요한 상태만, 기능에 필요한 최소한의 상태
```ts
// BEFORE
const [isEmpty, setIsEmpty] = useState(true);
const [isTyping, setIsTyping] = useState(false);
const [isSubmitting, setIsSubmitting] = useState(false);
const [isSuccess, setIsSuccess] = useState(false);
const [isError, setIsError] = useState(false);

// AFTER
const [answer, setAnswer] = useState('');
const [error, setError] = useState(null);
const [status, setStatus] = useState('typing'); // 'typing', 'submitting', or 'success'
```

> a non-null `error` doesn’t make sense when `status` is `'success'`

> To model the state more precisely, you can extract it into a reducer. Reducers let you unify multiple state variables into a single object and consolidate all the related logic!

- 위의 예시에서 error, status state도 관련이 있는 것처럼, 관련된 상태에 대한 로직을 통합적으로 관리해야할 때는 reducer로 단일 object로 통합하여 관리

# Choosing the State Structure

## Principles for structuring state 

1. **Group related state.** 
2. **Avoid contradictions in state.** 
3. **Avoid redundant state.**
   - If you can calculate some information from the component’s props or its existing state variables during rendering, you **should not** put that information into that component’s state.
4. **Avoid duplication in state.** < 마침 뉴스스탠드 리스트 보기 데이터로 고민하던 부분인데 조언을 구해보자
5. **Avoid deeply nested state.**

# Sharing State Between Components

- 하위 컴포넌트들끼리 상태를 공유해야 하는 경우 -> Controlled components (driven by props)
- Uncontrolled Component (driven by state)

1. **Remove** state from the child components.
2. **Pass** hardcoded data from the common parent.
3. **Add** state to the common parent and pass it down together with the event handlers.

# Preserving and resetting state

- when to preserve state and when to reset it between re-renders.
- React preserves the parts of the tree that “match up” with the previously rendered component tree.
- 다시 말하면 렌더링이 변경되어야 하면 리액트가 변화를 감지할 상태가 있어야 한다는 의미한다.

> When you give a component state, you might think the state “lives” inside the component. But the state is actually held inside React. React associates each piece of state it’s holding with the correct component by where that component sits in the UI tree.

> React will keep the state around for as long as you render the same component at the same position

1. Same component at the same position preserves state 
1. Different components at the same position reset state
	> As a rule of thumb, **if you want to preserve the state between re-renders, the structure of your tree needs to “match up”** from one render to another. 
	> If the structure is different, the state gets destroyed because React destroys state when it removes a component from the tree.
	
### Resetting State
1. Render components in different positions

```jsx
// BEFORE
<div>
  {isPlayerA ? (
	<Counter person="Taylor" />
  ) : (
	<Counter person="Sarah" />
  )}
  <button onClick={() => {
	setIsPlayerA(!isPlayerA);
  }}>
	Next player!
  </button>
</div>

// 1. Render components in different positions
<div>
  {isPlayerA &&
	<Counter person="Taylor" />
  }
  {!isPlayerA &&
	<Counter person="Sarah" />
  }
  <button onClick={() => {
	setIsPlayerA(!isPlayerA);
  }}>
	Next player!
  </button>
</div>

// 2. Resetting state with a key
<div>
  {isPlayerA ? (
	<Counter key="Taylor" person="Taylor" />
  ) : (
	<Counter key="Sarah" person="Sarah" />
  )}
  <button onClick={() => {
	setIsPlayerA(!isPlayerA);
  }}>
	Next player!
  </button>
</div>
```

```jsx
{isPlayerA && <Counter person="Taylor" />}
{!isPlayerA && <Counter person="Sarah" />}
```

- 위의 둘을 각각의 위치로 인식
![[Pasted image 20230611090934.png]]

```jsx
// BEFORE
<Chat contact={to} />

// AFTER
<Chat key={to.email} contact={to} />
```

### Preserving state for removed components

1. Render all chats instead of just the current one, but hide all the others with CSS
2. Lift the state up and hold the pending message for each recipient in the parent component. 
3. Use a different source (ex. LocalStorage)

> No matter which strategy you pick, a chat _with Alice_ is conceptually distinct from a chat _with Bob_, so it makes sense to give a `key` to the `<Chat>` tree based on the current recipient.

- 내용은 맞는데 수신자 설정만 다르게 하는 경우 -> 그럼 그냥 그대로

> Remember that keys are not globally unique. They only specify the position _within the parent_.
- 같은 계층(동일한 부모 컴포넌트를 가진 자식 컴포넌트들 간)에서는 key가 고유해야 함.

# Extracting State Logic into a Reducer
- 해결하려는 문제: 액션에 따른 복잡한 상태 변경을 컴포넌트 로직에서 분리하여 역할을 구분하고자 한다.
- Reducer: A single function which consolidate all the state update logic outside component.
- specify “what the user just did” by dispatching “actions” from your event handlers. 
- You could even use the reduce() method with an initialState and an array of actions to calculate the final state by passing your reducer function to it: 
	- 테스트코드 작성할 때 이렇게 하면 좋을듯! 원하는 액션들이 처리된 후의 최종 결과 테스트

```js
let initialState = [];
let actions = [
  {type: 'added', id: 1, text: 'Visit Kafka Museum'},
  {type: 'added', id: 2, text: 'Watch a puppet show'},
  {type: 'deleted', id: 1},
  {type: 'added', id: 3, text: 'Lennon Wall pic'},
];

let finalState = actions.reduce(tasksReducer, initialState);
```

## Comparing useState and useReducer 
- **Reducers must be pure.**
- **Each action describes a single user interaction, even if that leads to multiple changes in the data.**

- 언제 Reducer? 하나의 액션으로 여러 상태가 변경되어야 할 때, 상태 간의 관계가 복잡할 때

# Passing Data Deeply with Context

- 해결하려는 문제: 하위 컴포넌트들한테 props 전달하기 어렵거나 귀찮을 때 
	1. 중간에 여러 컴포넌트를 거쳐서 props가 전달되는 경우(props drilling)
	2. 여러 컴포넌트가 동일한 data를 사용하는 경우 (props 전달 많이 필요)
- 해결 방법: 자식이 부모로부터 일방적으로 받는게 아니라 부모조상한테 데이터를 요청하게 하기
- `createContext` 사용할 Context를 만든다.  
- `useContext` 만들어둔 context를 import하여 이 값을 useContext의 인자로 넣어 사용
- different React contexts don’t override each other.

```jsx
import { LevelContext } from './LevelContext.js';

export default function Section({ level, children }) {
  return (
    <section className="section">
      <LevelContext.Provider value={level}>
        {children}
      </LevelContext.Provider>
    </section>
  );
}
```

```tsx
import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

export default function Section({ children }) {
  const level = useContext(LevelContext);
  return (
    <section className="section">
      <LevelContext.Provider value={level + 1}>
        {children}
      </LevelContext.Provider>
    </section>
  );
}
```
- 컴포넌트는 그 위에 있는 UI 트리에서 가장 가까운 nearest `<LevelContext.Provider>`의 값을 사용

- 일단 props로 전달하는게 베스트
- Passing props: it makes it very clear which components use which data!
	- 위의 문제를 최대한 해결해보기: props 전달을 줄일 수 있다면 줄이기 (레이어) `<Layout><Posts posts={posts} /></Layout>`
- Context를 너무 남발하면 컴포넌트가 어떤 데이터를 사용하는지 파악하기 어려워짐

## Use cases for context

- 현재 계정(로그인 정보): 로그인 정보처럼 전역 상태로 둬야 하는 것
- 트리의 다른 부분에 있는 멀리 떨어진 컴포넌트에서 일부 정보가 필요한 경우
  
> 또한 일부 앱에서는 여러 계정을 동시에 조작할 수 있습니다(예: 다른 사용자로 댓글을 남기는 경우). 이러한 경우 UI의 일부를 다른 현재 계정 값으로 중첩된 provider로 감싸는 것이 편리할 수 있습니다.
- 이게 무슨 상황이지?
- 네이버처럼 계정 여러개로 번갈아가면서 로그인 정보를 변경할 때?

# 👍 Scaling Up with Reducer and Context

- 왜 reducer와 context를 함께 상태 관리에 적용해야 할까?
- 최상위 컴포넌트의 상태를 변경시키는 이벤트 핸들러를 사용하려면.. props를 계속 전달해줘야
- 와 reducer랑 context랑 합치면 Magic이네... 원하는 컴포넌트에서부터만 전역을 만들어서 쓸 수 있게 만드는 느낌?
- reducer, context 관련 로직을 하나의 모듈로 만들어서 커스텀 훅 만들기

```js
export function useTasks() {
  return useContext(TasksContext);
}

export function useTasksDispatch() {
  return useContext(TasksDispatchContext);
}
```
