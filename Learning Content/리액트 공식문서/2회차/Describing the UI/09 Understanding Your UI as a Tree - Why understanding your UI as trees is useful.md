### Key takeaways
- **Trees**: a common way to **represent the relationship between entities.** 
	- to model UI (View Model)
- **Render trees**: a representation of **the nested relationship** between React components **across a single render.**
- **top-level and leaf components**
	- **Top-level components**: affect the rendering performance of all components beneath them.
	- **leaf components**: often re-rendered frequently. 
	- useful for understanding and debugging rendering performance.
- **Dependency trees**: represent the module dependencies in a React app.
	- used by build tools **to bundle the necessary code** to ship an app.
	- useful for **debugging large bundle sizes** that slow time to paint and expose opportunities for **optimizing what code is bundled.**

## 9-1. How React “sees” component structures: Your UI as a tree

> use trees to represent their **view hierarchy**.
- ex. HTML → DOM, CSS → CSSOM

## 9-2. What a render tree is and what it is useful for: The Render Tree 

> nest components: to compose components of other components.
> model this relationship in a render tree

> Top-level components are the components nearest to the root component and affect the rendering performance of all the components beneath them and often contain the most complexity. 
> 
> Leaf components are near the bottom of the tree and have no child components and are often frequently re-rendered.

- This categorization of components is useful for understanding **data flow and rendering performance.**

## 9-3. What a module dependency tree is and what it is useful for: The Module Dependency Tree

> Dependency trees are useful to determine **what modules are necessary** to run your React app.

> bundlers will use the dependency tree **to determine what modules should be included.**

> Getting a sense of your app’s dependency tree may help with debugging the issues that delay time for UI to get drawn (bundle size grows → expensive for a client to download and run)