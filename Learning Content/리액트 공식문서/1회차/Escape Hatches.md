## Referencing values with Refs
- 해결하려는 문제: 컴포넌트가 변화된 data를 기억은 해야 하는데, setState처럼 렌더링을 유발하지는 않도록
- ref는 current라는 프로퍼티를 가지는 일반적인 Object이다.
- a regular state variable without a setter: UI에 필요하지 않은 변수를 저장하기 위함

### When to use refs
> If your component needs to store some value, but it doesn’t impact the rendering logic, choose refs.

- Storing timeout IDs
- Storing and manipulating [DOM elements](https://developer.mozilla.org/docs/Web/API/Element)
- Storing other objects that aren’t necessary to calculate the JSX.

## Manipulating the DOM with Refs
> `<input ref={inputRef}>`. 
- This tells React to **put this `<input>`’s DOM node into `inputRef.current`.**
- 언제 필요할까? For calling browser APIs that React does not expose.

### [ref callback](https://react-ko.dev/reference/react-dom/components/common#ref-callback)

> Unless you pass the same function reference for the `ref` callback on every render, the callback will get temporarily detached and re-attached during every re-render of the component.

- ? 무슨 소리지?

### useImperativeHandle
- 해결하려는 문제: DOM Element를 노출하되, 원하는 동작에만 제한하도록 하기 위해서 (아래에선 focus만!)

```jsx
// 1번
import { forwardRef, useRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}

// 2번
import {
  forwardRef, 
  useRef, 
  useImperativeHandle
} from 'react';

const MyInput = forwardRef((props, ref) => {
  const realInputRef = useRef(null);
  useImperativeHandle(ref, () => ({
    // Only expose focus and nothing else
    focus() {
      realInputRef.current.focus();
    },
  }));
  return <input {...props} ref={realInputRef} />;
});

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}

```

> React sets `ref.current` during the commit.
- ref.current는 실제 DOM에 반영할 때 설정된다.

### Flushing state updates synchronously with flushSync
- DOM에 commit 시점과 ref.current를 참조하는 시점을 동기화시키기 위해서 (DOM에 반영된 후에, DOM API를 사용하고 싶을 때)
```jsx
flushSync(() => {
  setTodos([ ...todos, newTodo]);
});
listRef.current.lastChild.scrollIntoView();
```


# useEffect

- Effect는 commit 이후에(DOM에 모두 반영이 되고 난 후에) 실행된다.

## Synchronizing with Effects

- React에서 [렌더링은 JSX의 순수한 계산이어야](https://react-ko.dev/learn/keeping-components-pure) 하며 DOM 수정과 같은 사이드 이펙트를 포함해서는 안됩니다.
- React는 지정한 _모든_ 의존성의 값이 이전 렌더링 때와 정확히 동일한 경우에만 Effect의 재실행을 건너뜁니다.
	- = 하나라도 변하면 Effect가 다시 실행된다.
- 부모 컴포넌트가 항상 동일한 props를 전달하는지 알 수 없기 때문에 부모로부터 전달받은 경우에는 의존성 배열에 이를 추가해야 한다!
```
React Hook useEffect has a missing dependency: 'handleRemoveAllOrders'. Either include it or remove the dependency array. If 'handleRemoveAllOrders' changes too often, find the parent component that defines it and wrap that definition in useCallback.eslint[react-hooks/exhaustive-deps](https://github.com/facebook/react/issues/14920)
```

- Deep dive What are good alternatives to data fetching in Effects?
	- useEffect에서 data fetching 작업을 직접 하면 뭔 문제가 있어서 react-query나 프레임워크에서 제공하는 API를 쓰기를 추천한다.
	- 어떤 문제인지는 아직 경험이 없다만 1. 서버 사이드 렌더링에서 실행 불가능() -> 프레임워크가 해줌 2. 데이터 캐시 안해줌 -> 리액트 쿼리는 캐시해줌
- **Putting it all together** 뭐라는거

## You Might Not Need an Effect

- **Use Effects only for code that should run _because_ the component was displayed to the user.**
- 어떤 상태가 변경될 때 다른 상태를 변경시키고 싶어! -> Effect 사용하지 말고 걍 렌더링 로직에서 처리해
	- 왜? Effect는 commit으로 dom에 반영다되고나서 반영된다. 그냥 렌더링될때 상태 동기화 할 수 있다면 렌더링 로직에서 
## Lifecycle of Reactive Effects
