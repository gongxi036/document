#  `useEffect` 的执行时机

###  示例1

```jsx
import { useState, useEffect } from 'react'

function App() {
  const [count, setCount] = useState(0)

  // render 方法调用时被触发
  console.log(1)

  // useEffect 方法调用时被触发
  useEffect(() => {
    console.log(2)
  }, [])

  Promise.resolve().then(() => {
    console.log(3)
  })

  setTimeout(() => {
    console.log(4)
  }, 0)

  return (
    <>
      <div>
        <h1>useEffect 执行时机</h1>
        <p>count: {count}</p>
      </div>
    </>
  )
}

export default App

```

####  输出结果

```tex
1
2
3
4
```

####  解析

**非交互行为产生的 `useEffect` 执行机制**

对于 `示例1` 中，由于渲染并没有消耗太多的时间，渲染前会尽量执行`useEffect`

```js
// 阻塞代码执行
const flag = Date.now();
while (Date.now() - flag < 1000) {
  // block render 100ms
}
```

**如果在 `示例1` 中加入上述阻塞代码，渲染则会消耗大部分的时间，则 `useEffect` 会在渲染之后执行**

####  加入阻塞代码 输出结果

```tex
1
3
4
2
```

###  示例2

```jsx
import { useEffect, useState } from 'react';

function App() {
  const [state, setState] = useState(0);

  // App Render Log
  console.log(1);

  const flag = Date.now();
  while (Date.now() - flag < 100) {
    // block render 100ms
  }

  // useEffect log
  useEffect(() => {
    console.log(2);
  }, [state]);

  // micro callback log
  Promise.resolve().then(() => console.log(3));

  // macro callback log
  setTimeout(() => console.log(4), 0);

  return (
    <div>
      <button onClick={() => setState((state) => state + 1)}>
        点击更新 State
      </button>
      <div onMouseEnter={() => setState(10)}>点击更新 State {state}</div>
    </div>
  );
}

export default App;
```

####   输出结果

```tex
1      1
2      3
3      4
4      2
```

> 1 3 4 2 的执行结果是因为，示例中添加了阻塞代码

####  解析

**对于交互行为 `useEffect` 的执行时机**

在事件体系中可以将不同的事件分为**离散型事件**和**非离散型事件**。

> 离散事件也就意味着每个事件都是用户单独意图触发的，比如点击事件，每一次点击都是用户单独意图触发的，假使用户点击两次，那么的确是因为用户有明确意图触发了两次点击	

> 非离散型事件，也被称之为"连续事件"。类似于 `onMouseEnter `事件。事件的多次触发并不是用户有意触发，站在用户角度来说用户并不关心执行了多少次 mouseEnter(mousemove) 事件，在用户的角度上仅仅是滑动过鼠标而已。这类事件 React 团队称之为 "continuous"，重要的是最新的事件而非发生了多少次，这类事件统一被称为非离散型（连续）事件。	

**对于离散型事件导致的 `useEffect` 执行，React 内部会在渲染前同步执行**

**对于连续性输入(非离散型)事件导致 `useEffect`执行，React 内部会按照非交互行为处理方式：如果渲染时间充足，则会尽可能的在渲染之前执行。**



###  参考地址

[地址](https://cloud.tencent.com/developer/article/2418672)