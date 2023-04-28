### Virtual DOM은 무엇인가요?

Virtual DOM (VDOM)은 UI의 이상적인 또는 “가상”적인 표현을 메모리에 저장하고 ReactDOM과 같은 라이브러리에 의해 “실제” DOM과 동기화하는 프로그래밍 개념입니다.

이 접근방식이 React의 선언적 API를 가능하게 합니다. React에게 원하는 UI의 상태를 알려주면 DOM이 그 상태와 일치하도록 합니다. 이러한 방식은 앱 구축에 사용해야 하는 어트리뷰트 조작, 이벤트 처리, 수동 DOM 업데이트를 추상화합니다.

“virtual DOM”은 특정 기술이라기보다는 패턴에 가깝기 때문에 사람들마다 의미하는 바가 다릅니다. React의 세계에서 “virtual DOM”이라는 용어는 보통 사용자 인터페이스를 나타내는 객체이기 때문에 [React elements](https://ko.legacy.reactjs.org/docs/rendering-elements.html)와 연관됩니다. 그러나 React는 컴포넌트 트리에 대한 추가 정보를 포함하기 위해 “fibers”라는 내부 객체를 사용합니다. 또한 React에서 “virtual DOM” 구현의 일부로 간주할 수 있습니다.

Virtual DOM은 실제 DOM과 같은 내용을 담고 있는 복사본이라고 생각하시면 쉽습니다. 복사본은 실제 DOM이 아닌 JS 객체형태로 메모리 안에 저장되어 있습니다.

Virtual DOM은 실제 DOM의 복사본이기 때문에, 실제 DOM의 모든 element과 속성을 공유합니다. 차이점은 브라우저에 있는 문서에 직접적으로 접근할 수 없다는 점인데요. 때문에 화면에 보여지는 내용을 직접 수정할 수 없다는 것입니다. 그렇다면 Virtual DOM은 왜 사용하는 것일까요?

리액트는 이를 굉장히 똑똑하게 사용하고 있는데요. 이를 통해 실제 DOM을 조작하는데 걸리는 시간을 획기적으로 줄여줍니다.

만약 폰트의 컬러만 바꾸고 싶어도, 다음과같이 DOM조작을 필요로 하게 됩니다.

document.querySelector(‘#title”).style.color = “red”;

브라우저는 HTML을 탐색해 해당 Element를 찾고, 해당 Element와 자식 Element들을 DOM에서 제거합니다. 이후 새롭게 수정된 Element로 이를 교체하는데요. CSS는 이 과정 이후 다시 계산하여 결과적으로는 레이아웃 정보를 알맞게 수정하게 되는 것이죠. 새롭게 계산된 내용이 브라우저에 그려지는 방식으로 플로우가 진행되는 것입니다.

사실 DOM조작은 트리에 있는 정보를 업데이트시켜준다는 점, 그리고 빠른 알고리즘을 사용한다는 조건 하에선 그렇게 퍼포먼스적으로 무리가 있는 작업은 아닙니다.

하지만 이를 반복적으로 수행한다면? 충분히 무거워질 수 있는 작업이 되는 것이죠.

이렇게 등장한 개념이 Virtual DOM이라는 것입니다.

앞서 말한 것 처럼, 가상 DOM은 실제 DOM과 내용을 공유하는 복사본입니다.

하지만 실제 DOM과 다르게 직접적으로 브라우저 화면의 UI를 조작할 수 있게 해주는 API는 제공하지 않습니다. 가상돔은 메모리에 저장되어 있는 자바스크립트 객체에 불과하기 때문이죠. 때문에 가상돔에 접근하고 수정하는 것은 매우 가볍고 빠른 작업이 되는 것입니다. _“실제 브라우저에 접근하는 것이 아니기 때문이죠.”_

**virtual DOM이 더 빠른가?**

정답은 _아니다!_ **정보 제공만 하는 웹 페이지**라면, 아무런 인터렉션이 발생하지 않기 때문에 DOM tree의 변화가 발생하지 않아서, **일반 DOM의 성능이 더 좋을 수 있다!** 그리고 실제 Virtual DOM의 diff 알고리즘이 빠르긴 하지만 추가적인 작업이 발생하기 때문에 필요에 맞게 도입을 해야한다. 하지만 SPA로 제작된 규모가 큰 웹페이지에서는 DOM 조작이 많이 발생하기 때문에 Virtual DOM을 사용해 브라우저의 연산량을 줄여 성능을 개선할 수 있다!!!

# 정리

데이터가 변경될 때 마다 DOM은 매번 처음부터 다시 화면이 그려지는 불필요한 반복이 발생된다. 그래서 DOM을 조작하는 Virtual DOM이 탄생하게 되었다. Virtual Dom은 변화가 실제 DOM에서 적용되기 전에 가상의 DOM을 거쳐서 계산 단계를 줄이고 효율적인 DOM조작을 가능하게 만들어준다. 만약 데이터가 변경되지 않는 웹 페이지라면 일반 DOM이 성능이 좋을 수 있고, 장바구니 등 사용자에 따라 데이터가 변경되는 웹페이지라면 DOM 조작이 많이 발생하기에 상황에 맞게 virtual DOM을 사용해야 한다!!!!

### **React의 Rendering**

**Triggering a render → Rendering the component → Commiting to the DOM**

### **Initial render**

대상 DOM 노드(root)로 **createRoot를 호출**한 다음 컴포넌트로 **render 메서드를 호출**한다.

### tip - createRoot

### React 18 createRoot()

createRoot를 호출하여 브라우저 DOM 엘리먼트 안에 콘텐츠를 표시하기 위한 React root를 생성합니다. React는 domNode에 대한 루트를 생성하고 그 안에 있는 DOM을 관리합니다. 루트를 생성한 후에는 root.render를 호출해 그 안에 React 컴포넌트를 표시해야 합니다:

### React18 createRoot()

```
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)

```

### 참고 - React18 이전의 createElement()

```
ReactDOM.render(React.createElement(CountButton), domContainer);

```

### createRoot()의 반환값

createRoot는 **render과 unmount**라는 두 가지 메서드가 있는 객체를 반환합니다. 앱이 서버 렌더링되는 경우 createRoot() 사용은 지원되지 않습니다. 대신 hydrateRoot()를 사용하세요.

### root.render(reactNode)

root.render를 호출하여 JSX 조각("React 노드")을 React 루트의 브라우저 DOM 노드에 표시합니다. root.render 은 undefined를 반환합니다.

```
//React는 루트에 <App />을 표시하고 그 안에 있는 DOM을 관리합니다.
root.render(<App />);

```

### root.render를 실행하면 일어나는 일

> root.render를 처음 호출하면 React는 React 컴포넌트를 렌더링하기 전에 React 루트 내부의 모든 기존 HTML 콘텐츠를 지웁니다.루트의 DOM 노드에 서버에서 또는 빌드 중에 React에 의해 생성된 HTML이 포함된 경우, 대신 이벤트 핸들러를 기존 HTML에 첨부하는 hydrateRoot()를 사용하세요.동일한 루트에서 렌더링을 두 번 이상 호출하면 React는 필요에 따라 DOM을 업데이트하여 사용자가 전달한 최신 JSX를 반영합니다. React는 이전에 렌더링된 트리와 "일치"시켜서 재사용할 수 있는 부분과 다시 만들어야 하는 부분을 결정합니다. 동일한 루트에서 렌더링을 다시 호출하는 것은 루트 컴포넌트에서 set 함수를 호출하는 것과 유사합니다.React는 불필요한 DOM 업데이트를 피합니다. - JQuery를 React내부에서 사용하지 말아야 할 이유

### root.unmount()

> root.unmount를 호출하여 React 루트 내부에서 렌더링된 트리를 삭제합니다. 이 함수는 React 루트의 DOM 노드(또는 그 조상 노드)가 다른 코드에 의해 DOM에서 제거될 수 있는 경우에 주로 유용합니다. 예를 들어, DOM에서 비활성 탭을 제거하는 jQuery 탭 패널을 상상해 보세요.탭이 제거되면 그 안에 있는 모든 것(내부의 React 루트를 포함)도 DOM에서 제거됩니다. 이 경우 root.unmount를 호출하여 제거된 루트의 콘텐츠 관리를 "중지"하도록 React에 지시해야 합니다. 그렇지 않으면 제거된 루트 내부의 컴포넌트가 구독과 같은 글로벌 리소스를 정리하고 확보하는 것을 알지 못합니다.root.unmount를 호출하면 루트에 있는 모든 컴포넌트가 unmount되고 트리의 이벤트 핸들러나 state를 제거하는 것을 포함해 루트 DOM 노드에서 React가 "분리"됩니다root.unmount는 undefined를 반환합니다.root.unmount를 호출하면 트리의 모든 컴포넌트가 unmount가 되고 루트 DOM노드에서 React가 분리됩니다.root.unmount를 호출한 후에는 같은 루트에서 root.render를 다시 호출할 수 없습니다. unmount된 루트에서 root.render를 호출하려고 하면 "unmount된 root를 업데이트할 수 없습니다." 오류가 발생합니다. 그러나 해당 노드의 이전 루트가 unmount된 후 동일한 DOM 노드에 대한 새 루트를 만들 수 있습니다.

### Usage

```
import { createRoot } from 'react-dom/client'; const root = createRoot(document.getElementById('root')); root.render(<App />);

```

### Rendering a page partially built with React

페이지가 React로 완전히 빌드되지 않은 경우, createRoot를 여러 번 호출하여 React가 관리하는 각 최상위 UI에 대한 루트를 생성할 수 있습니다. root.render를 호출하여 각 루트에 다른 콘텐츠를 표시할 수 있습니다.

여기서는 서로 다른 두 개의 React 컴포넌트가 index.html 파일에 정의된 두 개의 DOM 노드에 렌더링됩니다.

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + React + TS</title>
  </head>
  <body>
    <!-- <div id="root"></div> -->
    <div id="navigation"></div>
    <div id="comments"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>

```

```
import { createRoot } from 'react-dom/client';
import { Comments, Navigation } from './Components.js';
const navDomNode = document.getElementById('navigation');
const navRoot = createRoot(navDomNode as HTMLElement);
navRoot.render(<Navigation />);
const commentDomNode = document.getElementById('comments');
const commentRoot = createRoot(commentDomNode as HTMLElement);
commentRoot.render(<Comments />);

```

document.createElement()를 사용하여 새 DOM 노드를 생성하고 문서에 수동으로 추가할 수도 있습니다.

```
const domNode = document.createElement('div'); const root = createRoot(domNode); root.render(<Comment />); document.body.appendChild(domNode); // You can add it anywhere in the document

```

위의 기능은 React 컴포넌트가 다른 프레임워크로 작성된 앱 내부에 있는 경우에 주로 유용합니다.

### Rendering 과정

**1. 함수 컴포넌트 호출 - 첫 렌더링에서는 root componenet를 호출한다.**

**2. 구현부 실행**

-   props 취득, hook 실행, 내부 변수 및 함수 생성
    
-   단, hook 에 등록해둔 상태값, 부수함수 효과 등은 별도 메모리에 저장되어 관리된다.
    

**3. return 실행**

-   렌더링 시작

**4. 렌더 단계(Render Phase)**

-   가상DOM을 생성한다.

**5. 커밋 단계(Commit Phase)**

렌더링 간에 차이가 있는 경우에만 DOM 노드를 변경함.

-   실제 DOM에 반영한다.

**6. useLayoutEffect**

-   브라우저가 화면에 Paint 하기 전에, useLayoutEffect에 등록해둔 effect(부수효과함수)가 '동기'로 실행된다.
    
-   이 때, state, redux store 등의 변경이 있다면 한번 더 재렌더링 된다.
    

**7. Paint**

-   브라우저가 실제 DOM을 화면에 그린다. didMount가 완료된다.

**8. useEffect**

-   Mount되어 화면이 그려진 직후, useEffect에 등록해둔 effect(부수효과함수)가 '비동기'로 실행된다.

## batching

-   `state` 값이 변경되었을 경우 React 에서는 해당 컴포넌트를 리렌더링 하며, 불필요한 리렌더링을 방지하기 위해 state를 변경하는 작업을 **일괄적으로 처리**한다.
-   이렇게 `state` 의 업데이트 작업을 모아 일괄 처리하는 방식을 **Batching** 이라고 하며, 이 덕에 React 에서는 불필요한 리렌더링을 방지할 수 있게 되었다.

→ **React는 state 업데이트를 하기 전에 이벤트 핸들러의 모든 코드가 실행될 때까지 기다립니다.**

이렇게 하면 너무 많은 [리렌더링](https://react-ko.vercel.app/learn/render-and-commit#re-renders-when-state-updates)을 촉발하지 않고도 여러 컴포넌트에서 나온 다수의 state 변수를 업데이트할 수 있습니다. 하지만 이는 이벤트 핸들러와 그 안에 있는 코드가 완료될 때까지 UI가 업데이트되지 않는다는 의미이기도 합니다. 일괄처리(배칭, batching)라고도 하는 이 동작은 React 앱을 훨씬 빠르게 실행할 수 있게 해줍니다. 또한 일부 변수만 업데이트된 “반쯤 완성된” 혼란스러운 렌더링을 처리하지 않아도 됩니다.

### ****다음 렌더링 전에 동일한 state 변수를 여러 번 업데이트하기****

흔한 사례는 아니지만, 다음 렌더링 전에 동일한 state 변수를 여러 번 업데이트 하고 싶다면 `setNumber(number + 1)`와 같은 _다음 state 값_을 전달하는 대신, `setNumber(n => n + 1)` 와 같이 큐의 이전 state를 기반으로 다음 state를 계산하는 _함수_를 전달할 수 있습니다. 이는 단순히 state 값을 대체하는 것이 아니라 React에게 “state 값으로 무언가를 하라”고 지시하는 방법입니다.

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(n => n + 1);
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>+3</button>
    </>
  )
}
```

### **updator function 사용하기**

여기서 `n => n + 1` 는 **업데이터 함수(updater function)**라고 부릅니다. 이를 state 설정자 함수에 전달 할 때

1.  React는 이벤트 핸들러의 다른 코드가 모두 실행된 후에 이 함수가 처리되도록 큐에 넣습니다.
2.  다음 렌더링 중에 React는 큐를 순회하여 최종 업데이트된 state를 제공합니다.

업데이터 함수 인수의 이름은 해당 state 변수의 첫 글자로 지정하는 것이 일반적입니다:

```jsx
setEnabled(e => !e);setLastName(ln => ln.reverse());setFriendCount(fc => fc * 2);
```

### Automatic Batching 이란?(React 18에서 추가됨)

-   React 18 버전 이하에서는 오직 React 의 **이벤트 핸들러 내부의** state update 작업에 대해서만 Batching 이 가능했다. 하지만 Promise나 setTimeout, Native Event Handler 내부의 작업은 불가능했다.
-   왜냐하면 이전에는 **브라우저의 이벤트가 실행되는 중에만** Batching 작업을 수행했기 때문이다. 따라서 이벤트가 종료된 후에 실행되는 경우는 Batching 작업이 불가능했다.
-   하단의 코드의 경우 state update 작업이 비동기적으로 처리되어 Event가 종료된 후에 실행되기 때문에, React의 Batching 작업에 걸리지 않아 두 차례 리렌더링을 유발시켰다.

```jsx
function App() {
	const [count, setCount] = useState(0);
	const [flag, setFlag] = useState(false);

	function handleClick() {
		fetchSomething().then(() => {
			// React 17 이전의 버전에서는 해당 작업을 Batching 처리하지 않는다.
			// 왜냐하면 해당 작업은 이벤트가 종료된 이후 (100ms 뒤) 에 실행되기 때문이다.
			setCount((c) => c + 1); // 리렌더링 유발
			setFlag((f) => !f); // 리렌더링 유발
		});
	}

	return (
		<div>
			<button onClick={handleClick}>Next</button>
			<h1 style={{ color: flag ? "blue" : "black" }}>{count}</h1>
		</div>);
}

function fetchSomething() {
	return new Promise((resolve) => setTimeout(resolve, 100));
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

```jsx
function App() {
	const [count, setCount] = useState(0);
	const [flag, setFlag] = useState(false);

	function handleClick() {
		fetchSomething().then(() => {
			// React 18 이후에서는 해당 작업을 Batching 처리 한다.
			setCount((c) => c + 1);
			setFlag((f) => !f);
			// React 는 해당 작업을 일괄 처리하여 한 번의 리렌더링만 진행한다.
		});
	}

	return (
		<div>
			<button onClick={handleClick}>Next</button>
			<h1 style={{ color: flag ? "blue" : "black" }}>{count}</h1>
		</div>);
}

function fetchSomething() {
	return new Promise((resolve) => setTimeout(resolve, 100));
}

const rootElement = document.getElementById("root");
// React 18 에서 새롭게 제공하는 createRoot 메서드를 사용해야 한다!
ReactDOM.createRoot(rootElement).render(<App />);
```

-   하지만 React 18 버전 이후부터는 단순히 이벤트 핸들러 내부 뿐만이 아니라 Promise나 setTimeout, Native Event Handler 같은 작업에 대해서도 Batching 작업을 **자동으로** 수행하게 해주었다.
-   React 18 에서 제공하는 `ReactDOM.createRoot` 메서드를 기반으로 렌더링을 진행할 경우 모든 state update 작업은 자동으로 Batching 처리된다. 이 기능을 **Automatic Batching** 이라고 한다.

### ✏️ ReactDOM.flushSync() 란?

-   react-dom 라이브러리에 추가된 `ReactDOM.flushSync()` 메서드는 **Auto Batching 을 무시하고** 즉시 DOM을 렌더링해준다.
-   React 에서는 공식적으로 해당 메서드의 사용을 추천하진 않으며 (de-opt case), 필요한 상황이 있을 경우에만 사용할 것을 강조했다.

```
import { flushSync } from "react-dom";

function handleClick() {
	// React 는 flushSync 메서드가 실행되는 즉시 DOM을 업데이트 한다.
	flushSync(() => {
		setCounter((c) => c + 1);
	});
	// React 는 flushSync 메서드가 실행되는 즉시 DOM을 업데이트 한다.
	flushSync(() => {
		setFlag((f) => !f);
	});
	// 따라서 해당 함수가 실행될 경우 React는 총 두 번의 리렌더링을 수행한다.
}
```

React는 컴포넌트의 상태가 바뀌면 UI를 변경한다.

-   즉, state, props, redux store 등의 상태값이 변경되면, 리액트가 해당 컴포넌트 함수를 자동으로 재호출하여 재렌더링 해준다.

이 때, **가상DOM(Virtual DOM)**을 통해 **변경된 부분의 UI만 효율적으로 업데이트**한다.

-   가상DOM은 실제DOM을 분석하여 만든 Javascrip 객체이다.
    
-   컴포넌트의 상태값이 변경되면, 새로운 가상DOM 객체를 만들고, 이전 가상DOM 객체와 비교한다.
    
-   최종적으로 바뀐 부분이 있을 경우, 해당 부분만 실제 DOM에 반영하여 UI를 업데이트 한다.
    

-   가상DOM이란 실제DOM의 구조를 분석하여 메모리에 저장하고 관리하는 객체라고 볼 수 있다.
-   렌더링시마다 새로운 가상DOM을 생성하여, 상태값 변경 이전/이후 달라진 부분을 비교하는 매커니즘을 사용한다.

그렇다면 리액트는 어떤 식으로 가상돔을 활용해 보다 효율적으로 실제 DOM을 조작할까.

리액트는 항상 **두개의 가상돔 객체**를 가지고 있다.

1.  렌더링 이전 화면 구조를 나타내는 가상돔
2.  렌더링 이후에 보이게 될 화면 구조를 나타내는 가상돔

리액트는 state가 변경될 때마다 Re-Rendering이 발생하는데요. 이 시점마다 새로운 내용이 담긴

가상돔을 생성하게 된다. 실제 브라우저가 그려지기 이전에 말이죠.

### **React 컴포넌트 재렌더링 케이스**

-   React 컴포넌트는 아래의 4가지 상황에서 재렌더링 된다.

1.  내부 상태값(state) 변경
    
2.  부모가 전해준 속성(props) 변경
    
3.  중앙 상태값(redux store 등) 변경
    
4.  부모 컴포넌트가 재렌더링되는 경우, 자식 컴포넌트도 재렌더링.
    

-   컴포넌트가 렌더링되면, 해당 컴포넌트 함수가 호출되어 화면을 다시 그리는데, 이 과정을 정리해둔다.

### **React 재렌더링 과정**

**1. 함수 컴포넌트 재호출**

**2. 구현부 실행**

-   props 취득, hook 실행, 내부 변수 및 함수 **재생성**
    
-   단, 각 hook의 특성에 따라 기존에 메모리에 저장한 내용을 적절히 활용한다.
    

**3. return 실행**

-   렌더링 시작

**4. 렌더 단계(Render Phase)**

-   새로운 가상DOM 생성 후, 이전 가상 DOM과 비교하여, 달라진 부분을 탐색하고, 실제 DOM에 반영할 부분을 결정한다.

**5. 커밋 단계(Commit Phase)**

-   달라진 부분만 실제 DOM에 반영한다.

**6. useLayoutEffect**

-   브라우저가 화면에 Paint 하기 전에, useLayoutEffect에 등록해둔 effect(부수효과함수)가 '동기'로 실행된다.
    
-   이 때, state, redux store 등의 변경이 있다면 한번 더 재렌더링 된다.
    

**7. Paint**

-   브라우저가 실제 DOM을 화면에 그린다. didUpdate가 완료된다.

**8. useEffect**

-   update되어 화면이 그려진 직후, useEffect에 등록해둔 effect(부수효과함수)가 '비동기'로 실행된다.
    
-   effect에 return부분이 있다면, 구현부보다 먼저 실행된다.