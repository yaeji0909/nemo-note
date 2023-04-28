여기서 `[]`구문을 [배열 구조분해](https://ko.javascript.info/destructuring-assignment)라고 하며, 배열에서 값을 읽을 수 있습니다.

`useState`가 반환하는 배열에는 항상 정확히 두 개의 항목이 있습니다.

`[useState](<https://react-ko.vercel.app/reference/react/useState>)`를 호출하는 것은, React에게 이 컴포넌트가 무언가를 기억하기를 원한다고 말하는 것입니다.

### Hook

Hook은 **React가 [렌더링](https://react-ko.vercel.app/learn/render-and-commit#step-1-trigger-a-render) 중일 때만 사용할 수 있는** 특별한 함수입니다 (다음 페이지에서 더 자세히 설명하겠습니다). 이를 통해 다양한 React 기능을 “연결”할 수 있습니다.

! **훅(`use`로 시작하는 함수)은 “컴포넌트의 최상위 레벨” (최상위 컴포넌트 아님) 또는 [커스텀 훅](https://react-ko.vercel.app/learn/reusing-logic-with-custom-hooks)에서만 호출할 수 있습니다.** 조건, 루프 또는 기타 중첩된 함수 내부에서는 훅을 호출할 수 없습니다. 훅은 함수이지만 컴포넌트의 필요에 대한 무조건적인 선언으로 생각하면 도움이 됩니다. 파일 상단에서 모듈을 “import”하는 것과 유사하게 컴포넌트 상단에서 React 기능을 “사용”합니다.

이 쌍의 이름은 `const [something, setSomething]` 과 같이 지정하는 것이 일반적입니다. 원하는 대로 이름을 지을 수 있지만, 규칙을 정하면 프로젝트 전반에서 이해하기 쉬워집니다.

***설정자 함수?**  -setter 
-   접근자(accessor) : 멤버 변수 값을 반환하는 멤버 함수를 지칭한다.
-   설정자(mutator) : 멤버 변수 값을 변경하는 멤버 함수를 지칭한다.
※ 일반적으로 getter(get) / setter(set) 이라고 불린다.



### ****React는 어떤 state를 반환할지 어떻게 알 수 있을까요?****

`useState`호출이 _어떤_ state 변수를 참조하는지에 대한 정보를 받지 못한다는 것을 눈치채셨을 것입니다. `useState`에 전달되는 “식별자”가 없는데 어떤 state 변수를 반환할지 어떻게 알 수 있을까요? 함수를 파싱하는 것과 같은 마법에 의존할까요? 대답은 ‘아니오’입니다.

내부적으로 React는 모든 컴포넌트에 대해 한 쌍의 state 배열을 가집니다. 또한 렌더링 전에 `0` 으로 설정된 현재 쌍 인덱스를 유지합니다. `useState` 를 호출할 때마다 React는 다음 state 쌍을 제공하고 인덱스를 증가시킵니다. 이 메커니즘에 대한 자세한 내용은 [React Hook: 마법이 아니라 배열일 뿐입니다](https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e) 에서 확인할 수 있습니다.



### React가 snapshot을 찍는 법

**Q1**

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}
```

**state를 설정하면 다음 렌더링에 대해서만 변경됩니다.**첫 번째 렌더링에서 `number`는 `0` 이었습니다. 따라서 해당 렌더링의 `onClick`핸들러에서 `setNumber(number + 1)` 가 호출된 후에도 `number`의 값은 여전히 `0`입니다.

`setNumber(number + 1)`를 세 번 호출했지만, 이 렌더링에서 이벤트 핸들러의 `number`는 항상 `0`이므로 state를 `1`로 세 번 설정했습니다. 이것이 이벤트 핸들러가 완료된 후 React가 컴포넌트안의 `number`를 `3`이 아닌 `1`로 다시 렌더링하는 이유입니다.

**Q2**

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        setTimeout(() => {
          alert(number);
        }, 3000);
      }}>+5</button>
    </>
  )
}
```

React에 저장된 state는 알림이 실행될 때 변경되었을 수 있지만, 사용자가 상호작용한 시점에 state 스냅샷을 사용하는 건 이미 예약되어 있던 것입니다.

**state 변수의 값은** 이벤트 핸들러의 코드가 비동기적이더라도 **렌더링 내에서 절대 변경되지 않습니다.**해당 렌더링의 `onClick`내에서, `setNumber(number + 5)`가 호출된 후에도 `number`의 값은 계속 `0` 입니다. 이 값은 컴포넌트를 호출해 React가 UI의 “스냅샷을 찍을” 때 “고정”된 값입니다.


# useState

`[]`구문을 [배열 구조분해](https://ko.javascript.info/destructuring-assignment)라고 하며, 배열에서 값을 읽을 수 있습니다.

`useState`가 반환하는 배열에는 항상 정확히 두 개의 항목이 있습니다.

`[useState](<https://react-ko.vercel.app/reference/react/useState>)`를 호출하는 것은, React에게 이 컴포넌트가 무언가를 기억하기를 원한다고 말하는 것입니다.

### Hook

Hook은 **React가 [렌더링](https://react-ko.vercel.app/learn/render-and-commit#step-1-trigger-a-render) 중일 때만 사용할 수 있는** 특별한 함수입니다 (다음 페이지에서 더 자세히 설명하겠습니다). 이를 통해 다양한 React 기능을 “연결”할 수 있습니다.

! **훅(`use`로 시작하는 함수)은 “컴포넌트의 최상위 레벨” (최상위 컴포넌트 아님) 또는 [커스텀 훅](https://react-ko.vercel.app/learn/reusing-logic-with-custom-hooks)에서만 호출할 수 있습니다.** 조건, 루프 또는 기타 중첩된 함수 내부에서는 훅을 호출할 수 없습니다. 훅은 함수이지만 컴포넌트의 필요에 대한 무조건적인 선언으로 생각하면 도움이 됩니다. 파일 상단에서 모듈을 “import”하는 것과 유사하게 컴포넌트 상단에서 React 기능을 “사용”합니다.

이 쌍의 이름은 `const [something, setSomething]` 과 같이 지정하는 것이 일반적입니다. 원하는 대로 이름을 지을 수 있지만, 규칙을 정하면 프로젝트 전반에서 이해하기 쉬워집니다.

-   **설정자 함수?** -setter
-   접근자(accessor) : 멤버 변수 값을 반환하는 멤버 함수를 지칭한다.
-   설정자(mutator) : 멤버 변수 값을 변경하는 멤버 함수를 지칭한다. ※ 일반적으로 getter(get) / setter(set) 이라고 불린다.

### **React는 어떤 state를 반환할지 어떻게 알 수 있을까요?**

`useState`호출이 _어떤_ state 변수를 참조하는지에 대한 정보를 받지 못한다는 것을 눈치채셨을 것입니다. `useState`에 전달되는 “식별자”가 없는데 어떤 state 변수를 반환할지 어떻게 알 수 있을까요? 함수를 파싱하는 것과 같은 마법에 의존할까요? 대답은 ‘아니오’입니다.

내부적으로 React는 모든 컴포넌트에 대해 한 쌍의 state 배열을 가집니다. 또한 렌더링 전에 `0` 으로 설정된 현재 쌍 인덱스를 유지합니다. `useState` 를 호출할 때마다 React는 다음 state 쌍을 제공하고 인덱스를 증가시킵니다. 이 메커니즘에 대한 자세한 내용은 [React Hook: 마법이 아니라 배열일 뿐입니다](https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e) 에서 확인할 수 있습니다.

### React가 snapshot을 찍는 법

**Q1**

```
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}

```

-   *state를 설정하면 다음 렌더링에 대해서만 변경됩니다.**첫 번째 렌더링에서 `number`는 `0` 이었습니다. 따라서 해당 렌더링의 `onClick`핸들러에서 `setNumber(number + 1)` 가 호출된 후에도 `number`의 값은 여전히 `0`입니다.

`setNumber(number + 1)`를 세 번 호출했지만, 이 렌더링에서 이벤트 핸들러의 `number`는 항상 `0`이므로 state를 `1`로 세 번 설정했습니다. 이것이 이벤트 핸들러가 완료된 후 React가 컴포넌트안의 `number`를 `3`이 아닌 `1`로 다시 렌더링하는 이유입니다.

**Q2**

```
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        setTimeout(() => {
          alert(number);
        }, 3000);
      }}>+5</button>
    </>
  )
}

```

React에 저장된 state는 알림이 실행될 때 변경되었을 수 있지만, 사용자가 상호작용한 시점에 state 스냅샷을 사용하는 건 이미 예약되어 있던 것입니다.

**state 변수의 값은** 이벤트 핸들러의 코드가 비동기적이더라도 **렌더링 내에서 절대 변경되지 않습니다.**해당 렌더링의 `onClick`내에서, `setNumber(number + 5)`가 호출된 후에도 `number`의 값은 계속 `0` 입니다. 이 값은 컴포넌트를 호출해 React가 UI의 “스냅샷을 찍을” 때 “고정”된 값입니다.