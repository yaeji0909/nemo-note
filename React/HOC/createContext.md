`createContext` lets you create a [context](https://react.dev/learn/passing-data-deeply-with-context) that components can provide or read.

> `createContext` 를 사용하면 컴포넌트가 제공하거나 읽을 수 있는 [컨텍스트](https://www.notion.so/3-6-Passing-Data-Deeply-with-Context-7756088925754e038cde85ff2435a633)를 만들 수 있습니다.

```jsx
const SomeContext = createContext(defaultValue)
```

-   Table of contents

---

## Reference

| 참조

### `createContext(defaultValue)`

Call `createContext` outside of any components to create a context.

> 컴포넌트 외부에서 `createContext`를 호출하여 컨텍스트를 생성합니다.

```jsx
import { createContext } from 'react';

const ThemeContext = createContext('light');
```

[See more examples below.](https://beta.reactjs.org/reference/react/createContext#usage)

> 아래에서 더 많은 예제를 확인하세요.

### Parameters

-   `defaultValue`: The value that you want the context to have when there is no matching context provider in the tree above the component that reads context. If you don’t have any meaningful default value, specify `null`. The default value is meant as a “last resort” fallback. It is static and never changes over time.

> `defaultValue`: 컨텍스트를 읽는 컴포넌트 상위 트리에 일치하는 컨텍스트 공급자가 없을 때 이 컨텍스트가 갖도록 할 값입니다. 기본값을 지정하지 않을 경우 `null`을 지정합니다. 기본값은 "가장 마지막 수단"으로 사용됩니다. 기본값은 정적값이며 시간이 지나도 절대 변경되지 않습니다.

### Returns

`createContext` returns a context object.

**The context object itself does not hold any information.** It represents _which_ context other components can read or provide. Typically, you will use `[SomeContext.Provider](<https://beta.reactjs.org/reference/react/createContext#provider>)` in components above to specify the context value, and call `[useContext(SomeContext)](<https://beta.reactjs.org/reference/react/useContext>)` in components below to read it. The context object has a few properties:

-   `SomeContext.Provider` lets you provide the context value to components.
-   `SomeContext.Consumer` is an alternative and rarely used way to read the context value.

> `createContext` 는 컨텍스트 객체를 반환합니다. **컨텍스트 객체 자체는 어떠한 정보도 보유하지 않습니다**. 다른 컴포넌트가 읽거나 제공할 수 있는 컨텍스트를 나타냅니다. 일반적으로, 위 컴포넌트에서 `SomeContext.Provider`를 사용해 컨텍스트 값을 지정하고, 아래 컴포넌트에서 `[useContext(SomeContext)](<https://www.notion.so/5169208ec79244068b91ed5f9fc70017>)`를 호출해 컨텍스트 값을 읽습니다. 컨텍스트 객체에는 몇 가지 속성이 있습니다:

-   `SomeContext.Provider`를 사용하면 컴포넌트에 컨텍스트 값을 제공할 수 있습니다.
-   `SomeContext.Consumer`는 컨텍스트 값을 읽는 또다른 방법이며 거의 사용되지 않습니다.

---

### `SomeContext.Provider`

Wrap your components into a context provider to specify the value of this context for all components inside:

> 컴포넌트를 컨텍스트 공급자로 래핑하여 내부의 모든 컴포넌트에 대해 이 컨텍스트 값을 지정하세요:

```jsx
function App() {
  const [theme, setTheme] = useState('light');
  // ...
  return (
    <ThemeContext.Provider value={theme}>
      <Page />
    </ThemeContext.Provider>
  );
}
```

### Props

-   `value`: The value that you want to pass to all the components reading this context inside this provider, no matter how deep. The context value can be of any type. A component calling `[useContext(SomeContext)](<https://beta.reactjs.org/reference/react/useContext>)` inside of the provider receives the `value` of the innermost corresponding context provider above it.

> `value` : 이 값은 해당 공급자 내에서 이 컨텍스트를 읽는 모든 컴포넌트에 전달하려는 값으로, 깊이에 상관없이 전달할 수 있습니다. 컨텍스트 값은 모든 타입이 될 수 있습니다. 공급자 내부에서 `[useContext(SomeContext)](<https://www.notion.so/5169208ec79244068b91ed5f9fc70017>)`를 호출하는 컴포넌트는 그 위에 있는 가장 안쪽에 해당하는 컨텍스트 공급자의 `value`를 받습니다.

---

### `SomeContext.Consumer`

Before `useContext` existed, there was an older way to read context:

> `useContext`가 존재하기 전에는 컨텍스트를 읽는 오래된 방법이 있었습니다:

```jsx
function Button() {
  // 🟡 Legacy way (not recommended)
  return (
    <ThemeContext.Consumer>
      {theme => (
        <button className={theme} />
      )}
    </ThemeContext.Consumer>
  );
}
```

Although this older way still works, but **newly written code should read context with `[useContext()](<https://beta.reactjs.org/reference/react/useContext>)` instead:**

> 이 오래된 방법은 여전히 동작하지만 **새로 작성된 코드는 `[useContext()](<https://www.notion.so/5169208ec79244068b91ed5f9fc70017>)`를 대신해서 사용하여 컨텍스트를 읽어야 합니다:**

```jsx
function Button() {
  // ✅ Recommended way
  const theme = useContext(ThemeContext);
  return <button className={theme} />;
}
```

### Props

-   `children`: A function. React will call the function you pass with the current context value determined by the same algorithm as `[useContext()](<https://beta.reactjs.org/reference/react/useContext>)` does, and render the result you return from this function. React will also re-run this function and update the UI whenever the context from the parent components changes.

> `자식요소` : 함수입니다. React는 `[useContext()](<https://www.notion.so/5169208ec79244068b91ed5f9fc70017>)`와 동일한 알고리즘에 의해 결정된 현재 컨텍스트 값으로 전달한 함수를 호출하고, 이 함수에서 반환한 결과를 렌더링합니다. 또한 React는 부모 컴포넌트의 컨텍스트가 변경될 때마다 이 함수를 다시 실행하고 UI를 업데이트합니다.

---

## Usage

| 사용법

### Creating context | context 생성하기

Context lets components [pass information deep down](https://beta.reactjs.org/learn/passing-data-deeply-with-context) without explicitly passing props.

Call `createContext` outside any components to create one or more contexts.

> 컨텍스트는 컴포넌트가 프로퍼티를 명시적으로 전달하지 않고도 [정보를 깊숙이 전달](https://www.notion.so/3-6-Passing-Data-Deeply-with-Context-7756088925754e038cde85ff2435a633)할 수 있게 해줍니다. 하나 이상의 컨텍스트를 생성하려면 컴포넌트 외부에서 `createContext`를 호출하세요.

```jsx
import { createContext } from 'react';

const ThemeContext = createContext('light');
const AuthContext = createContext(null);
```

`createContext` returns a context object. Components can read context by passing it to `[useContext()](<https://beta.reactjs.org/reference/react/useContext>)`:

> `createContext`는 컨텍스트 객체를 반환합니다. 컴포넌트는 컨텍스트를 `[useContext()](<https://www.notion.so/5169208ec79244068b91ed5f9fc70017>)`에 전달하여 컨텍스트를 읽을 수 있습니다:

```jsx
function Button() {
  const theme = useContext(ThemeContext);
  // ...
}

function Profile() {
  const currentUser = useContext(AuthContext);
  // ...
}
```

By default, the values they receive will be the default values you have specified when creating the contexts. However, by itself this isn’t useful because the default values never change.

Context is useful because you can **provide other, dynamic values from your components:**

> 기본적으로 수신되는 값은 컨텍스트를 만들 때 지정한 기본값이 됩니다. 그러나 기본값은 절대 변경되지 않기 때문에 그 자체로는 유용하지 않습니다.

컨텍스트가 유용한 이유는 **다음과 같이 컴포넌트에서 다른 동적 값을 제공할 수 있기 때문입니다.**

```jsx
function App() {
  const [theme, setTheme] = useState('dark');
  const [currentUser, setCurrentUser] = useState({ name: 'Taylor' });

  // ...

  return (
    <ThemeContext.Provider value={theme}>
      <AuthContext.Provider value={currentUser}>
        <Page />
      </AuthContext.Provider>
    </ThemeContext.Provider>
  );
}
```

Now the `Page` component and any components inside it, no matter how deep, will “see” the passed context values. If the passed context values change, React will re-render the components reading the context as well.

[Read more about reading and providing context and see examples.](https://beta.reactjs.org/reference/react/useContext)

> 이제 `Page` 컴포넌트와 그 안에 있는 모든 컴포넌트는 아무리 깊숙이 들어가더라도 전달된 컨텍스트 값을 "바라보게" 됩니다. 만약 전달된 컨텍스트 값이 변경되면, React는 컨텍스트를 읽는 컴포넌트도 다시 렌더링합니다.

[컨텍스트 읽기 및 제공에 대한 자세한 내용과 예시를 참조하세요.](https://www.notion.so/5169208ec79244068b91ed5f9fc70017)

---

### Importing and exporting context from a file | 파일에서 컨텍스트 가져오기 및 내보내기

Often, components in different files will need access to the same context. This is why it’s common to declare contexts in a separate file. Then you can use the `[export` statement]([https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export)) to make context available for other files:

> 서로 다른 파일에 있는 컴포넌트가 동일한 컨텍스트에 액세스해야 하는 경우가 종종 있습니다. 그렇기 때문에 컨텍스트를 별도의 파일에 선언하는 것이 일반적입니다. 그런 다음 `[export statement](<https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export>)`을 사용하여 다른 파일에서 컨텍스트를 사용할 수 있도록 설정할 수 있습니다:

```jsx
// Contexts.js
import { createContext } from 'react';

export const ThemeContext = createContext('light');
export const AuthContext = createContext(null);
```

Components declared in other files can then use the `[import](<https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/import>)` statement to read or provide this context:

> 다른 파일에서 선언된 컴포넌트는 `[import statement](<https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/import>)`을 사용하여 이 컨텍스트를 읽거나 제공할 수 있습니다:

```jsx
// Button.js
import { ThemeContext } from './Contexts.js';

function Button() {
  const theme = useContext(ThemeContext);
  // ...
}
```

```jsx
// App.js
import { ThemeContext, AuthContext } from './Contexts.js';

function App() {
  // ...
  return (
    <ThemeContext.Provider value={theme}>
      <AuthContext.Provider value={currentUser}>
        <Page />
      </AuthContext.Provider>
    </ThemeContext.Provider>
  );
}
```

This works similar to [importing and exporting components.](https://beta.reactjs.org/learn/importing-and-exporting-components)

> 이는 [컴포넌트 가져오기 및 내보내기](https://www.notion.so/1-2-import-export-Importing-and-Exporting-Components-fbba7630aab74680826121c7f90b09b9)와 유사한 방식으로 작동합니다.

---

## Troubleshooting

| 문제해결

### I can’t find a way to change the context value | 컨텍스트 값을 변경하는 방법을 찾을 수 없어요.

Code like this specifies the _default_ context value:

> 이러한 코드는 _기본_ 컨텍스트 값을 지정합니다:

```jsx
const ThemeContext = createContext('light');
```

This value never changes. React only uses this value as a fallback if it can’t find a matching provider above.

To make context change over time, [add state and wrap components in a context provider.](https://beta.reactjs.org/reference/react/useContext#updating-data-passed-via-context)

> 이 값은 절대 변하지 않습니다. React는 위에서 일치하는 공급자를 찾을 수 없는 경우에만 이 값을 대체하여 사용합니다.

시간이 지남에 따라 컨텍스트가 변경되도록 하려면 [컨텍스트 공급자에 state를 추가하고 컴포넌트를 감싸세요.](https://www.notion.so/5169208ec79244068b91ed5f9fc70017)