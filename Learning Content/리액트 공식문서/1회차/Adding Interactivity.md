## #01

Conditional Rendering
- **Don’t put numbers on the left side of `&&`.**
> To test the condition, JavaScript converts the left side to a boolean automatically. However, if the left side is `0`, then the whole expression gets that value (`0`), and React will happily render `0` rather than nothing.
> For example, a common mistake is to write code like `messageCount && <p>New messages</p>`. It’s easy to assume that it renders nothing when `messageCount` is `0`, but it really renders the `0` itself!
> To fix it, make the left side a boolean: `messageCount > 0 && <p>New messages</p>`.

-  &&: 첫번째 falsy를 반환, 피연산자에 falsy가 없다면 마지막 값을 반환
- 따라서 0을 렌더링함

## #02 

Rendering Lists

```js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );
  return (
    <article>
      <h1>Scientists</h1>
      <ul>{listItems}</ul>
    </article>
  );
}
```

- 컴포넌트 배열 자체를 넘긴다? 자바스크립트 문법으로만 따지면 `{...listItems}` 이 더 맞지 않나?라는 생각이 들었는데, JSX가 컴포넌트 배열이면 알아서 바꾸나? -> YES
```gpt
JSX에서 중괄호(`{}`)는 JavaScript 식을 평가하고 해당 결과를 JSX 요소 안에 포함시키는 역할을 합니다. 따라서 `{...listItems}`를 사용하면 배열인 `listItems`를 객체 분해 연산자를 통해 JSX 요소 안에 넣으려고 시도하는 것입니다.

그러나 JSX에서는 배열을 직접 사용할 수 없으며, 배열을 JSX 요소로 변환하기 위해서는 배열의 각 요소를 JSX로 매핑해야 합니다. 따라서 `listItems` 배열을 `{listItems}` 형태로 JSX 요소 안에 넣어줘야 합니다.

리액트는 JSX에서 컴포넌트 배열을 인식하고 자동으로 렌더링합니다. JSX에서 중괄호 내부에 컴포넌트 배열을 넘겨주면, 리액트는 각 컴포넌트를 순차적으로 렌더링하고 최종 결과를 반환합니다.

따라서 `listItems` 배열을 JSX 요소로 사용하기 위해서는 `{listItems}` 형태로 배열을 JSX 안에 넣어주어야 리액트가 알맞게 렌더링할 수 있습니다. `{...listItems}`는 배열을 분해하는 문법이지만, JSX에서는 컴포넌트 배열을 직접 사용해야 합니다.
```

## #03

> Why and how to set `key` on each component in a collection so React can keep track of each of them even if their position or data changes. 
- virtualDOM를 비교할 때 컴포넌트를 식별하여 위치나 데이터를 변경해주기 위해서

## #04
> **By calling the component functions twice, Strict Mode helps find components that break these rules.**
- StrictMode가 왜 두번씩 실행하는지? Component가 순수함수가 아닌지 확인하기 위함
- don’t mutate variables outside of the function’s scope or objects that were created before the call

## #05

- 리액트 컴포넌트는 순수하다. 왜 리액트는 순수함수로 구현했을까?
- 여러번 실행해도 된다. 독립적으로 외부에 아무런 영향을 주지 않으므로
	- Your components could run in a different environment—for example, on the server! Since they return the same result for the same inputs, one component can serve many user requests.
- 외부로부터 의존하지 않기 때문에 값이 변하지 않은걸 보장할 수 있다.
	- You can improve performance by [skipping rendering](https://react.dev/reference/react/memo) components whose inputs have not changed. This is safe because pure functions always return the same results, so they are safe to cache.
- 중간에 멈추고 다시 실행해도 무방하다. 
	- If some data changes in the middle of rendering a deep component tree, React can restart rendering without wasting time to finish the outdated render. Purity makes it safe to stop calculating at any time.

## #06
 > **Hooks—functions starting with `use`—can only be called at the top level of your components or [your own Hooks.](https://react.dev/learn/reusing-logic-with-custom-hooks)** You can’t call Hooks inside conditions, loops, or other nested functions.

- 함수라고 생각하고 이해해보면, 최상단에서 한번만 호출되는게 아니라 여러번 호출되면? 렌더링 순서나 의존성 파악이 어려워, 상태 예측이 어렵다.

## #07 How does React know which state to return? 🔎
> Internally, React holds an array of state pairs for every component. It also maintains the current pair index, which is set to `0` before rendering. Each time you call `useState`, React gives you the next state pair and increments the index. You can read more about this mechanism in [React Hooks: Not Magic, Just Arrays.](https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e)

- 저번에 useState에서 덴이랑 제이든이 이야기나누던 부분이 이거다 싶었다. (근데 잘 이해가 안된다. 한번 정리했으면)

```js
let componentHooks = [];
let currentHookIndex = 0;

// How useState works inside React (simplified).
function useState(initialState) {
  let pair = componentHooks[currentHookIndex];
  if (pair) {
    // This is not the first render,
    // so the state pair already exists.
    // Return it and prepare for next Hook call.
    currentHookIndex++;
    return pair;
  }

  // This is the first time we're rendering,
  // so create a state pair and store it.
  pair = [initialState, setState];

  function setState(nextState) {
    // When the user requests a state change,
    // put the new value into the pair.
    pair[0] = nextState;
    updateDOM();
  }

  // Store the pair for future renders
  // and prepare for the next Hook call.
  componentHooks[currentHookIndex] = pair;
  currentHookIndex++;
  return pair;
}

```

- 배열에 pair [initialState, setState] 를 useState Hook이 호출된 순서대로 배열에 들어간다.
- currentHookIndex의 역할은?

## #08 Render, Commit

- **“Rendering” is React calling your components.** : 리액트가 함수 컴포넌트를 호출하는 것 (리액트 내부적으로 변경된 상태를 반영하는 것)
- commit은 실제 DOM에 반영하는 것

> State actually “lives” in React itself—as if on a shelf!—outside of your function. When React calls your component, it gives you a snapshot of the state for that particular render.

- 함수 컴포넌트를 호출할 때 useState 내부 로직으로 가겠지 = 초기 렌더링이 아니라면 (initialState value는 무관하게 상태를 업데이트하겠지)

```jsx
<>
  <h1>{number}</h1>
  <button onClick={() => {
	setNumber(number + 1); // 0 + 1
	setNumber(number + 1); // 0 + 1
	setNumber(number + 1); // 0 + 1
  }}>+3</button>
</>
```

- 같은 렌더링 내에서는 number가 동일 (변경된 상태는 다음 렌더링에 반영된다.)
> **React keeps the state values “fixed” within one render’s event handlers.** You don’t need to worry whether the state has changed while the code is running.

## #09 Batching

> **React waits until _all_ code in the event handlers has run before processing your state updates.**

> you can pass a _function_ that calculates **the next state based on the previous one** in the queue, like `setNumber(n => n + 1)`. 

- 이벤트 핸들러 내부의 모든 로직이 다 실행되고 난 후 UI를 업데이트 한다.
- 큐에서 순서대로 함수가 실행되고, 업데이트 대상 변수는 이전 상태값으로 고정되어 있다!

## #10 Immutable

- 값의 비교로 상태 변경을 감지하므로 Object의 경우 완전히 새로운 객체로 대체하여야 감지 가능
	- `!==` 참조값으로 비교

```ts
onPointerMove={e => {
	position.x = e.clientX;
	position.y = e.clientY;
}}

setPosition({
	x: e.clientX,
	y: e.clientY
});
```

