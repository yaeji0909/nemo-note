#### React 18 createRoot()
createRoot를 호출하여 브라우저 DOM 엘리먼트 안에 콘텐츠를 표시하기 위한 React root를 생성합니다.
React는 domNode에 대한 루트를 생성하고 그 안에 있는 DOM을 관리합니다. 루트를 생성한 후에는 root.render를 호출해 그 안에 React 컴포넌트를 표시해야 합니다:

#### React18 createRoot() 
```javascript
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

#### 참고 - React18 이전의 createElement() 
```javascript
ReactDOM.render(React.createElement(CountButton), domContainer);
```


#### createRoot()의 반환값
createRoot는 **render과 unmount**라는 두 가지 메서드가 있는 객체를 반환합니다.
앱이 서버 렌더링되는 경우 createRoot() 사용은 지원되지 않습니다. 대신 hydrateRoot()를 사용하세요.

#### root.render(reactNode)
root.render를 호출하여 JSX 조각("React 노드")을 React 루트의 브라우저 DOM 노드에 표시합니다.
root.render 은 undefined를 반환합니다.
```javascript
//React는 루트에 <App />을 표시하고 그 안에 있는 DOM을 관리합니다.
root.render(<App />);
```

#### root.render를 실행하면 일어나는 일
> -  `root.render`를 처음 호출하면 React는 React 컴포넌트를 렌더링하기 전에 React 루트 내부의 모든 기존 HTML 콘텐츠를 지웁니다. 
> 
> - 루트의 DOM 노드에 서버에서 또는 빌드 중에 React에 의해 생성된 HTML이 포함된 경우, 대신 이벤트 핸들러를 기존 HTML에 첨부하는 `hydrateRoot()`를 사용하세요.
> 
> - 동일한 루트에서 렌더링을 두 번 이상 호출하면 React는 필요에 따라 DOM을 업데이트하여 사용자가 전달한 최신 JSX를 반영합니다. **React는 이전에 렌더링된 트리와 "일치"시켜서 재사용할 수 있는 부분과 다시 만들어야 하는 부분을 결정**합니다. 동일한 루트에서 렌더링을 다시 호출하는 것은 루트 컴포넌트에서 `set` 함수를 호출하는 것과 유사합니다.
> 
> - React는 불필요한 DOM 업데이트를 피합니다. - JQuery를 React내부에서 사용하지 말아야 할 이유

### root.unmount()
>- root.unmount를 호출하여 React 루트 내부에서 렌더링된 트리를 삭제합니다. 이 함수는 React 루트의 DOM 노드(또는 그 조상 노드)가 다른 코드에 의해 DOM에서 제거될 수 있는 경우에 주로 유용합니다. 예를 들어, DOM에서 비활성 탭을 제거하는 jQuery 탭 패널을 상상해 보세요. 
>
>- 탭이 제거되면 그 안에 있는 모든 것(내부의 React 루트를 포함)도 DOM에서 제거됩니다. 
>이 경우 root.unmount를 호출하여 제거된 루트의 콘텐츠 관리를 "중지"하도록 React에 지시해야 합니다. 그렇지 않으면 제거된 루트 내부의 컴포넌트가 구독과 같은 글로벌 리소스를 정리하고 확보하는 것을 알지 못합니다.
>
>- root.unmount를 호출하면 루트에 있는 모든 컴포넌트가 unmount되고 트리의 이벤트 핸들러나 state를 제거하는 것을 포함해 루트 DOM 노드에서 React가 "분리"됩니다
>- root.unmount는 undefined를 반환합니다.
>- root.unmount를 호출하면 트리의 모든 컴포넌트가 unmount가 되고 루트 DOM노드에서 React가 분리됩니다.
>- root.unmount를 호출한 후에는 같은 루트에서 root.render를 다시 호출할 수 없습니다. unmount된 루트에서 root.render를 호출하려고 하면 "unmount된 root를 업데이트할 수 없습니다." 오류가 발생합니다. 그러나 해당 노드의 이전 루트가 unmount된 후 동일한 DOM 노드에 대한 새 루트를 만들 수 있습니다.


### Usage
```javascript
import { createRoot } from 'react-dom/client'; const root = createRoot(document.getElementById('root')); root.render(<App />);
```



### Rendering a page partially built with React
페이지가 React로 완전히 빌드되지 않은 경우, createRoot를 여러 번 호출하여 React가 관리하는 각 최상위 UI에 대한 루트를 생성할 수 있습니다. root.render를 호출하여 각 루트에 다른 콘텐츠를 표시할 수 있습니다.

여기서는 서로 다른 두 개의 React 컴포넌트가 index.html 파일에 정의된 두 개의 DOM 노드에 렌더링됩니다.
```javascript
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

```javascript
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

```javascript
const domNode = document.createElement('div'); const root = createRoot(domNode); root.render(<Comment />); document.body.appendChild(domNode); // You can add it anywhere in the document
```

위의 기능은 React 컴포넌트가 다른 프레임워크로 작성된 앱 내부에 있는 경우에 주로 유용합니다.