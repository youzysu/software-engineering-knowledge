> [Build your own React](https://pomb.us/build-your-own-react/)

# 1. The `createElement` Function

- Return: an object with 2 properties type, config
1. type
	- tagName: a string that specifies the type of the DOM node to create
2. config
	- object that has all the keys and values 

### 참고: [ReactElement](https://github.dev/facebook/react/blob/f4cc45ce962adc9f307690e1d5cfa28a288418eb/packages/react/src/ReactElement.js)

#### ReactElement
- Params: `type, key, ref, self, source, owner, props`
- Return: `element` 
- 내부에서 `Object.defineProperty()` 를 활용하여 element라는 객체의 property 속성을 인자로 받은 정보를 바탕으로 설정한 element를 반환한다.
	```js
	element = {
	$$typeof: REACT_ELEMENT_TYPE,
	type: type,
	key: key,
	ref: ref,
	props: props,
	_owner: owner,
	};
	```

#### createElement 
- Params: `type, config, children` 
- Return: `ReactElement`
- 전달받은 인자를 바탕으로 ReactElement의 인자로 한다.

# 참고자료 

- [나만의 리액트 라이브러리 만들기](https://bluewings.github.io/build-your-own-react/)
- [나만의 리액트 라이브러리 만들기](https://velog.io/@godori/build-your-own-react)