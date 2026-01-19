# 들어가며

리액트 프로젝트에서 저는 컴포넌트의 지역 상태가 필요할 때 컴포넌트 함수 안에서 `useState`를 사용합니다.
`useState`를 활용하여 정말 손쉽게 상태를 관리할 수 있죠.

> `초기 상태initialState`를 주면 내가 알아서 `상태state`를 저장해두고 너한테 줄게. 상태를 변경할 때는 `setState`를 사용해!
> 나는 너가 이렇게 상태를 변경하면 최신 상태로 그 컴포넌트를 다시 렌더링해. 그게 너가 원하는 거잖아?

근데 컴포넌트가 리렌더링될 때, 즉 해당 컴포넌트 함수가 다시 실행되었음에도 불구하고 이전 상태값을 도대체 어떻게 기억하고 있는걸까요?
저는 `useState`를 볼때마다 내부적으로 어떻게 구현이 되었는지 전혀 모르고 사용할 수 있을 만큼 추상화가 정말 잘되어있다는 생각이 들었어요.

공식문서 중 [How does React know which state to return?](https://react.dev/learn/state-a-components-memory#how-does-react-know-which-state-to-return)에서도 소개하고 있지만,
State를 **index**로 구분하여 관리하고 **클로저**로 이전 상태값을 기억해두고 사용합니다.

> Hooks rely on a stable call order on every render of the same component. Hooks will always be called in the same order.
> hooks는 동일한 컴포넌트의 모든 렌더링에서 항상 동일한 순서로 호출됩니다.

이러한 아이디어를 기반으로 useState hook을 직접 구현해보면서 동작 원리를 이해해보려고 합니다.

# 개념

본격적인 구현에 앞서, 구현하고자 하는 대상에 대해 정확히 정의하고 시작해보겠습니다.

## 상태State

먼저 `상태State`란 무엇일까요? 리액트에서는 상태를 <u>컴포넌트의 메모리</u>라고 정의하고 있습니다.
컴포넌트의 메모리에는 컴포넌트가 가져야 할 데이터가 저장되어 있죠. 쉽게 말해 상태는 <u>컴포넌트가 가지고 있는 데이터</u>라고 할 수 있어요.

## useState

`useState`는 이러한 `상태state`를 저장할 수 있는 공간을 만들어주는 함수입니다.

# hook 설계

### Parameters

1. `initialState`

- 초기 상태값으로 초기 렌더링 이후에는 무시됩니다.
- 실제 리액트에서는 초기값으로 함수를 전달하여 컴포넌트를 초기화할 때 호출하여 그 결과값을 초기 상태로 사용할 수 있는데요, 우선 이번 구현에서는 제외하고 이후에 추가해볼게요.

### Returns

`현재 상태state`와 `이를 업데이트하는 함수setState`, 두 개의 값을 가진 배열`[state, setState]`을 반환합니다.

1. `state` : **현재** 상태값

- useState를 호출한 순서를 기준으로 index를 부여하여 관리합니다.

2. `setState` : 상태를 다른 값으로 변경하고, 리렌더링을 trigger하는 함수

- 이 함수는 새로운 상태값을 인자로 받습니다.
- 실제 리액트에서는 리렌더링 큐에 상태 업데이트를 쌓아 모았다가 한번에 처리한다고 알고 있으나, 이번 구현에서는 바로 리렌더링을 실행하도록 할게요.
- 실제 리액트에서는 setState 인자로 함수를 전달하면 이전 상태값을 인자로 받고 이를 기반으로 새로운 상태값을 반환하는 함수를 사용할 수 있으나, 마찬가지로 우선 이번 구현에서는 함수를 인자로 받는 경우를 제외할게요.

# hook 구현

## 미리보기

- https://codesandbox.io/p/sandbox/serene-architecture-4kltwj

## Entry Point

```tsx
// main.tsx
import ReactDOM from 'react-dom/client';
import App from './App.tsx';
import './index.css';

const root = ReactDOM.createRoot(document.getElementById('root')!);
root.render(<App />);

export function reRender() {
  root.render(<App />);
}
```

- 상태 변경 시 App을 리렌더링 하기 위해 `main.tsx` 파일에 간단히 `reRender` 함수를 만들고 export 하여 사용했어요.

## useState 구현

```ts
import { reRender } from './main';

export const { useState } = (() => {
  let callIndex = 0;
  const stateStack: unknown[] = [];

  const useState = <T>(initialValue: T): [T, (newValue: T) => void] => {
    const state = stateStack[callIndex] || initialValue;
    const setState = (() => {
      const currentIndex = callIndex;

      return (newValue: T) => {
        stateStack[currentIndex] = newValue;
        callIndex = 0;
        reRender();
      };
    })();

    if (!stateStack[callIndex]) {
      stateStack[callIndex] = state;
    }
    callIndex++;

    return [state as T, setState];
  };

  return { useState };
})();
```

### callIndex

- `useState` 함수가 호출된 순서를 기억하기 위한 변수입니다.

### stateStack

- `useState` 함수가 호출된 순서대로 상태값을 저장하는 배열입니다.

### useState

#### 1. state

- `state`는 `stateStack`의 `callIndex`번째 값을 가져오거나, 없으면 `initialValue`를 사용합니다. (첫번째 호출 시에만 사용되겠죠?)

#### 2. setState

- `setState` 함수 호출 시 해당 index에 바로 접근할 수 있도록 `setState` 함수 상위 스코프의 `currentIndex` 변수에 현재 `callIndex`를 저장해둡니다.
- 반환한 `setState` 함수가 실행되면 `stateStack`의 `currentIndex`번째 값을 인자로 받은 `newValue`로 변경합니다.
- `callIndex`를 0으로 초기화한 후, `reRender` 함수를 호출하여 변경된 상태로 화면을 업데이트해줍니다.

#### 💡 여기서 잠깐! 왜 `callIndex`를 0으로 초기화했을까요?

- 리렌더링 할 때 `App` 컴포넌트 함수가 호출되고, 내부의 `useState` 함수도 순서대로 호출되기 때문에, `callIndex`를 0으로 초기화하지 않으면 이전 index를 그대로 사용하여 `callIndex`를 증가시킵니다.
- 예를 들어, `App` 컴포넌트 함수가 호출되고, 내부에서 `useState`를 2번 호출했다고 가정해봅시다.
- `App` 컴포넌트 함수를 실행하기 전에 `callIndex`를 0으로 초기화 해야 내부에서 `useState` 함수가 2번 호출되면서 `callIndex`는 1, 2로 증가합니다.
- 만약 `callIndex`를 0으로 초기화하지 않으면, `useState`가 다시 호출되었으니 `callIndex`가 4가 되어버렸겠죠?

#### useState 함수 마무리

- 이전 상태값이 없을 때 `stateStack`의 `callIndex`번째에 `state`값을 저장합니다.
- 다음 `useState` 호출 시에 다음 index를 사용하도록 `callIndex`를 1 증가시키고, `state`와 `setState`를 반환합니다.

# 클로저

위에서 `useState` hook을 구현한 내용을 보면, 호출될 때마다 증가하는 `callIndex` 값을 기억하고, 또 이를 기반으로 이전 상태값을 저장해두고 사용하는 것을 볼 수 있습니다.
이러한 동작은 자바스크립트의 '클로저'라는 특성을 기반으로 하는데요, 클로저에 대해 간단히 정리해볼게요.

## 클로저란?

- 클로저는 함수가 자신이 생성될 때의 스코프에서 알 수 있었던 변수를 기억하는 현상을 말합니다. 이후에 함수가 호출될 때 이 변수를 참조하여 사용할 수 있습니다.
- 어떤 함수에서 선언한 변수를 참조하는 내부 함수를 외부로 전달할 경우, 함수의 실행 컨텍스트가 종료된 이후에도 함수에서 선언한 변수가 사라지지 않는 현상을 말합니다.

1. `useState` 함수를 반환하고 나서도, `useState` 함수 외부 스코프에서 선언해둔 `callIndex`, `stateStack` 변수를 참조하여 사용하고 있습니다.
2. 마찬가지로 `setState`가 반환한 함수도 외부에서 선언해둔 `currentIndex` 변수를 참조하여 사용하고 있습니다.

# 내가 만든 hook 적용해보기

## `App` 컴포넌트

- 최상위 App 컴포넌트에는 `count`와 `product` 상태를 선언했어요.
- 상품 이름 상태를 변경하는 `onNameEdit` 함수와, 상품 가격 상태를 변경하는 `onChangePrice` 함수를 선언하고, 이를 하위 컴포넌트에 전달합니다.

## `ProductPriceButtons` 컴포넌트

- `ProductPriceButtons` 컴포넌트에는 `increment`와 `decrement` 상태를 선언했어요.
- Props로 전달받은 `onChangePrice`로 상태값을 변경하는 버튼을 만들고, `PriceAmountEditor` 컴포넌트를 렌더링합니다.

## `PriceAmountEditor` 컴포넌트

- `PriceAmountEditor` 컴포넌트에는 `amount` 상태를 선언했어요.
- Props로 전달받은 `initialAmount`를 기준으로 증가/감소를 구분하여 버튼을 렌더링하고, `onChangeAmount`로 상태값을 변경합니다.

## `ProductNameEditor` 컴포넌트

- `ProductNameEditor` 컴포넌트에는 `input` 상태를 선언했어요.
- Props로 전달받은 `onNameEdit`로 상태값을 변경합니다.

## 결과

![image](https://github.com/youzysu/software-engineering-knowledge/assets/useState.gif)

- 지금은 상태가 변경되면 APP 컴포넌트 전체를 리렌더링하도록 구현되었기 때문에, 모든 컴포넌트가 리렌더링되는 것을 볼 수 있어요.
- 하지만 실제 리액트는 상태가 변경된 컴포넌트만 리렌더링 하도록 최적화되어있어요. (이와 관련해서는 가능하다면 나중에 더 다뤄보겠습니다 😉)

# 마치며

`useState` 덕분에 하나의 컴포넌트에서 독립적으로 여러 개의 고유한 `state`를 만들어서 관리할 수 있게 되었고,
클래스 컴포넌트의 복잡한 문법 없이 함수 컴포넌트로 간단하게 상태를 관리할 수 있게 되었어요.
그리고 바로 이 점이 리액트가 이렇게 인기를 얻게될 수 있었던, 상태 관리의 큰 변화였다고 들었던 기억이 납니다.

자바스크립트의 클로저라는 특성을 잘 활용한 아이디어로 탄생한 useState hook!
저도 현재의 관행에 머무르지 않고, 더 편리하고 간결하게 문제를 해결할 수 없을지 고민하고 실현하는 힘을 길러야겠어요!

잘못된 내용이나 보완할 점이 있다면 언제든지 댓글로 남겨주세요! 감사합니다 😊

## 참고자료

- [React 공식문서 Reference - useState](https://react.dev/reference/react/useState)
- [기술서적 스터디에서 Den이 공유해준 코드](https://github.com/sjuhan123/myLibraries)
