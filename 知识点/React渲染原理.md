* https://www.30secondsofcode.org/articles/s/react-rendering-basics
* https://blog.isquaredsoftware.com/2020/05/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior/

## 渲染过程
React 渲染会从组件树的root 开始一层一层向下移动，找到所有被标记为需要更新的组件，这些组件会根据当前的props和state描绘出想要的UI结构。 对于每一个被标记为需要更新的组件，React会调用它的 `render()` 方法（class组件） 或者
`FunctionComponent()` （function组件）。 然后`React.createElement()` 方法会将JSX结果转化为一个普通JS对象，即虚拟DOM.

在得到整个组件树的 render结果后，React会使用diff算法比较生成的虚拟DOM 和当前DOM树的区别，计算出DOM中哪些地方发生了变化，这个过程就叫做调和 reconciliation。在这之后，React就会把这些变化更新到DOM上

## Render和Commit阶段
从概念上说，这个分成两个阶段
* Render阶段：rendering组件，计算变化
* Commit阶段：把变化更新到DOM

Commit阶段完成后，React就会调用class组件的生命周期方法 `componentDidMount` 和 `componentDidUpdat`， 对function组件调用Hooks `useLayoutEffect` 和 `useEffect`

注意两点：
* rendering 不等于更新DOM
* 组件可能会在没有可见变化的情况下render

## 什么时候render
在最初的render完成后，这些操作会触发再一次render
* `this.setState()` class组件
* `this.forceUpdate()` class组件
* `useState()` setters function组件
* `useReducer` dispatches function组件
* `ReactDom.render()` 再次调用在root组件上

## rendering 行为
默认情况下，当父组件被render时，react就会递归地render所有的子组件。 这意味着React不管组件的 props有没有发生改变，只要父组件render，所有的子组件就会render.

换句话说，只要在根组件中不做任何修改调用`setState()`， react就会重新render组件树中的每一个组件。很可能大多数组件都没有发生任何改变，返回的render结果和上次的render结果一样。
但是rendering和diff计算都会发生，这很消耗性能。
