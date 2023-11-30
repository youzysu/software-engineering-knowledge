# 1. Your First Component

```
React components 
- reusable UI elements
- regular JavaScript functions
- always begin with a capital letter.
- return JSX markup.
```

## Components: UI building blocks

> React lets you **combine** your markup, CSS, and JavaScript into custom “components”, **reusable UI elements for your app.**

- Component는 UI를 만드는 블록이다.
- markup, CSS, JS 로직을 합쳐서 작성한다.
- JavaScript function이다.

> Just like with HTML tags, you can compose, order and nest components to design whole pages.

- It's similar structure to HTML document.

## Using a component

- React Component는 대문자로, HTML tag는 소문자로
- 리액트 컴포넌트도 결국 HTML tag로 구성되어 브라우저에 표시된다.

### Components all the way down

> Using an empty HTML file and letting React “take over” managing the page with JavaScript, they _also_ generate the HTML automatically from your React components. This allows your app to show some content before the JavaScript code loads.

```
리액트의 한계, 프레임워크 사용하는 이유
- React: using an empty HTML file and taking over managing the page with JavaScript
	- 리액트로 작성한 자바스크립트 코드는 비어있는 HTML file에 root component를 만들고, 작성된 코드를 기반으로 전체 UI를 동적으로 만든다.
- Framework generate the HTML automatically from your React components. This allows your app to show some content before the JavaScript code loads.
	- 프레임워크는 리액트로 작성된 자바스크립트 코드를 기반으로  HTML 을 만들어둔다.
```

## Challenges

- JSX 반환 시 `()` 으로 묶어서 하나의 값처럼 반환해야 한다. -> 반환된 값을 React.createElement의 인자로 넘겨서 객체 형태로 변환된다. (Virtual DOM으로 구성된다.)

# 2. Importing and exporting components

- root component (entry point)

### Default vs named exports 

> Default exports(`export default`): the file exports **only one component**
> Named exports(`export`): the file exports **multiple** components and values.

- MixedComponents
	- 하나의 파일에서 두 방식을 동시에 사용할 수도 있다.

# 3. Writing Markup with JSX

- JSX: Syntax extension for **JavaScript** to write HTML-like markup

### JSX: Putting markup into JavaScript 

> But as the Web became more interactive, logic increasingly determined content. JavaScript was in charge of the HTML! This is why **in React, rendering logic and markup live together in the same place—components.**

> React components group **rendering logic together with markup** because they are **related**. 

> keeping rendering logic and content in the same place.

 - JavaScript logic determined content? Rendering logic과 markup을 함께!

### Why do multiple JSX tags need to be wrapped?

> JSX is transformed into **plain JavaScript objects**.

- 함수는 하나만(원시타입이든 참조타입이든) 반환할 수 있다.
- 리액트 컴포넌트도 함수다.

### 1. Return a single root element 

#### Fragment Component

### 2. Close all the tags 
### 3. camelCase

> JSX turns into JavaScript and attributes written in JSX become keys of JavaScript objects.

- 실제 DOM HTMLElement의 property에 상응하도록 React Component object(virtualDOM)의 Key를 구성 [(참고)](https://react.dev/reference/react-dom/components/common)

> For historical reasons, [`aria-*`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA) and [`data-*`](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes) attributes are written as in HTML with dashes.

- the evolution of web standards and the need for compatibility and consistency.

# 4. JavaScript in JSX with Curly Braces

- JSX에서 `{}` 안에 작성된 코드는 JavaScript로 평가된다. 

```
JSX는 객체를 반환하는데, 어떻게 평가하는걸까! 파싱하다가 {} 를 만나면...
```

# 5. Passing Props to a Component

> Props serve the same role as **arguments serve for functions**—in fact, props _are_ the only argument to your component! **React component functions accept a single argument, a `props` object**

> The default value is only used if the `size` prop is missing or if you pass `size={undefined}`. But if you pass `size={null}` or `size={0}`, the default value will **not** be used.

> the parent component will receive that content in a prop called `children`

- 자식 컴포넌트를 빈번하게 변경해야 할 때
- 자식 컴포넌트가 많을 때

```
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```

> **props are immutable.** 
> When a component needs to change its props (for example, in response to a user interaction or new data), it will have to “ask” its parent component to pass it different props—a new object!
> Its old props will then be cast aside, and eventually the JavaScript engine will reclaim the memory taken by them.

```
HOW 부모 컴포넌트에 변경된 props를 요청?
- 아마.. onChange 등등의 이벤트에 setState를 이벤트 핸들러로 등록해두고.. setState 실행 시 newState 객체를 생성하고 이걸 기반으로 Rerender (새롭게 props로 받음)
- props를 직접적으로 변경하는게 아니라, (view가 model을 변경하는 것이 아니라)
- data가 단방향으로 흐르도록 setState hook으로 model 변경
- hook -> model -> view
새로운 props object를 다시 받고, 기존 props object는 더이상 참조하지 않으므로 가비지 콜렉터가 먹음
```
