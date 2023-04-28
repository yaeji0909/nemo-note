
### React.memo (HOC)
memo를 사용하면 props가 변경되지 않았을 때 컴포넌트를 다시 렌더링하지 않을 수 있습니다.

### Usage
- component에만 적용 가능
- **한 컴포넌트를 같은 프로퍼티로 리랜더링하는 경우가 빈번할 때 사용**해야 한다.
- React.memo를 사용하면 어떤 컴포넌트를 어떤 Props와 함께 불렀는지 기록하고, 같은 경우에는 이전에 기록해둔 컴포넌트 결과값을 반환한다. Memoization 방식이다.


### Parameters

### 매개변수

```jsx
`Component`: The component that you want to memoize. The `memo` does not modify this component, but returns a new, memoized component instead. Any valid React component, including functions and `[forwardRef](<https://beta.reactjs.org/reference/react/forwardRef>)` components, is accepted.

> `Component`: 메모이즈하려는 컴포넌트입니다. `memo`는 이 컴포넌트를 수정하지 않고 새롭게 메모이즈된 컴포넌트를 반환합니다. 함수와 `[forwardRef](<https://beta.reactjs.org/reference/react/forwardRef>)` 컴포넌트를 포함한 모든 유효한 React 컴포넌트가 허용됩니다.
> 
```

```
**optional** `arePropsEqual`: A function that accepts two arguments: the component’s previous props, and its new props. It should return `true` if the old and new props are equal: that is, if the component will render the same output and behave in the same way with the new props as with the old. Otherwise it should return `false`. Usually, you will not specify this function. By default, React will compare each prop with `[Object.is`.](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is>)

> **선택적** `arePropsEqual`: 두 인수, 컴포넌트의 이전 prop과 새 prop을 받는 함수입니다. 새로운 prop이 이전 prop과 같으면 `true`를 반환해야 합니다. 다시 말해, 새로운 prop으로도 컴포넌트가 동일한 출력을 만들고 동일한 방식으로 작동하는 경우입니다. 그렇지 않으면 `false`를 반환해야 합니다. 일반적으로 이 함수를 지정하지 않습니다. React는 기본적으로 `[Object.is](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is>)`로 각 prop을 비교합니다.
> 
```

### Returns

### 반환값

`memo` returns a new React component. It behaves the same as the component provided to `memo` except that React will not always re-render it when its parent is being re-rendered unless its props have changed.

> `memo`는 새로운 React 컴포넌트를 반환합니다. `memo`로 제공된 컴포넌트와 동일하게 작동하지만 부모가 다시 렌더링될 때 prop이 변경되지 않은 경우에만 React가 다시 렌더링하지 않습니다.

## Usage

## 사용법

### Skipping re-rendering when props are unchanged

### prop이 변경되지 않았을 때 렌더링 건너뛰기

React normally re-renders a component whenever its parent re-renders. With `memo`, you can create a component that React will not re-render when its parent re-renders so long as its new props are the same as the old props. Such a component is said to be _memoized_.

> React는 일반적으로 부모가 다시 렌더링될 때마다 컴포넌트를 다시 렌더링합니다. `memo`를 사용하면 새 props가 이전 props와 동일한 경우 부모가 다시 실행될 때 React가 다시 실행되지 않는 컴포넌트를 만들 수 있습니다.이러한 컴포넌트를 _메모이즈된_ 컴포넌트라고합니다.

To memoize a component, wrap it in `memo` and use the value that it returns in place of your original component:

> 컴포넌트를 메모이즈하려면 `memo`로 감싸고 반환된 값을 원래 컴포넌트의 자리에 사용하십시오.

```
const Greeting = memo(function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
});

