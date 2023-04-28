
`
> `forwardRef`를 사용하면 컴포넌트가 [ref](https://www.notion.so/4-2-ref-DOM-Manipulating-the-DOM-with-Refs-dc08e3777cf84bcabb3d07e669552420)를 사용하여 부모 컴포넌트에 DOM 노드를 노출할 수 있습니다.

```jsx
const SomeComponent = forwardRef(render)
```

### `forwardRef(render)`


> 컴포넌트가 ref를 수신하여 하위 컴포넌트로 전달하도록 하려면 forwardRef()를 호출하세요:

```jsx
import { forwardRef } from 'react';
const MyInput = forwardRef(function MyInput(props, ref) {
  // ...
});
```

### Parameters

| 매개변수

> `render`: 컴포넌트의 렌더링 함수입니다. React는 컴포넌트가 부모로부터 받은 props와 `ref`를 가지고 이 함수를 호출합니다. 반환하는 JSX는 컴포넌트의 출력이 됩니다.

### Returns

| 반환값

> `forwardRef`는 JSX에서 렌더링할 수 있는 React 컴포넌트를 반환합니다. 일반 함수로 정의된 React 컴포넌트와 달리, `forwardRef`가 반환하는 컴포넌트는 `ref` 프로퍼티를 수신할 수도 있습니다.

### Caveats

> **주의사항**

> 
> -   Strict Mode에서 React는 [실수로 발생한 불순물을 찾기 위해](https://beta.reactjs.org/reference/react/forwardRef#my-initializer-or-updater-function-runs-twice) 렌더 함수를 **두 번** 호출합니다. 이는 개발 전용 동작이며 프로덕션에는 영향을 주지 않습니다. 렌더링 함수가 순수하다면(그래야만 하는 것처럼) 컴포넌트의 로직에 영향을 미치지 않을 것입니다. 호출 중 하나의 결과는 무시됩니다.

### `render` function

| `render` 함수

> `forwardRef`는 렌더 함수를 인자로 받습니다. React는 이 함수를 `props`와 `ref`로 호출합니다:

```jsx
const MyInput = forwardRef(function MyInput(props, ref) {
  return (
    <label>
      {props.label}
      <input ref={ref} />
    </label>
  );
});
```

### Parameters

| 매개변수


    > `props` : 부모 컴포넌트가 전달한 props입니다.

-   `ref`: The `ref` attribute passed by the parent component. The `ref` can be an object or a function. If the parent component has not passed a ref, it will be `null`. You should either pass the `ref` you receive to another component, or pass it to `[useImperativeHandle`.]([https://beta.reactjs.org/reference/react/useImperativeHandle](https://beta.reactjs.org/reference/react/useImperativeHandle))
    
    > `ref`: 부모 컴포넌트가 전달한 `ref` 어트리뷰트입니다. `ref`는 객체나 함수일 수 있습니다. 부모 컴포넌트가 참조를 전달하지 않은 경우 `null`이 됩니다. 받은 `ref`를 다른 컴포넌트에 전달하거나 `[useImperativeHandle](<https://www.notion.so/useImperativeHandle-b0e84b1fac3c4057a5ce00b3187c5447>)`에 전달해야 합니다.
    

### Returns

| 반환값

`forwardRef` returns a React component that you can render in JSX. Unlike React components defined as plain functions, the component returned by `forwardRef` is able to take a `ref` prop.

> `forwardRef`는 JSX에서 렌더링할 수 있는 React 컴포넌트를 반환합니다. 일반 함수로 정의된 React 컴포넌트와 달리, `forwardRef`가 반환하는 컴포넌트는 `ref` prop을 받을 수 있습니다.

## Usage

| 사용법

### Exposing a DOM node to the parent component

| 부모 컴포넌트에 DOM 노드 노출하기

By default, each component’s DOM nodes are private. However, sometimes it’s useful to expose a DOM node to the parent—for example, to allow focusing it. To opt in, wrap your component definition into `forwardRef()`:

> 기본적으로 각 컴포넌트의 DOM 노드는 비공개입니다. 그러나 때로는 포커싱을 허용하기 위해 부모에 DOM 노드를 노출하는 것이 유용할 수 있습니다. 이를 선택하려면 컴포넌트 정의를 `forwardRef()`로 감싸면 됩니다:

```jsx
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} />
    </label>
  );
});
```

You will receive a `ref` as the second argument after props. Pass it to the DOM node that you want to expose:

> props 다음에 두 번째 인수로 `ref`를 받게 됩니다. 이를 노출하려는 DOM 노드에 전달합니다:

```jsx
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} />
    </label>
  );
});
```

This lets the parent `Form` component access the `<input>` DOM node exposed by `MyInput`:

> 이렇게 하면 부모 `Form`컴포넌트가 `MyInput`에 의해 노출된 `<input>` DOM 노드에 접근할 수 있습니다:

```jsx
function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <MyInput label="Enter your name:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}
```

This `Form` component [passes a ref](https://react.dev/reference/react/useRef#manipulating-the-dom-with-a-ref) to `MyInput`. The `MyInput` component _forwards_ that ref to the `<input>` browser tag. As a result, the `Form` component can access that `<input>` DOM node and call `[focus()](<https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus>)` on it.

> 이 `Form` 컴포넌트는 `MyInput`에 대한 [ref를 전달](https://www.notion.so/useRef-ed58be78855e4c1ca1c88041109f06ca)합니다. `MyInput` 컴포넌트는 해당 ref를 `<input>` 브라우저 태그에 전달합니다. 그 결과 `Form` 컴포넌트는 해당 `<input>` DOM 노드에 접근하여 이 노드에서 `[focus()](<https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus>)`를 호출할 수 있습니다.

Keep in mind that by exposing a ref to the DOM node inside your component, you’re making it harder to change your component’s internals later. You will typically expose DOM nodes from reusable low-level components like buttons or text inputs, but you won’t do it for application-level components like an avatar or a comment.

> 컴포넌트 내부의 DOM 노드에 대한 ref를 노출하면 나중에 컴포넌트의 내부를 변경하기가 더 어려워진다는 점에 유의하세요. 일반적으로 버튼이나 텍스트 입력과 같이 재사용 가능한 로우 레벨 컴포넌트에서 DOM 노드를 노출하지만 아바타나 코멘트와 같은 애플리케이션 레벨 컴포넌트에서는 노출하지 않습니다.

### Try out some examples

> 몇 가지 도전 과제를 시도해 보세요.

### Example 1 of 2: Focusing a text input

> 과제 1 / 2: 텍스트 입력에 포커싱하기

Clicking the button will focus the input. The `Form` component defines a ref and passes it to the `MyInput` component. The `MyInput` component forwards that ref to the browser `<input>`. This lets the `Form` component focus the `<input>`.

> 버튼을 클릭하면 입력에 초점이 맞춰집니다. `Form` 컴포넌트는 참조를 정의하고 이를 `MyInput` 컴포넌트로 전달합니다. `MyInput` 컴포넌트는 해당 ref를 브라우저 `<input>`으로 전달합니다. 이렇게 하면 `Form` 컴포넌트가 `<input>`에 포커스를 맞출 수 있습니다.

[](https://codesandbox.io/embed/weathered-smoke-1w346v?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split)[https://codesandbox.io/embed/weathered-smoke-1w346v?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split](https://codesandbox.io/embed/weathered-smoke-1w346v?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split)

### **Example 2 of 2: Playing and pausing a video**

> 과제 2 / 2: 동영상 재생 및 일시정지

Clicking the button will call `[play()](<https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/play>)` and `[pause()](<https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/pause>)` on a `<video>` DOM node. The `App` component defines a ref and passes it to the `MyVideoPlayer` component. The `MyVideoPlayer` component forwards that ref to the browser `<video>` node. This lets the `App` component play and pause the `<video>`.

> 버튼을 클릭하면 `<video>` DOM 노드에서 `play()` 및 `pause()`를 호출합니다. `App` 컴포넌트는 ref를 정의하고 이를 `MyVideoPlayer` 컴포넌트에 전달합니다. `MyVideoPlayer` 컴포넌트는 해당 ref를 브라우저 `<video>` 노드로 전달합니다. 이렇게 하면 `App` 컴포넌트가 `<video>`를 재생하고 일시 정지할 수 있습니다.

[](https://codesandbox.io/embed/sharp-sea-s3utd4?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split)[https://codesandbox.io/embed/sharp-sea-s3utd4?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split](https://codesandbox.io/embed/sharp-sea-s3utd4?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split)

### Forwarding a ref through multiple components

| 여러 컴포넌트를 통해 ref 전달

Instead of forwarding a `ref` to a DOM node, you can forward it to your own component like `MyInput`:

> `ref`를 DOM 노드로 전달하지 않고 `MyInput`과 같은 자체 컴포넌트로 전달할 수 있습니다:

```jsx
const FormField = forwardRef(function FormField(props, ref) {
  // ...
  return (
    <>
      <MyInput ref={ref} />
      ...
    </>
  );
});
```

If that `MyInput` component forwards a ref to its `<input>`, a ref to `FormField` will give you that `<input>`:

> `MyInput` 컴포넌트가 해당 `<input>`에 대한 ref를 전달하면 `FormField`에 대한 ref가 해당 `<input>`을 제공합니다:

```jsx
function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <FormField label="Enter your name:" ref={ref} isRequired={true} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}
```

The `Form` component defines a ref and passes it to `FormField`. The `FormField` component forwards that ref to `MyInput`, which forwards this ref to a browser `<input>` DOM node. This is how `Form` accesses that DOM node.

> `Form` 컴포넌트는 ref를 정의하고 이를 `FormField`에 전달합니다. `FormField` 컴포넌트는 해당 ref를 `MyInput`으로 전달하고, 이 ref는 브라우저 `<input>` DOM 노드로 전달합니다. 이것이 `Form`이 해당 DOM 노드에 접근하는 방식입니다.

[](https://codesandbox.io/embed/hopeful-jepsen-crs8qt?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split)[https://codesandbox.io/embed/hopeful-jepsen-crs8qt?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split](https://codesandbox.io/embed/hopeful-jepsen-crs8qt?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split)

### Exposing an imperative handle instead of a DOM node

| DOM 노드 대신 명령형 핸들 노출하기

Instead of exposing an entire DOM node, you can expose a custom object, called an _imperative handle,_ with a more constrained set of methods. To do this, you’d need to define a separate ref to hold the DOM node:

> 전체 DOM 노드를 노출하는 대신 보다 제한된 메서드 집합을 사용하여 명령형 핸들이라고 하는 사용자 정의 객체를 노출할 수 있습니다. 이렇게 하려면 DOM 노드를 보유할 별도의 ref를 정의해야 합니다:

```jsx
const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  // ...

  return <input {...props} ref={inputRef} />;
});
```

Pass the `ref` you received to `[useImperativeHandle](<https://react.dev/reference/react/useImperativeHandle>)` and specify the value you want to expose to the `ref`:

> 받은 `ref`를 `[useImperativeHandle](<https://www.notion.so/useImperativeHandle-b0e84b1fac3c4057a5ce00b3187c5447>)`에 전달하고 ref에 노출할 값을 지정합니다:

```jsx
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});
```

If some component gets a ref to `MyInput`, it will only receive your `{ focus, scrollIntoView }` object instead of the DOM node. This lets you limit the information you expose about your DOM node to the minimum.

> 일부 컴포넌트가 `MyInput`에 대한 ref를 받으면 DOM 노드 대신 `{ focus, scrollIntoView }` 객체만 받습니다. 이를 통해 DOM 노드에 대해 노출하는 정보를 최소한으로 제한할 수 있습니다.

[](https://codesandbox.io/embed/sparkling-microservice-i2zsnu?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split)[https://codesandbox.io/embed/sparkling-microservice-i2zsnu?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split](https://codesandbox.io/embed/sparkling-microservice-i2zsnu?fontsize=14&hidenavigation=1&theme=dark&module=/App.js&editorsize=70&view=split)

[Read more about using imperative handles.](https://beta.reactjs.org/reference/react/useImperativeHandle)

> [명령형 핸들 사용에 대해 자세히 알아보세요.](https://www.notion.so/useImperativeHandle-b0e84b1fac3c4057a5ce00b3187c5447)

<aside> ⚠️ **Pitfall**

**Do not overuse refs.** You should only use refs for _imperative_ behaviors that you can’t express as props: for example, scrolling to a node, focusing a node, triggering an animation, selecting text, and so on.

> **ref를 과도하게 사용하지 마세요**. 노드로 스크롤하기, 노드에 포커스 맞추기, 애니메이션 트리거하기, 텍스트 선택하기 등 prop으로 표현할 수 없는 필수적인 동작에만 ref를 사용해야 합니다.

**If you can express something as a prop, you should not use a ref.** For example, instead of exposing an imperative handle like `{ open, close }` from a `Modal` component, it is better to take `isOpen` as a prop like `<Modal isOpen={isOpen} />`. [Effects](https://beta.reactjs.org/learn/synchronizing-with-effects) can help you expose imperative behaviors via props.

> **prop으로 무언가를 표현할 수 있다면 ref를 사용해서는 안 됩니다.** 예를 들어 `Modal` 컴포넌트에서 `{ open, close }`와 같은 명령형 핸들을 노출하는 대신 `<Modal isOpen = {isOpen} />` 와 같은 propdmfh `isOpen` 을 취하는것이 더 좋습니다. [Effects](https://www.notion.so/4-3-Effect-Synchronizing-with-Effects-45f483b6041544349fbc03d9b6761edf)는 props를 통해 명령형 동작을 노출하는 데 도움이 될 수 있습니다.

</aside>

## Troubleshooting

| 트러블슈팅

### My component is wrapped in `forwardRef`, but the `ref` to it is always `null`

| 내 컴포넌트가 `forwardRef`로 래핑되어 있지만, 이 컴포넌트에 대한 `ref`는 항상 `null`입니다.

This usually means that you forgot to actually use the `ref` that you received.

> 이는 일반적으로 받은 `ref` 를 실제로 사용하는 것을 잊어버렸음을 의미합니다.

For example, this component doesn’t do anything with its `ref`:

> 예를 들어, 이 컴포넌트는 `ref`로 아무 작업도 하지 않습니다:

```jsx
const MyInput = forwardRef(function MyInput({ label }, ref) {
  return (
    <label>
      {label}
      <input />
    </label>
  );
});
```

To fix it, pass the `ref` down to a DOM node or another component that can accept a ref:

> 이 문제를 해결하려면 `ref` 를 DOM 노드나 ref를 받을 수 있는 다른 컴포넌트로 전달하세요:

```jsx
const MyInput = forwardRef(function MyInput({ label }, ref) {
  return (
    <label>
      {label}
      <input ref={ref} />
    </label>
  );
});
```

The `ref` to `MyInput` could also be `null` if some of the logic is conditional:

> 일부 논리가 조건부인 경우 `MyInput`에 대한 `ref`가 `null`일 수도 있습니다:

```jsx
const MyInput = forwardRef(function MyInput({ label, showInput }, ref) {
  return (
    <label>
      {label}
      {showInput && <input ref={ref} />}
    </label>
  );
});
```

If `showInput` is `false`, then the ref won’t be forwarded to any node, and a ref to `MyInput` will remain empty. This is particularly easy to miss if the condition is hidden inside another component, like `Panel` in this example:

> `showInput`이 `false`이면 ref가 어떤 노드로도 전달되지 않으며 `MyInput`에 대한 ref는 비어 있게 됩니다. 이 예제의 `Panel`처럼 조건이 다른 컴포넌트 안에 숨겨져 있는 경우 특히 이 점을 놓치기 쉽습니다:

```jsx
const MyInput = forwardRef(function MyInput({ label, showInput }, ref) {
  return (
    <label>
      {label}
      <Panel isExpanded={showInput}>
        <input ref={ref} />
      </Panel>
    </label>
  );
});
```