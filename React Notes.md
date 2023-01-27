## React Notes

- [React Notes](#react-notes)
- [Intro](#intro)
- [Setup](#setup)
  - [npm vs yarn](#npm-vs-yarn)
  - [modify JS project](#modify-js-project)
- [React](#react)
  - [CSS Module](#css-module)
  - [Load Images and Fonts](#load-images-and-fonts)
  - [Create Class Component](#create-class-component)
  - [Props and State](#props-and-state)
  - [React Event](#react-event)
  - [Asynchronous](#asynchronous)
  - [Lifecycle](#lifecycle)
- [React Hooks](#react-hooks)
  - [useState](#usestate)
  - [useEffect](#useeffect)
  - [useContext](#usecontext)
  - [useReducer: manage global state](#usereducer-manage-global-state)
  - [useCallback: handle side effect of callback](#usecallback-handle-side-effect-of-callback)
  - [useRef: return a reference, remain unchanged during whole lifecycle](#useref-return-a-reference-remain-unchanged-during-whole-lifecycle)
  - [useLayoutEffect: 在所有DOM变更之后同步调用，读取DOM布局并同步出发重新渲染](#uselayouteffect-在所有dom变更之后同步调用读取dom布局并同步出发重新渲染)
  - [useDebugValue: 在React开发者工具中显示自定义的hook标签](#usedebugvalue-在react开发者工具中显示自定义的hook标签)
- [Higher-Order Component](#higher-order-component)
- [Others](#others)
- [Pending](#pending)

## Intro

10 pages, 20 components, 4 function modularity: product browsing, user login, chart and order system

Generated API documents using Postman

## Setup

```shell
npx create-react-app my-app
npx create-react-app my-app-ts --template typescript
```

> npx is a package runner tool that comes with npm 5.2+.

eject

> Note: this is a one-way operation. Once you `eject`, you can't go back!

弹出所有配置文件，同时会对项目造成不可逆的结构改变（permanent）

jsx allows us to use html syntax in JS

TypeScript add static type check to original JavaScript.

Compiler: ts-loader, awesome-typescript-loader and babel-loader

### npm vs yarn

1. install dependencies from `package.json`: `npm install` or `yarn`
2. add/install dependency to `package.json`: `npm install {libraryName} --save` or `yarn add {libraryName}`
3. add/install devDependency to `package.json`: `npm install {libraryName} --save-dev` or `yarn add {libraryName} --dev`
4. delete dependency: `npm uninstall package --save` or `yarn remove package`
5. update dependency: `npm update --save` or `yarn upgrade`
6. install dependency globally: `npm install package -g` or `yarn global add package`

### modify JS project

```shell
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
// @types/react: react ts interface definition (typing)
```

change `.js` or `jsx` files to `.tsx` files

```ts
// tsconfig.json
{
  "compilerOptions": {
    "noImplicitAny": false,
    "target": "es5", // es5, es6/es2015, es2016, es2017 ... esnext
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true, // allow importing using commonjs way
    // import React from 'react'
    // "esModuleInterop": false, import * as React from 'react'
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext", // module system: Node.js CommonJS, ES6 esnext and requirejs AMD
    "moduleResolution": "node", // 'node' or 'classic' (abandoned)
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": [
    "src"
    // "**": any child direction, "*": any file name, "?": character can be omitted.
  ]
  // "file": [
  //   "./file1.ts",
  //   "./file2.ts"
  // ],
  //   "exclude": [
  //     "node_modules",
  //     "**//.test/ts"
  //   ]
}
```

## React

1. What is React?
2. What are the Pros. and Cons of React?
3. What si JSX?
4. What is Virtual DOM?
5. What is Component?
6. What is the deference between State and Props?
7. What is the life cycle of React Component?

Ajax -> AngularJS MVC (Model/View/Controller) ->

Cons of AngularJS (version 1):

1. two-way data binding
2. website run slowly
3. MVC architecture: page state management confusion

Virtual DOM

State Change -> Computer Diff -> Re-render

为什么使用 JSX？

1. React 并不强制使用 JSX，也可以使用原生 JS
2. React 认为视图的本质就是渲染逻辑与 UI 视图表现的内在统一
3. React 把 HTML 与渲染逻辑进行了耦合，形成了 JSX

JSX 的特点：

1. 常规的 HTML 代码都可以与 JSX 兼容
2. 可以在 JSX 中嵌入表达式
3. 使用 JSX 指定子元素
4. JSX 的命名约定
   1. camelCase
   2. 自定义属性，以 `data-` 开头 `const element = <div data-customized={'自定义属性'}></div>;`
5. JSX 表示对象：JSX 会被编译为 React.createElement() 对象

```jsx
const element = React.createElement(
  "h1", // type
  { className: "greeting" },
  "hello, world!"
);

const element = <h1 className="greeting">Hello, world!</h1>;
```

### CSS Module

(CSS 模块化，以及 JS 中 CSS 的对象化和自动联想)

import CSS file directly

有可能造成 CSS 样式的全局污染和冲突

```ts
import "./index.css";
<div className="app"></div>;
```

CSS in JS (JSS) material-ui

replace `import './index.css'` with `import styles from './index.css'`

隔离了 CSS 样式的全局冲突；但是 className 由 JS 动态生成可能会对网站调试造成麻烦

```ts
// add file custom.d.ts
declare module "*.css" {
  const css: { [key: string]: string };
  export default css;
}
```

convert CSS into an **object** and import it as a module

```ts
import styles from "./index.css";
<div className={styles.app}></div>;
```

`npm install --save-dev typescript-plugin-css-modules`

add `"plugins": [{"name": "typescript-plugin-css-modules"}]` into `compilerOptions` in `tsconfig.json` file

### Load Images and Fonts

`npm install react-icons`

```ts
import React from "react";
import logo from "./logo.png"; // Tell webpack this JS file uses this image

console.log(logo); // /logo.84287d09.png

function Header() {
  // Import result is the URL of your image
  return <img src={logo} alt="Logo" />;
}

export default Header;
```

or

Adding SVGs

> Note: this feature is available with react-scripts@2.0.0 and higher, and react@16.3.0 and higher.

```ts
import { ReactComponent as Logo } from "./logo.svg";

function App() {
  return (
    <div>
      {/* Logo is an actual React component */}
      <Logo />
    </div>
  );
}
```

Since fonts are global styles, we can import them in `index.css` file.

```css
@font-face {
  font-family: "Slidefu";
  src: local("Slidefu"),
    url(./assets/fonts/Slidefu-Regular-2.ttf) format("truetype");
}
```

```css
/* in *.module.css */
h1 {
  font-family: "Slidefu";
  font-size: 72px;
}
```

### Create Class Component

```ts
import React from "react";
import styles from "./shoppingCart.module.css";

interface Props {}

interface State {
  isOpen: boolean;
}

class ShoppingCart extends React.Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = {
      isOpen: false,
    };
  }

  render() {
    return (
      <div className={styles.cartContainer}>
        <button
          className={styles.button}
          onClick={() => {
            this.setState({ isOpen: !this.state.isOpen });
          }}
        >
          cart contains 2 items.
        </button>
        <div
          className={styles.cartDropDown}
          style={{ display: this.state.isOpen ? "block" : "none" }}
        >
          <ul>
            <li>item1</li>
            <li>item2</li>
          </ul>
        </div>
      </div>
    );
  }
}

export default ShoppingCart;
```

### Props and State

Props stands for properties. They are immutable.

using `setState()` modify State

After call `setState()`, state will not change immediately, it is an asynchronous process.

### React Event

开启 any 参数类型的自动映射：tsconfig.json->compilerOptions->`"noImplicitAny": false`

```ts
class ShoppingCart extends React.Component<Props, State> {
  construct(props: Props) {
    super(props);
    this.state = {
      isOpen: false,
    };
    // this.handleClick = this.handleClick.bind(this);
  }
  // handleClick(e: React.MouseEvent<HTMLButtonElement, MouseEvent>) {
  //   this.setState({ isOpen: !this.state.isOpen });
  // }

  handleClick = (e: React.MouseEvent<HTMLButtonElement, MouseEvent>) => {
    this.setState({ isOpen: !this.state.isOpen});
  }

  render() {
    reture (
      <div>
      <button
        onClick={this.handleClick}
        onClick={(e) => this.handleClick(e)}
      >
      </button>
      </div>
    )
  }
}
```

`target` 描述的是事件发生的元素

`currentTarget` 描述的是事件处理绑定的元素

### Asynchronous

```ts
componentDidMount() {
  fetch('https://jsonplaceholder.typicode.com/users')
  .then((response) => response.json())
  .then(data => this.setState( {robotGallery: data} ))
}
```

`setState()` 同步执行，异步更新：React 会将多个 `setState` 的调用合并为一个来执行，也就是说，当执行 `setState` 的时候，`state` 中的数据并不会马上更新。

正是因为 `setState` 异步操作的特殊性，当我们使用 `this.state` 的时候只要页面没有更新，那么 `state` 的状态就依然会停留在上一次操作中。

`setState()` 函数有两个参数：

- 第一个参数接收一个对象类型，使用对象来更新 `state`
- ，第二个参数就是组件 `state` 的异步操作处理。在回调函数中，我们可以实时的获取到更新之后的数据。
- 但是第一个参数不仅可以使用对象，同时也可以像第二个参数一样传入函数，取得上一个状态的信息

```ts
this.setState((preState, preProps) => ({ count: preState.count + 1 }));
```

### Lifecycle

React Lifecycle

- Mounting: create virual DOM element, render UI
- Updating: update DOM element, re-render UI
- Unmounting: delete virual DOM element

`static getDerivedStateFromProps` 是一个静态函数，接收两个参数，nextProps 和 prevState，在初始化和组件更新的时候都会被调用，作用就是用来对比当前 props 和之前 props 的变化

`shouldComponentUpdate` 也接收两个参数，nextProps 和 prevState，通过判断  props 和 state 的变化控制组件是否被更新

1. `Initialize -> construct -> getDerivedStateFromProps -> render -> componentDidMount`：在初始化阶段，最一开始react会调用构建函数来初始化state，接着getDerivedStateFromProps函数将会被调用，这个函数会探测state和props是否有变化;然后，我们的render函数执行了，ui就显示出来了;ui渲染完毕，componentDidMount 函数就执行。react组件的初始化现在就结束了。

2. `getDerivedStateFromProps -> shouldComponentUpdate -> render -> componentDidUpdate`：接下来，只要组件中的props或着state有变化，我们的组件就进入了更新阶段，首先执行的是getDerivedStateFromProps函数，用来探测state和props是否有变化;接着执行shouldComponentUpdate，我们可以在这里判断组件是否需要更新;如果需要更新，那么render函数就会执行，UI就会根据state或者props的变化来重新染，渲染完成以后，componentDidUpdate就会被执行，我们可以在这里处理一些组件更新后的逻辑。

3. `componentWillUnmount -> destroy`：最后，当这个组件使命结束以后，组件就会消亡，这时componentWilUnmount函数就会被调用，我们在这里回收内存、删除事件监听，等等。处理完成以后，组件才会被最终销毁。

## React Hooks

[React Hooks](https://reactjs.org/docs/hooks-reference.html)

在大型的 react 项目中，很多 react 组件会变得冗长而复杂，尤其是与 redux 全局状态连接后不可复用。两种解决方案： 无状态组件（没有生命周期、没有副作用、无法访问 API 进行异步数据更新）和 HOC 高阶组件都有缺陷。

### useState

return value is an array with two elements: [state, stateUpdateFunction]

```ts
const [count, setCount] = useState(0);
```

### useEffect

Pure Function: same input, same output

Side Effect: 指一个函数处理了与返回值无关的事情，如修改了全局变量、修改了传入的参数、使用了 Ajax 进行 API 调用、修改了 DOM 元素、console.log

```ts
import React, { useState, useEffect } from "react";

interface Props {
  username: string
}

const App: React.FC<Props> = (props) => {
  const [count, setCount] = useState<number>(0);
  const [robotGallery, setRobotGallery] = useState<any>([]);
  const [loading, setLoading] = useState<boolean>(false);
  // show loading state
  const [error, setError] = useState<string>();
  
  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
    .then(response => response.json())
    .then(data => setRobotGallery(data)
  }, [])
  
  return (
    <div className={styles.app}>
      <h2>{props.username}</h2>
      <Child username={props.username} />
    </div>
  )
}
```

async & await

```ts
// 想要调用 await 关键词，函数本身要是一个 promise
useEffect(() => {
  const fetchData = async () => {
    setLoading(true);
    // handle error
    try {
      const responses = await fetch(
        "https://jsonplaceholder.typicode.com/users"
      );
      const data = await responses.data;
      setRobotGallery(data);
      setLoading(false);
    } catch(e) {
      if (e instanceof Error) {
        setError(e.message);
      }
    };
  };
  fetchData();
}, [])
// condition
{(error || error !== "") && <div>error: {error}</div>}
```

使用空数组 `[]` 相当于模拟类组件的生命周期函数 componentDidMount，Effect 仅会在第一次挂载 UI 的时候起作用

不使用第二个参数，相当于模拟生命周期 `componentDidUpdate`

### useContext

handle cross-component data transmit: 允许组件之间共享数据，而不是通过 prop drilling 的方式

useContext 减少了模板代码，减少了代码的层级，消灭了多个consumer互相嵌套的可能系

```ts
// index.tsx
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

const defaultContextValue = {
  username: "James",
};

// notice export
export const appContext = React.createContext(defaultContextValue);

ReactDOM.render(
  <React.StrictMode>
    <appContext.Provider value={defaultContextValue}>
      <App />
    </appContext.Provider>
  </React.StrictMode>
)
```

```ts
// components/Robot.tsx
import React, { useContext } from "react";
import { appContext } from "../index.tsx";

interface RobotProps {
  id: number;
  name: string;
  email: string;
}
const Robot: React.FC<RobotProps> = ({id, name, email}) => {
  const value = useContext(appContext)
  return (
   <div>
     <p>Author: {value.username}</p>
   </div>
  )
}
```

组件化 Context Provider

```ts
import React, { PropsWithChildren, useState } from "react";

interface AppStateValue {
    username: string;
    shoppingCart: { items: {id: number, name: string}[] }
}

const defaultContextValue: AppStateValue = {
    username: "James",
    shoppingCart: { items: [] }
}

export const appContext = React.createContext(defaultContextValue);

export const AppStateProvider: React.FC<PropsWithChildren<{}>> = (props) => {
    const [state, setState] = useState(defaultContextValue);
    return (
        <appContext.Provider value={state}>
            {props.children}
        </appContext.Provider>
    )
}
```

### useReducer: manage global state

### useCallback: handle side effect of callback

### useRef: return a reference, remain unchanged during whole lifecycle

### useLayoutEffect: 在所有DOM变更之后同步调用，读取DOM布局并同步出发重新渲染

### useDebugValue: 在React开发者工具中显示自定义的hook标签

## Higher-Order Component

`const EnhancedComponent = higherOrderComponent(WarappedComponent);`

`withXXX()`

1. 抽取重复代码，实现组件复用
2. 条件渲染，控制组件的渲染逻辑
3. 捕获/劫持被处理组件的生命周期

```ts
// AddToCart.tsx
import React from "react";
import { addStateContext } from "../AppState";
import { RobotProps } from "./Robot";

export const withAddToCart = (ChildComponent: React.ComponentType<RobotProps>) => {
  // React.ComponentType<P> is an alias for React.FunctionComponent<P> | React.ClassComponent<P>
  // return class extends React.Component {}
  return (props) => {
    const setState = useContext(appSetStateContext)
    const addToCart = (id, name) => {
      if (setState) {
        setState((state) => {
          return {
            ...state,
            shoppingCart: {
              items: [...state.shoppingCart.items, { id, name }],
            },
          };
        });
      }
    }
    return <ChildComponent {...props} addToCart={addToCart}/>
  }
}
```

```ts
// Robot.tsx
import React, { useContext } from "react";
import styles from "./Robot.module.css";
import { appContext, appSetStateContext } from "../AppState";
import { withAddToCart } from "./AddToCart";

export interface RobotProps {
  id: number;
  name: string;
  email: string;
  addToCart: (id, name) => void;
}

const Robot: React.FC<RobotProps> = ({ id, name, email, addToCart }) => {
  const value = useContext(appContext);
  return (
    <div classname={styles.cardContainer}>
      <button onClick={() => addToCart(id, name)}>Add to Cart</button>
    </div>
  );
}
export default withAddToCart(Robot);
```

Customized Hook

```ts
export const useAddToCart = () => {
  const setState = useContext(appSetStateContext)
  const addToCart = (id, name) => {
    if (setState) {
     setState((state) => {
       return {
         ...state,
         shoppingCart: {
           items: [...state.shoppingCart.items, { id, name }],
         },
       };
     });
   }
  }
  return addToCart;
}
```

## Others

footnotes <sup><a href="#note1">[1]</a></sup>

<a name="note1"></a> [how-to-add-footnotes-to-github-flavoured-markdown](http://stackoverflow.com/questions/25579868/how-to-add-footnotes-to-github-flavoured-markdown)

## Pending

fetch status data

promise

git rollback
