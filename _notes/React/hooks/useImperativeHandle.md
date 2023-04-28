
## useImperativeHandle

useRef를 한정적으로 사용하고 싶을 때 사용하는 hook

### Usage

기본적으로 컴포넌트는 자식 컴포넌트의 DOM 노드를 부모 컴포넌트에 노출하지 않습니다. 예를 들어 `MyInput`의 부모 컴포넌트가 `<input>` DOM 노드에 [접근하려면](https://react-ko.vercel.app/learn/manipulating-the-dom-with-refs) [`forwardRef`](https://react-ko.vercel.app/reference/react/forwardRef)를 사용하여 선택적으로 참조에 포함해야 합니다:

```jsx
import { forwardRef } from 'react';  
const MyInput = forwardRef(function MyInput(props, ref) {  
return <input {...props} ref={ref} />;  
});
```


위의 코드에서 [`MyInput`에 대한 ref는 `<input>` DOM 노드를 받게 됩니다.](https://react-ko.vercel.app/reference/react/forwardRef#exposing-a-dom-node-to-the-parent-component) 그러나 대신 사용자 지정 값을 노출할 수 있습니다. 노출된 핸들을 사용자 정의하려면 컴포넌트의 최상위 레벨에서 `useImperativeHandle`을 호출하세요.

```jsx
import { forwardRef, useImperativeHandle } from 'react';  

const MyInput = forwardRef(function MyInput(props, ref) {  

useImperativeHandle(ref, () => {  
return {  
// ... your methods ...  
// ... 메소드를 여기에 입력하세요 ...  
};  
}, []);  
return <input {...props} />;  
});
```

위의 코드에서 `<input>`에 대한 ref는 더 이상 전달되지 않습니다.
예를 들어 전체 `<input>` DOM 노드를 노출하지 않고 `focus`와 `scrollIntoView`의 두 메서드만을 노출하고 싶다고 가정해 봅시다. 그러기 위해서는 실제 브라우저 DOM을 별도의 ref에 유지해야 합니다. 그리고 `useImperativeHandle`을 사용하여 부모 컴포넌트에서 호출할 메서드만 있는 핸들을 노출합니다:

이제 부모 컴포넌트가 `MyInput`에 대한 ref를 가져오면 `focus` 및 `scrollIntoView` 메서드를 호출할 수 있습니다. 그러나 기본 `<input>` DOM 노드의 전체 액세스 권한은 없습니다.

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

#### -> 사용자 정의 명령만을 노출한다


