###### `useContext`는 컴포넌트에서 [context](https://react-ko.vercel.app/learn/passing-data-deeply-with-context)를 읽고 구독할 수 있게 하는 hook

### context api란?

> react는 16.3 버전부터 정식적으로 [context api (opens new window)](https://reactjs.org/docs/context.html)를 지원하고 있습니다. 일반적으로 부모와 자식간 props를 날려 state를 변화시키는 것과는 달리 context api는 컴포넌트 간 간격이 없습니다. 즉, 컴포넌트를 건너띄고 다른 컴포넌트에서 state, function을 사용할 수 있습니다. 또한 redux의 많은 어려운 개념보다 context api는 `Provider`, `Consumer`, `createContext` 개념만 알면 적용가능합니다.

### 언제 쓰는가

context는 컴포넌트안에서 전역적으로 데이터를 공유하도록 나온 개념입니다. 그런 데이터는 로그인 데이터, 웹 내 사용자가 쓰는 설정파일, 테마, 언어 등등 다양하게 컴포넌트간 공유되어야할 데이터로 사용하면 좋습니다.


## API

-   api에 대한 정보만 파악하시고 하단에 예제가 있으니 그때 따라하면서 습득하시면 좋을 것 같습니다.

### [#](https://kyounghwan01.github.io/blog/React/react-context-api/#react-createcontext)React.createContext

```
const MyStore = React.createContext(defaultValue);
```

-   context 객체를 만듭니다. 컴포넌트가 이 context를 가지려면 해당 컴포넌트 상위에 `provider`로 부터 context를 정의한 변수 `myStore`를 감싸면 됩니다
-   `defaultValue` param은 트리 안에 적절한 provider를 찾지 못했을 때 쓰이는 값입니다. (해당 store가 어떠한 provider에 할당되지 않은 경우)완전 독립적인 context를 유지할때 쓰입니다, provider를 통해 undefined를 보낸다 해도 해당 context를 가진 컴포넌트는 provider를 읽지 못합니다.

### [#](https://kyounghwan01.github.io/blog/React/react-context-api/#context-provider)Context.Provider

```
<MyStore.Provider value={this.state}>
  <subComponent1 />
  <subComponent2 />
</MyStore.Provider>
```

-   `provider`는 정의한 context를 하위 컴포넌트에게 전달하는 역할을 합니다.
-   provider를 전달하는 변수는 꼭 `value`를 사용해야 합니다
-   전달 받는 컴포넌트의 제한 수는 없습니다
-   provider에 하위 provider배치 가능하며, 그럴경우 하위 provider값이 우선시 됩니다
-   provider하위에 context를 가진 component는 provider의 value로 가진 state가 변화할 때마다, 전부 re render됩니다.

### [#](https://kyounghwan01.github.io/blog/React/react-context-api/#context-consumer)Context.Consumer 

```
<MyContext.Consumer>
  {value => /* context 값을 이용한 렌더링 */}
</MyContext.Consumer>
```

-   context 변화를 구독하는 컴포넌트입니다
-   class 컴포넌트의 구독법은 아래 예시에 있습니다.
-   context의 자식은 함수(컴포넌트)이어야 합니다.
-   이 함수(컴포넌트)가 가지는 context 값은 가장 가까운 provider의 값입니다
-   상위 provider가 없다면 `createContext()`에서 정의한 `defaultValue`를 가집니다

### useContext 구현하기

```js
const MyReact = (() => {
  function createContext(initialValue) {
    const emitter = createEventEmitter(initialValue)

    const Provider = ({ value, children }) => <>{children}</>

    const Consumer = ({ children }) => <>{children(emitter.get())}</>

    return {
      Provider,
      Consumer,
    }
  }
})()
```

컨택스트의 초기 값을 인자로 받아 컨택스트 객체를 만드는 함수다. 이 값으로 이벤트 에미터를 만들었다. Provider, Consumer 컴포넌트를 만들었는데 이벤트 에미터를 통해 메세지를 공급/소비하는 역할을 할 것이다.

컨택스트를 이렇게 사용해 보자.

```jsx
const countContext = MyReact.createContext({
  count: 0,
  setCount() {},
})

const CountProvider = ({ children }) => {
  const [count, setCount] = useState(0)

  return (
    <countContext.Provider value={{ count, setCount }}>
      {children}
    </countContext.Provider>
  )
}
```

count와 세터 setCount를 가진 값을 이용해 countContext를 만들었다. CountProvider 컴포넌트는 이 컨택스트를 이용해 값을 공급하는 역할을 한다.

```jsx
const Count = () => {
  return (
    <countContext.Consumer>
      {({ count }) => <div>{count}</div>}
    </countContext.Consumer>
  )
}

const PlusButton = () => {
  return (
    <countContext.Consumer>
      {({ count, setCount }) => (
        <button onClick={() => setCount(count + 1)}>+ 카운트 올리기</button>
      )}
    </countContext.Consumer>
  )
}
```

컨택스트가 제공하는 값 중 count를 이용해 값을 표시하는 Count 컴포넌트를 만들었다. 컨택스트의 Consumer 컴포넌트가 자식 children을 렌더 프롭으로 사용하기 때문에 함수를 자식 컴포넌트로 정의했다.

버튼 역할을 하는 PlusButton 컴포넌트도 만들었다. 컨택스트가 제공하는 count와 setCount를 사용하는데 카운트 값을 하나씩 증가시킬 것이다.

컨택스트를 이용해 만든 컴포넌트를 조합해 만든 리액트 트리는 이런 모습일 것이다.

```jsx
const App = () => {
  return (
    <CountProvider>
      <Count />
      <PlusButton />
    </CountProvider>
  )
}
```

컴포넌트간에는 아무런 메세지도 전달하지 않는다. 컨택스트를 만들고 CountProvider가 공용 값을 제공할 것이다. Count와 PlusButton 컴포넌트는 제공된 이 값을 직접 소비한다.

그러나 아직 동작하지는 않는다. 컨택스트의 값은 변경되지만 변경된 값으로 리액트가 컴포넌트를 리렌더 해야하는 과제가 남았다. 컨택스트 값의 변화를 이벤트 에미터가 전달할하여 각 Consumer 컴포넌트가 이를 인지하고 리렌더링 하도록 해야한다.

# Prvider와 Consumer

컴포넌트가 리렌더링되려면 컴포넌트 상태나 프롭스가 변경 되어야 한다.

Provider와 Consumer를 다시 살펴보자.

```jsx
const Provider = ({ value, children }) => <>{children}</>

const Consumer = ({ children }) => <>{children(emitter.get())}</>
```

이벤트 에미터에서 값을 가져오기만했지 값의 변화를 전파하지는 않았다. 이 값이 리액트 상태로 관리되고 값의 이벤트 에미터가 관리하는 값의 변화를 이 상태와 동기화 해야할 것이다.

값을 제공하는 Provider 컴포넌트를 이렇게 바꾸어 보자.

```jsx
const Provider = ({ value, children }) => {
  // value 값이 변하면 이벤트 에미터에게 이를 알린다.
  // 이벤트 에미터는 구독하고 있는 객체들에게 이를 전파할 것이다.
  React.useEffect(() => {
    emitter.set(value)
  }, [value])

  return <>{children}</>
}
```

Provider 컴포넌트는 프롭스로 value가 변경될 때마다 이를 이벤트 에미터에게 알릴 것이다. 메세지를 받은 이벤트 에미터는 캡쳐해둔 값을 새로운 메세지로 변경할 것이다. 그리고 핸들러 함수를 모두 실행하면서 구독하고 있는 객체들에게 변경된 값을 통지할 것이다.

Consumer 컴포넌트를 이렇게 바꿔보자.

```jsx
const Consumer = ({ children }) => {
  // 리렌더링을 위해 이벤트 에미터의 값을 상태 value로 가지고 있다.
  const [value, setValue] = useState(emitter.get())

  // 이벤트 에미터를 구독한다.``
  // 이벤트 에미터가 변경을 알리면 이 값을 상태로 세팅한다.
  // 상태가 변경되면 리액트는 이 컴포넌트를 다시 그릴 것이다.
  React.useEffect(() => {
    emitter.on(setValue)
    return () => emitter.off(setValue)
  }, [])

  return <>{children(value)}</>
}
```

리렌더링을 위해 이벤트 에미터의 값을 컴포넌트의 자체 상태 value로 정의했다. 이벤트 에미터를 구독하고 있다가 새로운 메세지가 통보를 받으면 이 상태 갱신한다. 마침내 리액트는 컴포넌트의 상태 변화를 감지한 뒤 이 컴포넌트를 다시 그릴 것이다.

[![컨택스트가 동작한다](https://jeonghwan-kim.github.io/static/1793887073b6a1c87c8bd9235149797d/f8067/CountApp1.png "컨택스트가 동작한다")](https://jeonghwan-kim.github.io/static/1793887073b6a1c87c8bd9235149797d/c65fa/CountApp1.png)

컨택스트가 동작한다

# 좀 더 단순한 방법을 고민

이렇게 해서 컨택스트는 프롭 드릴링 문제를 해결할 수 있지만 여전히 아쉬운 점이 있다.
렌더 프롭은 컴포넌트 재사용의 한 방법이긴 하지만 간결한 사용법이 있으면 더 좋겠다. 재사용의 이점은 있지만 코드 읽기가 비교적 불편하기 때문이다.

게다가 UI와 무관한 Consumer 컴포넌트를 리액트 앨리먼트 사이에 넣어야 한다. 여러 컨택스트를 사용해야되는 상황을 떠올려 보면 확실히 문제다. 컴포넌트 본연의 엘리먼트를 만들기 위해 각 컨택스트의 소비자 컴포넌트로 몇 겹씩 감싸는 모습은 쉽게 UI를 가늠하기 어렵다.

```jsx
<ConsumerA>{a => (
  <ConsumerB>{b => (
    <ConsumerC>{c => (
      // ...
    )}</ConsumerC>
  )}<ConsumerB>
)}</ConsumerA>
```

# 훅을 만들어 보자

Consumer 컴포넌트는 이벤트 에미터의 메세지를 리액트 상태로 가지면서 리액트의 리렌더링을 유발했다.

상태를 커스텀 훅으로도 분리한 뒤 이 훅을 컴포넌트에서 사용할 수 있다. 컨택스트를 소비할 수 있는 useContext 훅을 만들어 보자.

```jsx
const MyReact = (() => {
  // ...

  // 컨택스트 값을 사용할 수 있는 훅이다.
  function useContext(context) {
    // 컨택스트 값을 상태 value로 저장해 둔다
    const [value, setValue] = useState(context.emitter.get())

    React.useEffect(() => {
      // 컨택스트의 이벤트 에미터로부터 값을 수신하면 상태 value를 갱신다.
      // 이 상태를 사용하는 컴포넌트는 리렌더링 될 것이다.
      context.emitter.on(setValue)
      return () => context.emitter.off(setValue)
    }, [context])

    // 컨택스트 값을 반환한다.
    return value
  }

  return {
    createContext,
    // 훅을 제공한다.
    useContext,
  }
})()
```

컨택스트를 인자로 받아 이벤트 에미터의 값을 초기값으로 가지는 상태 value를 만들었다. 이벤트 에미터의 변경을 setValue 함수가 구독해서 컨택스트 값이 변하면 상태 value를 갱신하도록 했다. Consumer 컴포넌트의 로직과 똑같다.

useContext 훅을 사용하는 컴포넌트는 컨택스트의 값이 변경될 때마다 리액트가 리렌더링 하게 될 것이다.

컨택스트 팩토리 함수의 반환값에 이벤트 에미터도 추가했다.

```jsx
function createContext(initialValue) {
  // ...

  return {
    Provider,
    Consumer,
    // emitter를 제공한다.
    emitter,
  }
}
```

마침내 컨텍스트를 소비하는 코드는 이렇게 바꿀 수 있다.

```jsx
const Count = () => {
  // 컨택스트의 값을 가져온다
  const { count } = MyReact.useContext(countContext)
  // Consumer의 렌더 프롭보다 간결하다.
  return <div>{count}</div>
}
```

Consumer 컴포넌트의 렌더 프롭을 통해 컨택스트 값을 조회했던 코드가 꽤나 간결해 졌다.

PlusButton도 마찬가지다.

```jsx
const PlusButton: FC = () => {
  // 컨택스트의 값을 가져온다
  const { count, setCount } = MyReact.useContext(countContext)
  // Consumer의 렌더 프롭보다 간결하다.
  return <button onClick={e => setCount(count + 1)}>+ 카운트 올리기</button>
}
```

[![트리에 Consumer 컴포넌트가 없다](https://jeonghwan-kim.github.io/static/cf479f4523bbcb0bd3f62eb5dd2f8bff/f8067/CountApp2.png "트리에 Consumer 컴포넌트가 없다")](https://jeonghwan-kim.github.io/static/cf479f4523bbcb0bd3f62eb5dd2f8bff/52ab5/CountApp2.png)

트리에 Consumer 컴포넌트가 없다

### 결론

이 주제를 고민할 때는 useContext 훅의 구조가 궁금했었다. 리액트가 제공하는 컨택스트 객체를 잘 살펴보면 컨택스트 제공하는 값이 있을 것이라고 생각해서 이를 활용하려고 했다. 값을 제공하기는 하지만 훅으로 분리하기에는 좀 마땅치 않아서 이렇게 컨택스트도 구현하게 되었다.

useContext가 해결하려는 문제는 무엇이었을까?

먼저는 UI 코드에서 컨택스트 코드를 분리할 수 있었다. 컨택스트 값을 사용하기 위해 Consumer 컴포넌트를 리액트 앨림먼트 사이에 끼워넣는 기존 구조는 가독성이 좋지 않다. 사용할 컨택스트 갯수만큼 리액트 트리가 깊어지기 때문이다.

훅을 사용하다 보니 Consumer 컴포넌트의 렌더 프롭이 다소 불편한 API 였구나라는 생각도 들었다.