export default Greeting;
```

A React component should always have [pure rendering logic.](https://beta.reactjs.org/learn/keeping-components-pure) This means that it must return the same output if its props, state, and context haven’t changed. By using `memo`, you are telling React that your component complies with this requirement, so React doesn’t need to re-render as long as its props haven’t changed. Even with `memo`, your component will re-render if its own state changes or if a context that it’s using changes.

> React 컴포넌트는 항상 [순수한 렌더링 로직](https://beta.reactjs.org/learn/keeping-components-pure)을 가져야합니다. 이는 props, state 및 컨텍스트가 변경되지 않으면 항상 동일한 출력을 반환해야 함을 의미합니다. `memo`를 사용하면 컴포넌트가 이 요구 사항을 준수한다고 React에 알리므로 prop이 변경되지 않는 한 React는 다시 렌더링할 필요가 없습니다. `memo`로도, 컴포넌트의 상태가 변경되거나 사용중인 컨텍스트가 변경되면 다시 렌더링됩니다.

In this example, notice that the `Greeting` component re-renders whenever `name` is changed (because that’s one of its props), but not when `address` is changed (because it’s not passed to `Greeting` as a prop):

> 이 예에서 `Greeting` 컴포넌트는 `name`이 변경될 때마다 (이것은 props 중 하나이기 때문에) 다시 렌더링되지만, `address`가 변경될 때는 (prop으로 전달되지 않기 때문에) 다시 렌더링되지 않습니다.

```
import { memo, useState } from 'react';

export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        Name{': '}
        <input value={name} onChange={e => setName(e.target.value)} />
      </label>
      <label>
        Address{': '}
        <input value={address} onChange={e => setAddress(e.target.value)} />
      </label>
      <Greeting name={name} />
    </>
  );
}

const Greeting = memo(function Greeting({ name }) {
  console.log("Greeting was rendered at", new Date().toLocaleTimeString());
  return <h3>Hello{name && ', '}{name}!</h3>;
});

