# 04 Preserving and Resetting State
### Key takeaways
- React **keeps state** for as long as **the same component** is rendered at **the same position**.
	- the same component: same type, same key
- State is associated with **the tree position** in which you put that JSX.
- giving component **a different key** forces a subtree to **reset its state.**