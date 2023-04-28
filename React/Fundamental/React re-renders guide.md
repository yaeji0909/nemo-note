When talking about React performance, there are two major stages that we need to care about:
-   **initial render** - happens when a component first appears on the screen
-   **re-render** - second and any consecutive render of a component that is already on the screen
>React 성능에 대해 이야기할 때, 우리가 신경 써야 할 두 가지 주요 단계가 있습니다:
>- 초기 렌더링 - 컴포넌트가 화면에 처음 나타날 때 발생합니다. 
>- 재렌더링 - 이미 화면에 있는 컴포넌트의 두 번째 및 모든 연속 렌더링

Re-render happens when React needs to update the app with some new data. Usually, this happens as a result of a user interacting with the app or some external data coming through via an asynchronous request or some subscription model.
>재렌더링은 React가 새로운 데이터로 앱을 업데이트해야 할 때 발생합니다. 
>일반적으로 사용자가앱과 상호작용하거나 비동기 요청 또는 일부 구독 모델을 통해 들어오는 일부 외부 데이터로 인해 발생합니다.

Non-interactive apps that don’t have any asynchronous data updates will **never** re-render, and therefore don’t need to care about re-renders performance optimization.

>비동기 데이터 업데이트가 없는 비대화형 앱은 다시 렌더링하지 않으므로 재렌더링 성능 최적화에 대해 신경 쓸 필요가 없습니다.

### [🧐 What is a necessary and unnecessary re-render?](https://www.developerway.com/posts/react-re-renders-guide#part1.1)

### 필요한 렌더링과 불필요한 렌더링은 무엇인가요?

**Necessary re-render** - re-render of a component that is the source of the changes, or a component that directly uses the new information. For example, if a user types in an input field, the component that manages its state needs to update itself on every keystroke, i.e. re-render.

>**필수 렌더링** - 변경 사항의 원인이 되는 컴포넌트 또는 새 정보를 직접 사용하는 컴포넌트를 다시 렌더링하는 것을 말합니다. 예를 들어 사용자가 입력 필드에 입력하는 경우, 입력 필드의 state를 관리하는 컴포넌트는 키 입력 시마다 렌더링을 다시 수행해야 합니다.

**Unnecessary re-render** - re-render of a component that is propagated through the app via different re-renders mechanisms due to either mistake or inefficient app architecture. For example, if a user types in an input field, and the entire page re-renders on every keystroke, the page has been re-rendered unnecessarily.

>**불필요한 리렌더링** - 실수 또는 비효율적인 앱 아키텍처로 인해 여러 리렌더링 메커니즘을 통해 앱에 전파되는 컴포넌트의 리렌더링. 예를 들어 사용자가 입력 필드에 입력할 때마다 페이지 전체가 다시 렌더링되는 경우 페이지가 불필요하게 다시 렌더링된 것입니다.

Unnecessary re-renders by themselves are **not a problem**: React is very fast and usually able to deal with them without users noticing anything.

>불필요한 재렌더링 자체는 문제가 되지 않습니다.
>React는 매우 빠르며 일반적으로 사용자가 눈치채지 못할 정도로 처리할 수 있습니다.

However, if re-renders happen too often and/or on very heavy components, this could lead to user experience appearing “laggy”, visible delays on every interaction, or even the app becoming completely unresponsive.

>그러나 리렌더링이 너무 자주 발생하거나 매우 무거운 컴포넌트에서 발생하면 사용자 경험이 "지연"되거나 모든 상호작용에서 눈에 띄는 지연이 발생하거나 심지어 앱이 완전히 응답하지 않을 수도 있습니다.

---

## [When React component re-renders itself?](https://www.developerway.com/posts/react-re-renders-guide#part2)

### React 컴포넌트가 언제 스스로 리렌더링하나요?

There are four reasons why a component would re-render itself: state changes, parent (or children) re-renders, context changes, and hooks changes. There is also a big myth: that re-renders happen when the component’s props change. By itself, it’s not true (see the explanation below).

**>컴포넌트가 리렌더링되는 이유에는 4가지가 있는데,
>state 변경
>부모(또는 자식) 리렌더링
>컨텍스트 변경
>hook이 변경될 때** 
>입니다. 
>
>컴포넌트의 프로퍼티가 변경되면 리렌더링이 발생한다는 잘못된 상식도 있습니다. 그 자체로는 사실이 아닙니다.

