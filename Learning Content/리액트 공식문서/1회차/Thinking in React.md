# Step 1: Break the UI into a component hierarchy

# Step 2: Build a static version in React

> The component at the top of the hierarchy will take your data model as a prop. This is called _one-way data flow_ because the data flows down from the top-level component to the ones at the bottom of the tree.

# Step 3: Find the minimal but complete representation of UI state

> Think of state as the minimal set of changing data that your app needs to remember. The most important principle for structuring state is to keep it [DRY (Don’t Repeat Yourself).](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) Figure out the absolute minimal representation of the state your application needs and compute everything else on-demand. For example, if you’re building a shopping list, you can store the items as an array in state. If you want to also display the number of items in the list, don’t store the number of items as another state value—instead, read the length of your array.

- State : The **minimal** set of changing data, the absolute minimal representation of the state.
	- NewsList를 페이지 별로 나눠서 저장해두려고 했음 -> X

### Props vs State

> A parent component will often keep some information in state (so that it can change it), and _pass it down_ to child components as their props.

- 상위 컴포넌트에서는 state로 가지고 상위 컴포넌트에서 변경함
- 하위 컴포넌트에서는 상위 컴포넌트의 state를 props로 전달받아 사용함

# Step 4: Identify where your state should live 

> Remember: React uses **one-way data flow**, passing data down the component hierarchy from parent to child component.

# Step 5: Add inverse data flow 