```

<aside> 📄 **Note**

**You should only rely on `memo` as a performance optimization.** If your code doesn’t work without it, find the underlying problem and fix it first. Then you may add `memo` to improve performance.

> `**memo`는 성능 최적화로만 사용해야 합니다.** 코드가 `memo`없이 작동하지 않으면 기본 문제를 찾아 먼저 고치고, 성능을 개선하기 위해 `memo`를 추가할 수 있습니다.

</aside>

### DEEP DIVE

### ****Should you add memo everywhere?****

### **모든 곳에 memo를 추가해야할까요?**

If your app is like this site, and most interactions are coarse (like replacing a page or an entire section), memoization is usually unnecessary. On the other hand, if your app is more like a drawing editor, and most interactions are granular (like moving shapes), then you might find memoization very helpful.

> 이 사이트와 같은 앱이면 대부분의 상호 작용이 조잡하고 (페이지 또는 전체 섹션 교체 같은) 메모이제이션이 일반적으로 불필요합니다. 반면 그림 편집기와 같은 앱이면 대부분의 상호 작용이 세부적이라면 (도형 이동과 같은) 메모이제이션이 매우 유용할 수 있습니다.

Optimizing with `memo` is only valuable when your component re-renders often with the same exact props, and its re-rendering logic is expensive. If there is no perceptible lag when your component re-renders, `memo` is unnecessary. Keep in mind that `memo` is completely useless if the props passed to your component are _always different,_ such as if you pass an object or a plain function defined during rendering. This is why you will often need `[useMemo](<https://react.dev/reference/react/useMemo#skipping-re-rendering-of-components>)` and `[useCallback](<https://react.dev/reference/react/useCallback#skipping-re-rendering-of-components>)` together with `memo`.

> `memo`로 최적화하는 것은 컴포넌트가 정확히 동일한 prop으로 자주 다시 렌더링되고 리렌더링 로직이 비용이 많이 들 때만 가치가 있습니다. 컴포넌트가 다시 렌더링되어도 인지할 수있는 지연이 없으면 `memo`가 필요하지 않습니다. `memo`는 컴포넌트에 전달되는 prop이 _항상 다른 경우_ (객체 또는 렌더링 중 정의된 일반 함수와 같은 경우) 완전히 무용지물입니다. 따라서 `memo`와 함께 `[useMemo](<https://react.dev/reference/react/useMemo#skipping-re-rendering-of-components>)` 및 `[useCallback](<https://react.dev/reference/react/useCallback#skipping-re-rendering-of-components>)` 을 종종 필요로합니다.

There is no benefit to wrapping a component in `memo` in other cases. There is no significant harm to doing that either, so some teams choose to not think about individual cases, and memoize as much as possible. The downside of this approach is that code becomes less readable. Also, not all memoization is effective: a single value that’s “always new” is enough to break memoization for an entire component.

> 그 외의 경우 컴포넌트를 `memo`로 감싸는 것에 이점이 없습니다. 그렇게 해도 큰 해를 끼치지 않기 때문에 일부 팀은 개별 사례에 대해 생각하지 않고 가능한 한 많이 메모하기로 선택합니다. 이 접근 방식의 단점은 코드를 읽기 어렵게 된다는 것입니다. 또한 모든 메모이제이션이 효과적이지는 않습니다: "항상 새로운" 단일 값은 컴포넌트 전체의 메모이제이션을 깨뜨릴 수 있습니다.

**In practice, you can make a lot of memoization unnecessary by following a few principles:**

> **실제로는 몇 가지 원칙을 따르면 많은 메모이제이션을 불필요하게 할 수 있습니다:**

1.  When a component visually wraps other components, let it [accept JSX as children.](https://react.dev/learn/passing-props-to-a-component#passing-jsx-as-children) This way, when the wrapper component updates its own state, React knows that its children don’t need to re-render.
2.  Prefer local state and don’t [lift state up](https://react.dev/learn/sharing-state-between-components) any further than necessary. For example, don’t keep transient state like forms and whether an item is hovered at the top of your tree or in a global state library.
3.  Keep your [rendering logic pure.](https://react.dev/learn/keeping-components-pure) If re-rendering a component causes a problem or produces some noticeable visual artifact, it’s a bug in your component! Fix the bug instead of adding memoization.
4.  Avoid [unnecessary Effects that update state.](https://react.dev/learn/you-might-not-need-an-effect) Most performance problems in React apps are caused by chains of updates originating from Effects that cause your components to render over and over.
5.  Try to [remove unnecessary dependencies from your Effects.](https://react.dev/learn/removing-effect-dependencies) For example, instead of memoization, it’s often simpler to move some object or a function inside an Effect or outside the component.

> 1.  컴포넌트가 다른 컴포넌트를 시각적으로 감싸는 경우, [JSX를 자식으로 허용하십시오.](https://react.dev/learn/passing-props-to-a-component#passing-jsx-as-children) 이렇게하면 래퍼 컴포넌트가 자체 상태를 업데이트 할 때 React는 자식 컴포넌트가 다시 렌더링할 필요가 없다는 것을 알 수 있습니다.
> 2.  가능한 한 local state를 선호하고 더 이상 필요하지 않은 곳까지 [state를 올리지](https://react.dev/learn/sharing-state-between-components) 마십시오. 예를 들어, 최상위 트리나 전역 상태 라이브러리에 양식과 아이템이 호버되었는지와 같은 일시적 상태를 유지하지 마십시오.
> 3.  [렌더링 로직을 순수하게](https://react.dev/learn/keeping-components-pure) 유지하십시오. 컴포넌트를 다시 렌더링하면 문제가 발생하거나 눈에 띄는 시각적 아티팩트가 발생하는 경우 컴포넌트의 버그입니다! 메모이제이션을 추가하는 대신 버그를 수정하십시오.
> 4.  [업데이트 상태를 업데이트하는 불필요한 효과](https://react.dev/learn/you-might-not-need-an-effect)를 피하십시오. React 앱 대부분의 성능 문제는 컴포넌트를 반복적으로 렌더링하는 Effect에서 발생하는 업데이트 체인으로 인해 발생합니다.
> 5.  [Effects에서 불필요한 종속성을 제거](https://react.dev/learn/removing-effect-dependencies)하십시오. 예를 들어, 메모이제이션 대신 일부 개체나 함수를 Effect 내부 또는 컴포넌트 외부로 이동하는 것이 더 간단한 경우가 많습니다.

If a specific interaction still feels laggy, [use the React Developer Tools profiler](https://react.dev/blog/2018/09/10/introducing-the-react-profiler.html) to see which components would benefit the most from memoization, and add memoization where needed. These principles make your components easier to debug and understand, so it’s good to follow them in any case. In the long term, we’re researching [doing granular memoization automatically](https://www.youtube.com/watch?v=lGEMwh32soc) to solve this once and for all.

> 특정 상호 작용이 여전히 지연되는 경우 [React Developer Tools 프로파일러를 사용](https://react.dev/blog/2018/09/10/introducing-the-react-profiler.html)하여 어떤 컴포넌트가 메모이제이션의 혜택을 가장 많이 받을 수 있는지 확인하고 필요한 경우 메모이제이션을 추가합니다. 이러한 원칙은 컴포넌트를 디버깅하고 이해하기 쉽게 하므로 어떤 경우에도 이를 따르는 것이 좋습니다. 장기적으로 우리는 이 문제를 완전히 해결하기 위해 [자동으로 세분화된 메모이제이션을 수행하는 것](https://www.youtube.com/watch?v=lGEMwh32soc)을 연구하고 있습니다.

### Updating a memoized component using state

### 상태를 사용하여 메모이즈된 컴포넌트 업데이트

Even when a component is memoized, it will still re-render when its own state changes. Memoization only has to do with props that are passed to the component from its parent.

> 컴포넌트가 메모이즈된 경우에도 자체 state가 변경되면 다시 렌더링됩니다. 메모이제이션은 부모에서 컴포넌트로 전달되는 props와만 관련이 있습니다.

```
import { memo, useState } from 'react';

export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        Name{': '}
        <input value={name} onChange={e => setName(e.target.value)} />
      </label>
      <label>
        Address{': '}
        <input value={address} onChange={e => setAddress(e.target.value)} />
      </label>
      <Greeting name={name} />
    </>
  );
}

const Greeting = memo(function Greeting({ name }) {
  console.log('Greeting was rendered at', new Date().toLocaleTimeString());
  const [greeting, setGreeting] = useState('Hello');
  return (
    <>
      <h3>{greeting}{name && ', '}{name}!</h3>
      <GreetingSelector value={greeting} onChange={setGreeting} />
    </>
  );
});

function GreetingSelector({ value, onChange }) {
  return (
    <>
      <label>
        <input
          type="radio"
          checked={value === 'Hello'}
          onChange={e => onChange('Hello')}
        />
        Regular greeting
      </label>
      <label>
        <input
          type="radio"
          checked={value === 'Hello and welcome'}
          onChange={e => onChange('Hello and welcome')}
        />
        Enthusiastic greeting
      </label>
    </>
  );
}

```

If you set a state variable to its current value, React will skip re-rendering your component even without `memo`. You may still see your component function being called an extra time, but the result will be discarded.

> state 변수를 현재 값으로 설정하면 React는 `memo` 없이도 컴포넌트 리렌더링을 건너뜁니다. 컴포넌트 기능이 추가 시간으로 호출되는 것을 볼 수 있지만 결과는 무시됩니다.

### Updating a memoized component using a context

### 컨텍스트를 사용하여 메모이즈된 컴포넌트 업데이트

Even when a component is memoized, it will still re-render when a context that it’s using changes. Memoization only has to do with props that are passed to the component from its parent.

> 컴포넌트가 메모이즈되었더라도 사용 중인 컨텍스트가 변경될 때 이 컴포넌트는 다시 렌더링됩니다. 메모이제이션은 부모로부터 컴포넌트로 전달되는 props와만 관련이 있습니다.

```
import { createContext, memo, useContext, useState } from 'react';

const ThemeContext = createContext(null);

export default function MyApp() {
  const [theme, setTheme] = useState('dark');

  function handleClick() {
    setTheme(theme === 'dark' ? 'light' : 'dark');
  }

  return (
    <ThemeContext.Provider value={theme}>
      <button onClick={handleClick}>
        Switch theme
      </button>
      <Greeting name="Taylor" />
    </ThemeContext.Provider>
  );
}

const Greeting = memo(function Greeting({ name }) {
  console.log("Greeting was rendered at", new Date().toLocaleTimeString());
  const theme = useContext(ThemeContext);
  return (
    <h3 className={theme}>Hello, {name}!</h3>
  );
});

```

To make your component re-render only when a _part_ of some context changes, split your component in two. Read what you need from the context in the outer component, and pass it down to a memoized child as a prop.

> 일부 컨텍스트의 일부가 변경될 때만 컴포넌트를 다시 렌더링하려면, 컴포넌트를 두 개로 분할합니다. 외부 컴포넌트의 컨텍스트에서 필요한 내용을 읽고 메모이즈된 자식에게 prop으로 전달합니다.

### Minimizing props changes

### props 변경 최소화

When you use `memo`, your component re-renders whenever any prop is not _shallowly equal_ to what it was previously. This means that React compares every prop in your component with its previous value using the `[Object.is](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is>)` comparison. Note that `Object.is(3, 3)` is `true`, but `Object.is({}, {})` is `false`.

`memo`를 사용할 때 어떤 prop든 이전의 prop과 _얕게_ 같지 않을 때마다 컴포넌트가 다시 작동합니다. 즉, React는 [Object.is](http://object.is/) 비교를 사용하여 컴포넌트의 모든 prop을 이전 값과 비교합니다. `Object.is (3, 3)`은 `true`이지만 [`Object.is](<http://object.is/>) ({}, {})`는 `false`입니다.

To get the most out of `memo`, minimize the times that the props change. For example, if the prop is an object, prevent the parent component from re-creating that object every time by using `[useMemo`:]([https://beta.reactjs.org/reference/react/useMemo](https://beta.reactjs.org/reference/react/useMemo))

> `memo`를 최대한 활용하려면, props가 변경되는 시간을 최소화합니다. 예를 들어, prop이 객체인 경우 `[useMemo](<https://beta.reactjs.org/reference/react/useMemo>)`를 사용하여 부모 컴포넌트가 매번 해당 객체를 다시 만들지 못하도록 합니다:

```
function Page() {
  const [name, setName] = useState('Taylor');
  const [age, setAge] = useState(42);

  const person = useMemo(
    () => ({ name, age }),
    [name, age]
  );

  return <Profile person={person} />;
}

const Profile = memo(function Profile({ person }) {
  // ...
});
```

A better way to minimize props changes is to make sure the component accepts the minimum necessary information in its props. For example, it could accept individual values instead of a whole object:

> props 변경을 최소화하는 더 나은 방법은 props가 해당 요소에 필요한 최소한의 정보를 수락하는지 확인하는 것입니다. 예를 들어 전체 객체 대신 개별 값을 사용할 수 있습니다:

```
function Page() {
  const [name, setName] = useState('Taylor');
  const [age, setAge] = useState(42);
  return <Profile name={name} age={age} />;
}

const Profile = memo(function Profile({ name, age }) {
  // ...
});
```

Even individual values can sometimes be projected to ones that change less frequently. For example, here a component accepts a boolean indicating the presence of a value rather than the value itself:

> 심지어 개별 값도 때때로 덜 자주 바뀌는 값에 투영될 수 있습니다. 예를 들어, 여기서 컴포넌트는 값 자체가 아닌 값의 존재를 나타내는 부울을 허용합니다:

```
function GroupsLanding({ person }) {
  const hasGroups = person.groups !== null;
  return <CallToAction hasGroups={hasGroups} />;
}

const CallToAction = memo(function CallToAction({ hasGroups }) {
  // ...
});
```

When you need to pass a function to memoized component, either declare it outside your component so that it never changes, or `[useCallback](<https://beta.reactjs.org/reference/react/useCallback#skipping-re-rendering-of-components>)` to cache its definition between re-renders.

> memoized 컴포넌트에 함수를 전달해야 하는 경우, 변경되지 않도록 컴포넌트 외부에서 선언하거나, 리렌더 사이에 정의를 캐시하기 위해 `[useCallback](<https://beta.reactjs.org/reference/react/useCallback#skipping-re-rendering-of-components>)`을 사용하십시오.

### Specifying a custom comparison function

### 사용자 정의 비교 함수 지정

In rare cases it may be infeasible to minimize the props changes of a memoized component. In that case, you can provide a custom comparison function, which React will use to compare the old and new props instead of using shallow equality. This function is passed as a second argument to `memo`. It should return `true` only if the new props would result in the same output as the old props; otherwise it should return `false`.

> 드물게 메모이즈된 컴포넌트의 props 변경을 최소화하는 것이 불가능할 수 있습니다. 이 경우 React가 얕은 동등성을 사용하는 대신 이전 props와 새 props를 비교하는 데 사용할 사용자 지정 비교 함수를 제공할 수 있습니다. 이 함수는 `memo`의 두 번째 인수로 전달됩니다. 새 props가 이전 props와 동일한 출력을 생성하는 경우에만 `true`를 반환해야 합니다. 그렇지 않으면 `false`를 반환해야 합니다.

```
const Chart = memo(function Chart({ dataPoints }) {
  // ...
}, arePropsEqual);

function arePropsEqual(oldProps, newProps) {
  return (
    oldProps.dataPoints.length === newProps.dataPoints.length &&
    oldProps.dataPoints.every((oldPoint, index) => {
      const newPoint = newProps.dataPoints[index];
      return oldPoint.x === newPoint.x && oldPoint.y === newPoint.y;
    })
  );
}
```

If you do this, use the Performance panel in your browser developer tools to make sure that your comparison function is actually faster than re-rendering the component. You might be surprised.

> 이 경우 브라우저 개발자 도구의 성능 패널을 사용하여 비교 기능이 컴포넌트를 다시 렌더링하는 것보다 실제로 더 빠른지 확인하십시오. 당신은 놀랄 수 있습니다.

When you do performance measurements, make sure that React is running in the production mode.

> 성능 측정을 할 때 React가 프로덕션 모드에서 실행되고 있는지 확인하십시오.

<aside> ⚠️ **Pitfall**

If you provide a custom `arePropsEqual` implementation, **you must compare every prop, including functions.** Functions often [close over](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) the props and state of parent components. If you return `true` when `oldProps.onClick !== newProps.onClick`, your component will keep “seeing” the props and state from a previous render inside its `onClick` handler, leading to very confusing bugs.

> 사용자 지정 `arePropsEqual` 구현을 제공하는 경우 **함수를 포함하여 모든 props를 비교해야 합니다**. 함수는 종종 부모 컴포넌트의 prop과 state를 [닫습니다](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures). `oldProps.onClick !== newProps.onClick`일 때 `true`를 반환하면 컴포넌트가 `onClick` 핸들러 내에서 이전 렌더링의 prop과 state를 계속 "인식"하여 매우 혼란스러운 버그가 발생합니다.

Avoid doing deep equality checks inside `arePropsEqual` unless you are 100% sure that the data structure you’re working with has a known limited depth. **Deep equality checks can become incredibly slow** and can freeze your app for many seconds if someone changes the data structure later.

> 작업 중인 데이터 구조가 알려진 제한된 깊이를 가지고 있다고 100% 확신하지 않는 한 `arePropsEqual` 내에서 깊은 동등성 검사를 수행하지 마십시오. **심층 평등 검사는 엄청나게 느려질 수 있으며** 나중에 누군가가 데이터 구조를 변경하면 몇 초 동안 앱이 정지될 수 있습니다.

</aside>

## Troubleshooting

## 트러블슈팅

### My component re-renders when a prop is an object, array, or function

### prop이 객체, 배열 또는 함수일 때 컴포넌트가 다시 렌더링됨

React compares old and new props by shallow equality: that is, it considers whether each new prop is reference-equal to the old prop. If you create a new object or array each time the parent is re-rendered, even if the individual elements are each the same, React will still consider it to be changed. Similarly, if you create a new function when rendering the parent component, React will consider it to have changed even if the function has the same definition. To avoid this, [simplify props or memoize props in the parent component](https://beta.reactjs.org/reference/react/memo#minimizing-props-changes).

> React는 이전 prop과 새 prop을 얕은 동등성으로 비교합니다. 즉, 각각의 새 prop이 이전 prop과 참조가 동일한지 여부를 고려합니다. 부모가 다시 렌더링될 때마다 새 객체나 배열을 생성하면 개별 요소가 각각 동일하더라도 React는 여전히 변경된 것으로 간주합니다. 마찬가지로 부모 컴포넌트를 렌더링할 때 새 함수를 만들면 React는 함수의 정의가 동일하더라도 변경된 것으로 간주합니다. 이를 방지하려면 [부모 컴포넌트에서 prop을 단순화하거나 메모이즈하십시오.](https://beta.reactjs.org/reference/react/memo#minimizing-props-changes)