### [🧐 Re-renders reason: state changes](https://www.developerway.com/posts/react-re-renders-guide#part2.1)

### Re-render되는 이유: state의 변화

When a component’s state changes, it will re-render itself. Usually, it happens either in a callback or in `useEffect` hook.
State changes are the “root” source of all re-renders.

>컴포넌트의 state가 변경되면 스스로 다시 렌더링합니다. 보통 콜백이나 useEffect hook에서 발생합니다. state 변경은 모든 리렌더링의 "root" 소스입니다.

[See example in codesandbox](https://codesandbox.io/s/part2-1-re-renders-because-of-state-ngh8uc?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part2-state-changes-example.png)

### [🧐 Re-renders reason: parent re-renders](https://www.developerway.com/posts/react-re-renders-guide#part2.2)

### Re-render되는 이유: 부모가 재렌더링시
A component will re-render itself if its parent re-renders. Or, if we look at this from the opposite direction: when a component re-renders, it also re-renders all its children.

It always goes “down” the tree: the re-render of a child doesn’t trigger the re-render of a parent. (There are a few caveats and edge cases here, see the full guide for more details: [The mystery of React Element, children, parents and re-renders](https://www.developerway.com/posts/react-elements-children-parents)).

>컴포넌트는 부모 컴포넌트가 다시 렌더링하면 자신도 다시 렌더링합니다. 또는 반대 방향에서 보면 한 컴포넌트가 다시 렌더링하면 모든 자식도 다시 렌더링합니다.
>
>자식의 리렌더링이 부모의 리렌더링을 트리거하지 않고 항상 트리의 "아래"로 이동합니다. (여기에는 몇 가지 주의 사항과 예외 사례가 있으며, 자세한 내용은 전체 가이드를 참조하세요: React 엘리먼트, 자식, 부모, 리렌더의 미스터리).

[See example in codesandbox](https://codesandbox.io/s/part-2-2-re-renders-because-of-parent-b0xvxt?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part2-parent-example.png)

### [🧐 Re-renders reason: context changes](https://www.developerway.com/posts/react-re-renders-guide#part2.3)

### Re-render되는 이유: context의 변화

When the value in Context Provider changes, **all** components that use this Context will re-render, even if they don’t use the changed portion of the data directly. Those re-renders can not be prevented with memoization directly, but there are a few workarounds that can simulate it (see [Part 7: preventing re-renders caused by Context](https://www.developerway.com/posts/react-re-renders-guide#part7)).

>컨텍스트 공급자의 값이 변경되면 이 컨텍스트를 사용하는 모든 컴포넌트는 데이터의 변경된 부분을 직접 사용하지 않더라도 다시 렌더링합니다. 이러한 재렌더링은 메모화를 통해 직접 방지할 수는 없지만, 이를 시뮬레이션할 수 있는 몇 가지 해결 방법이 있습니다(7부: 컨텍스트로 인한 재렌더링 방지 참조).

[See example in codesandbox](https://codesandbox.io/s/part-2-3-re-render-because-of-context-i75lwh?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part2-context-example.png)

### [🧐 Re-renders reason: hooks changes](https://www.developerway.com/posts/react-re-renders-guide#part2.4)

### Re-render되는 이유: hook의 변화

Everything that is happening inside a hook “belongs” to the component that uses it. The same rules regarding Context and State changes apply here:
-   state change inside the hook will trigger an **unpreventable** re-rerender of the “host” component
-   if the hook uses Context and Context’s value changes, it will trigger an **unpreventable** re-rerender of the “host” component
Hooks can be chained. Every single hook inside the chain still “belongs” to the “host” component, and the same rules apply to any of them.

>훅 내부에서 일어나는 모든 일은 훅을 사용하는 컴포넌트에 "속해" 있습니다. 컨텍스트 및 state 변경에 관한 동일한 규칙이 여기에도 적용됩니다.
>
>훅 내부의 state 변경은 "호스트" 컴포넌트의 예측할 수 없는 재렌더링을 트리거합니다.
>훅이 컨텍스트를 사용하고 컨텍스트의 값이 변경되면 "호스트" 컴포넌트의 예방할 수 없는 재렌더링이 트리거됩니다.
>
>훅은 체인으로 연결될 수 있습니다. 체인 안의 모든 훅은 여전히 "호스트" 컴포넌트에 "소속"되며, 모든 훅에 동일한 규칙이 적용됩니다.

[See example in codesandbox](https://codesandbox.io/s/part-2-4-re-render-because-of-hooks-5kpdrp?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part2-hooks-example.png)

### [⛔️ Re-renders reason: props changes (the big myth)](https://www.developerway.com/posts/react-re-renders-guide#part2.5)

### Re-render되는 이유: props의 변화

It doesn’t matter whether the component’s props change or not when talking about re-renders of not memoized components.

In order for props to change, they need to be updated by the parent component. This means the parent would have to re-render, which will trigger re-render of the child component regardless of its props.

memoized되지 않은 컴포넌트를 다시 렌더링할 때는 컴포넌트의 props이 변경되었는지 여부는 중요하지 않습니다.
props가 변경되려면 부모 컴포넌트가 업데이트되야 합니다. 즉, 부모 컴포넌트가 다시 렌더링해야 하며, 그러면 자식 컴포넌트의 props에 관계없이 다시 렌더링이 트리거됩니다.

메모화 기법(React.memo, useMemo)을 사용하는 경우에만 props 변경이 중요해집니다.


[See example in codesandbox](https://codesandbox.io/s/part-2-5-re-render-props-not-relevant-2b8o0p?file=/src/App.tsx)

Only when memoization techniques are used (`React.memo`, `useMemo`), then props change becomes important.

![](https://www.developerway.com/assets/react-re-renders-guide/part2-props-myth.png)

---

## [Preventing re-renders with composition](https://www.developerway.com/posts/react-re-renders-guide#part3)
This part is also available as a video](https://www.youtube.com/watch?v=7sgBhmLjVws)

### [⛔️ Antipattern: Creating components in render function](https://www.developerway.com/posts/react-re-renders-guide#part3.1)

Creating components inside render function of another component is an anti-pattern that can be the biggest performance killer. On every re-render React will re-mount this component (i.e. destroy it and re-create it from scratch), which is going to be much slower than a normal re-render. On top of that, this will lead to such bugs as:

-   possible “flashes” of content during re-renders
-   state being reset in the component with every re-render
-   useEffect with no dependencies triggered on every re-render
-   if a component was focused, focus will be lost

⛔️ 안티패턴: 렌더 함수에서 컴포넌트 생성하기
다른 컴포넌트의 렌더 함수 안에 컴포넌트를 생성하는 것은 가장 큰 성능 저하 요인이 될 수 있는 안티패턴입니다. 재렌더링할 때마다 React는 이 컴포넌트를 다시 마운트(즉, 삭제하고 처음부터 다시 생성)하게 되므로 일반적인 재렌더링보다 훨씬 느려집니다. 
게다가 다음과 같은 버그가 발생할 수 있습니다:

다시 렌더링하는 동안 콘텐츠가 '깜박일' 가능성
리렌더링할 때마다 컴포넌트에서 state가 리셋됨
다시 렌더링할 때마다 종속성이 없는 useEffect가 트리거됩니다.
컴포넌트에 포커스가 맞춰져 있으면 포커스가 손실됨

[See example in codesandbox](https://codesandbox.io/s/part-3-1-creating-components-inline-t2vmkj?file=/src/App.tsx)

Additional resources to read: [How to write performant React code: rules, patterns, do's and don'ts](https://www.developerway.com/posts/how-to-write-performant-react-code)

![](https://www.developerway.com/assets/react-re-renders-guide/part3-creating-components.png)

### [✅ Preventing re-renders with composition: moving state down](https://www.developerway.com/posts/react-re-renders-guide#part3.2)

This pattern can be beneficial when a heavy component manages state, and this state is only used on a small isolated portion of the render tree. A typical example would be opening/closing a dialog with a button click in a complicated component that renders a significant portion of a page.

In this case, the state that controls modal dialog appearance, dialog itself, and the button that triggers the update can be encapsulated in a smaller component. As a result, the bigger component won’t re-render on those state changes.

컴포지션으로 재렌더링 방지: state 아래로 이동
이 패턴은 무거운 컴포넌트가 state를 관리하고 이 state가 렌더 트리에서 분리된 작은 부분에만 사용되는 경우에 유용할 수 있습니다. 페이지의 상당 부분을 렌더링하는 복잡한 컴포넌트에서 버튼 클릭으로 대화 상자를 열거나 닫는 것이 대표적인 예입니다.

이 경우 모달 대화 상자 모양, 대화 상자 자체, 업데이트를 트리거하는 버튼을 제어하는 state를 더 작은 컴포넌트에 캡슐화할 수 있습니다. 결과적으로 더 큰 컴포넌트는 이러한 state 변경 시 다시 렌더링되지 않습니다.

[See example in codesandbox](https://codesandbox.io/s/part-3-2-moving-state-down-vlh4gf?file=/src/App.tsx)

Additional resources to read: [The mystery of React Element, children, parents and re-renders](https://www.developerway.com/posts/react-elements-children-parents), [How to write performant React code: rules, patterns, do's and don'ts](https://www.developerway.com/posts/how-to-write-performant-react-code)

![](https://www.developerway.com/assets/react-re-renders-guide/part3-moving-state-down.png)

### [✅ Preventing re-renders with composition: children as props](https://www.developerway.com/posts/react-re-renders-guide#part3.3)
This part is also available as a video](https://www.youtube.com/watch?v=7sgBhmLjVws&t=272s)

This can also be called “wrap state around children”. This pattern is similar to “moving state down”: it encapsulates state changes in a smaller component. The difference here is that state is used on an element that wraps a slow portion of the render tree, so it can’t be extracted that easily. A typical example would be `onScroll` or `onMouseMove` callbacks attached to the root element of a component.

In this situation, state management and components that use that state can be extracted into a smaller component, and the slow component can be passed to it as `children`. From the smaller component perspective `children` are just prop, so they will not be affected by the state change and therefore won’t re-render.

[See example in codesandbox](https://codesandbox.io/s/part-3-3-children-as-props-59icyq?file=/src/App.tsx)

이를 "자식 주위에 state 감싸기"라고도 합니다. 이 패턴은 state 변화를 더 작은 컴포넌트에 캡슐화한다는 점에서 "state 아래로 이동"과 유사합니다. 여기서 차이점은 렌더링 트리의 느린 부분을 감싸는 요소에 state가 사용되므로 쉽게 추출할 수 없다는 것입니다. 일반적인 예로는 컴포넌트의 루트 엘리먼트에 연결된 onScroll 또는 onMouseMove 콜백을 들 수 있습니다.

이 경우 state 관리와 해당 state를 사용하는 컴포넌트를 더 작은 컴포넌트로 추출하고 느린 컴포넌트를 자식으로 전달할 수 있습니다. 작은 컴포넌트 관점에서 자식은 props에 불과하므로 state 변경의 영향을 받지 않으므로 다시 렌더링되지 않습니다.

Additional resources to read: [The mystery of React Element, children, parents and re-renders](https://www.developerway.com/posts/react-elements-children-parents)

![](https://www.developerway.com/assets/react-re-renders-guide/part3-passing-as-children.png)

### [✅ Preventing re-renders with composition: components as props](https://www.developerway.com/posts/react-re-renders-guide#part3.4)
This part is also available as a video](https://www.youtube.com/watch?v=7sgBhmLjVws&t=462s)

Pretty much the same as the previous pattern, with the same behavior: it encapsulates the state inside a smaller component, and heavy components are passed to it as props. Props are not affected by the state change, so heavy components won’t re-render.

Can be useful when a few heavy components are independent from the state, but can’t be extracted as children as a group.

이전 패턴과 거의 동일하며 동작 방식도 동일합니다. 작은 컴포넌트 안에 state를 캡슐화하고 무거운 컴포넌트는 프로퍼티로 전달합니다. 프로퍼티는 state 변경의 영향을 받지 않으므로 무거운 컴포넌트가 다시 렌더링되지 않습니다.

몇 개의 무거운 컴포넌트가 state와 독립적이지만 그룹으로 자식으로 추출할 수 없는 경우에 유용할 수 있습니다.

[See example in codesandbox](https://codesandbox.io/s/part-3-4-passing-components-as-props-9h3o5u?file=/src/App.tsx)

Read more about passing components as props here: [React component as prop: the right way™️](https://www.developerway.com/posts/react-component-as-prop-the-right-way), [The mystery of React Element, children, parents and re-renders](https://www.developerway.com/posts/react-elements-children-parents)

![](https://www.developerway.com/assets/react-re-renders-guide/part3-passing-as-props.png)

---

## [Preventing re-renders with React.memo]
This part is also available as a video](https://youtu.be/feEY3Qajrwg)

Wrapping a component in `React.memo` will stop the downstream chain of re-renders that is triggered somewhere up the render tree, unless this component’s props have changed.

This can be useful when rendering a heavy component that is not dependent on the source of re-renders (i.e. state, changed data).

컴포넌트를 React.memo로 래핑하면 이 컴포넌트의 프로퍼티가 변경되지 않는 한 렌더링 트리의 어딘가에서 트리거되는 리렌더링의 다운스트림 체인이 중지됩니다.

이는 리렌더링 소스(예: state, 변경된 데이터)에 의존하지 않는 무거운 컴포넌트를 렌더링할 때 유용할 수 있습니다.

[See example in codesandbox](https://codesandbox.io/s/part-4-simple-memo-fz4xhw?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part4-memo-normal-example.png)

### [✅ React.memo: component with props](https://www.developerway.com/posts/react-re-renders-guide#part4.1)

**All props** that are not primitive values have to be memoized for React.memo to work

기본값이 아닌 모든 프로퍼티를 메모화해야 React.memo가 작동합니다.

[See example in codesandbox](https://codesandbox.io/s/part-4-1-memo-on-component-with-props-fq55hm?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part4-memo-with-props.png)

### [✅ React.memo: components as props or children](https://www.developerway.com/posts/react-re-renders-guide#part4.2)

`React.memo` has to be applied to the elements passed as children/props. Memoizing the parent component will not work: children and props will be objects, so they will change with every re-render.

자식/props으로 전달된 엘리먼트에 React.memo를 적용해야 합니다. 부모 컴포넌트를 메모하는 것은 작동하지 않습니다. 자식과 props은 객체이므로 다시 렌더링할 때마다 변경됩니다.

See here for more details on how memoization works for children/parent relationships: [The mystery of React Element, children, parents and re-renders](https://www.developerway.com/posts/react-elements-children-parents)

[See example in codesandbox](https://codesandbox.io/s/part-4-2-memo-on-components-in-props-55tebl?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part4-memo-as-props.png)

---

## [Improving re-renders performance with useMemo/useCallback](https://www.developerway.com/posts/react-re-renders-guide#part5)

### [⛔️ Antipattern: unnecessary useMemo/useCallback on props](https://www.developerway.com/posts/react-re-renders-guide#part5.1)

Memoizing props by themselves will not prevent re-renders of a child component. If a parent component re-renders, it will trigger re-render of a child component regardless of its props.

props 자체를 메모화해도 자식 컴포넌트의 재렌더링을 막지는 못합니다. 부모 컴포넌트가 리렌더링되면 props에 관계없이 자식 컴포넌트의 리렌더링이 트리거됩니다.

[See example in codesandbox](https://codesandbox.io/s/part-5-1-unnecessary-usememo-lmk8fq?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part5-unnecessary-usememo-on-props.png)

### [✅ Necessary useMemo/useCallback](https://www.developerway.com/posts/react-re-renders-guide#part5.2)

If a child component is wrapped in `React.memo`, all props that are not primitive values have to be memoized

자식 컴포넌트가 React.memo에 감싸져 있으면 모든 props는 (원시값이 아닌) memoize되야합니다.

[See example in codesandbox](https://codesandbox.io/s/part-5-2-usememo-in-props-trx97x?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part5-necessary-usememo-props.png)

If a component uses non-primitive value as a dependency in hooks like `useEffect`, `useMemo`, `useCallback`, it should be memoized.

컴포넌트가 사용효과, 사용메모, 사용콜백 같은 hook에서 원시값이 아닌 값을 종속성으로 사용하는 경우, 이를 메모화해야 합니다.

[See example in codesandbox](https://codesandbox.io/s/part-5-2-usememo-in-effect-88tbov)

![](https://www.developerway.com/assets/react-re-renders-guide/part5-necessary-usememo-dep.png)

### [✅ useMemo for expensive calculations](https://www.developerway.com/posts/react-re-renders-guide#part5.3)

One of the use cases for `useMemo` is to avoid expensive calculations on every re-render.

`useMemo` has its cost (consumes a bit of memory and makes initial render slightly slower), so it should not be used for every calculation. In React, mounting and updating components will be the most expensive calculation in most cases (unless you’re actually calculating prime numbers, which you shouldn’t do on the frontend anyway).

As a result, the typical use case for `useMemo` would be to memoize React elements. Usually parts of an existing render tree or results of generated render tree, like a map function that returns new elements.

The cost of “pure” javascript operations like sorting or filtering an array is usually negligible, compare to components updates.

사용 메모의 사용 사례 중 하나는 렌더링할 때마다 비용이 많이 드는 계산을 피하는 것입니다.

사용메모에는 비용이 발생하므로(약간의 메모리를 소모하고 초기 렌더링이 약간 느려짐) 모든 계산에 사용되어서는 안 됩니다. React에서는 대부분의 경우 컴포넌트를 마운트하고 업데이트하는 것이 가장 비용이 많이 드는 계산이 될 것입니다(어차피 프론트엔드에서 하지 말아야 하는 소수를 계산하는 경우가 아니라면).

따라서 useMemo의 일반적인 사용 사례는 React 엘리먼트를 메모하는 것입니다. 보통 기존 렌더 트리의 일부 또는 새로운 엘리먼트를 반환하는 맵 함수처럼 생성된 렌더 트리의 결과입니다.

배열을 정렬하거나 필터링하는 것과 같은 "순수한" 자바스크립트 연산의 비용은 일반적으로 컴포넌트 업데이트에 비해 무시할 수 있는 수준입니다.

[See example in codesandbox](https://codesandbox.io/s/part-5-3-usememo-for-expensive-calculations-trx97x?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part5-necessary-usememo-complex.png)

---

## [Improving re-render performance of lists](https://www.developerway.com/posts/react-re-renders-guide#part6)

In addition to the regular re-renders rules and patterns, the `key` attribute can affect the performance of lists in React.

**Important**: just providing `key` attribute will not improve lists' performance. To prevent re-renders of list elements you need to wrap them in `React.memo` and follow all of its best practices.

Value in `key` should be a string, that is consistent between re-renders for every element in the list. Typically, item’s `id` or array’s `index` is used for that.

It is okay to use array’s `index` as key, if the list is **static**, i.e. elements are not added/removed/inserted/re-ordered.

Using array’s index on dynamic lists can lead to:

-   bugs if items have state or any uncontrolled elements (like form inputs)
-   degraded performance if items are wrapped in React.memo

Read about this in more details here: [React key attribute: best practices for performant lists](https://www.developerway.com/posts/react-key-attribute).

일반적인 리렌더링 규칙과 패턴 외에도 키 속성은 React에서 목록의 성능에 영향을 줄 수 있습니다.

중요: 키 어트리뷰트만 제공한다고 해서 목록의 성능이 향상되지는 않습니다. 목록 요소의 재렌더링을 방지하려면 목록 요소를 React.memo로 감싸고 모든 모범 사례를 따라야 합니다.

키의 값은 목록의 모든 요소에 대한 리렌더링 간에 일관된 문자열이어야 합니다. 일반적으로 항목의 ID 또는 배열의 인덱스가 사용됩니다.

목록이 정적인 경우, 즉 요소가 추가/제거/삽입/재순서가 변경되지 않는 경우에는 배열의 인덱스를 키로 사용해도 괜찮습니다.

동적 목록에서 배열의 인덱스를 사용하면 다음과 같은 문제가 발생할 수 있습니다:

항목에 state 또는 제어되지 않는 요소(예: 양식 입력)가 있는 경우 버그가 발생할 수 있습니다.
항목이 React.memo로 래핑된 경우 성능 저하
이에 대한 자세한 내용은 여기에서 읽어보세요: React 핵심 속성: 성능 좋은 목록을 위한 모범 사례.

[See example in codesandbox - static list](https://codesandbox.io/s/part-6-static-list-with-index-and-id-as-key-7i0ebi?file=/src/App.tsx)

[See example in codesandbox - dynaminc list](https://codesandbox.io/s/part-6-dynamic-list-with-index-and-id-as-key-s50knr?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part6-lists-example.png)

### [⛔️ Antipattern: random value as key in lists](https://www.developerway.com/posts/react-re-renders-guide#part6.1)

Randomly generated values should never be used as values in `key` attribute in lists. They will lead to React re-mounting items on every re-render, which will lead to:

-   very poor performance of the list
-   bugs if items have state or any uncontrolled elements (like form inputs)

무작위로 생성된 값을 목록의 키 속성의 값으로 사용해서는 안 됩니다. 그러면 렌더링할 때마다 React가 항목을 다시 마운트하게 됩니다:

목록의 성능이 매우 저하됨
항목에 state 또는 제어되지 않는 요소(예: 양식 입력)가 있는 경우 버그 발생

[See example in codesandbox](https://codesandbox.io/s/part-6-1-random-values-in-keys-z1zhy6?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part6-lists-antipattern.png)

---

## [Preventing re-renders caused by Context](https://www.developerway.com/posts/react-re-renders-guide#part7)

### [✅ Preventing Context re-renders: memoizing Provider value](https://www.developerway.com/posts/react-re-renders-guide#part7.1)

If Context Provider is placed not at the very root of the app, and there is a possibility it can re-render itself because of changes in its ancestors, its value should be memoized.

컨텍스트 프로바이더가 앱의 루트가 아닌 다른 곳에 배치되어 있고, 조상의 변경으로 인해 다시 렌더링될 가능성이 있는 경우 해당 값을 기억해 두어야 합니다.

[See example in codesandbox](https://codesandbox.io/s/part-7-1-memoize-context-provider-value-qgn0me?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part7-context-provider-memo.png)

### [✅ Preventing Context re-renders: splitting data and API](https://www.developerway.com/posts/react-re-renders-guide#part7.2)

If in Context there is a combination of data and API (getters and setters) they can be split into different Providers under the same component. That way, components that use API only won’t re-render when the data changes.

컨텍스트에 데이터와 API(게터와 세터)의 조합이 있는 경우 동일한 컴포넌트 아래에서 서로 다른 프로바이더로 분할할 수 있습니다. 이렇게 하면 API만 사용하는 컴포넌트는 데이터가 변경되어도 다시 렌더링되지 않습니다.

이 패턴에 대한 자세한 내용은 여기를 참조하세요: 컨텍스트가 있는 고성능 React 앱을 작성하는 방법

Read more about this pattern here: [How to write performant React apps with Context](https://www.developerway.com/posts/how-to-write-performant-react-apps-with-context)

[See example in codesandbox](https://codesandbox.io/s/part-7-2-split-context-data-and-api-r8lsws?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part7-context-split-api.png)

### [✅ Preventing Context re-renders: splitting data into chunks](https://www.developerway.com/posts/react-re-renders-guide#part7.3)

If Context manages a few independent data chunks, they can be split into smaller providers under the same provider. That way, only consumers of changed chunk will re-render.

컨텍스트가 몇 개의 독립적인 데이터 청크를 관리하는 경우, 동일한 공급자 아래에서 더 작은 공급자로 분할할 수 있습니다. 이렇게 하면 변경된 청크의 소비자만 다시 렌더링할 수 있습니다.

Read more about this pattern here: [How to write performant React apps with Context](https://www.developerway.com/posts/how-to-write-performant-react-apps-with-context)

[See example in codesandbox](https://codesandbox.io/s/part-7-3-split-context-into-chunks-dbg20m?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part7-context-split-data.png)

### [✅ Preventing Context re-renders: Context selectors](https://www.developerway.com/posts/react-re-renders-guide#part7.4)

There is no way to prevent a component that uses a portion of Context value from re-rendering, even if the used piece of data hasn’t changed, even with `useMemo` hook.

Context selectors, however, could be faked with the use of higher-order components and `React.memo`.

사용 된 데이터 조각이 변경되지 않았더라도 사용 메모 훅을 사용하더라도 컨텍스트 값의 일부를 사용하는 컴포넌트가 다시 렌더링되는 것을 방지 할 수있는 방법은 없습니다.

하지만 컨텍스트 선택자는 고차 컴포넌트와 React.memo를 사용하면 위조될 수 있습니다.

이 패턴에 대한 자세한 내용은 여기를 읽어보세요: React Hook 시대의 고차 컴포넌트

Read more about this pattern here: [Higher-Order Components in React Hooks era](https://www.developerway.com/posts/higher-order-components-in-react-hooks-era)

[See example in codesandbox](https://codesandbox.io/s/part-7-4-context-selector-lc8n5g?file=/src/App.tsx)

![](https://www.developerway.com/assets/react-re-renders-guide/part7-context-selectors.png)


출처 : https://www.developerway.com/posts/react-re-renders-guide