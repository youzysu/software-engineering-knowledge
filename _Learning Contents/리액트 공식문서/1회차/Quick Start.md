## Creating and nesting components 

1. 리액트 컴포넌트는 대문자로 시작한다.
2. 하나의 태그만 반환해야 한다.

## Rendering lists

> For each item in a list, you should pass a string or a number that **uniquely identifies that item** among its siblings. 
> React uses your keys to know what happened if you later insert, delete, or reorder the items.

- 여전히 의문: 왜 id가 꼭 필요할까?
> Keys let React **keep track of each item’s place** in the list even if the list changes.