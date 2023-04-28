
-   컴포넌트는 순수해야 합니다:
    -   **자신의 일에만 신경씁니다.** 렌더링 전에 존재했던 객체나 변수를 변경하지 않아야 합니다.
    -   **동일한 입력, 동일한 출력.** 동일한 입력이 주어지면 컴포넌트는 항상 동일한 JSX를 반환해야 합니다.
-   렌더링은 언제든지 발생할 수 있으므로, 컴포넌트는 서로의 렌더링 순서에 의존해서는 안 됩니다.
-   컴포넌트가 렌더링에 사용하는 어떠한 입력값도 변이해서는 안 됩니다. 여기에는 props, state 및 context가 포함됩니다. 화면을 업데이트하려면 기존 객체를 변이하는 대신 [“set” state](https://react-ko.vercel.app/learn/state-a-components-memory)를 사용하세요.
-   컴포넌트의 로직을 반환하는 JSX 안에 표현하기 위해 노력하세요. “무언가를 변경”해야 할 때는 보통 이벤트 핸들러에서 이 작업을 수행하고자 할 것입니다. 최후의 수단으로 `useEffect`를 사용할 수도 있습니다.
-   순수 함수를 작성하는 데는 약간의 연습이 필요하지만, React 패러다임의 힘을 발휘할 수 있습니다.



### **React와 연관지어 생각하기**

```
<h1>...</h1>
```
 태그를 생성하고 text를 적용하는 함수를 만들어봅니다.

**JQuery를 사용하여 DOM을 변경하는 경우**

```
function setHeader(text) {
   let h1 = document.createElement('h1');
   h1.innerText = text;
   document.body.appendChild(h1);
}

setHeader('Header Setting');
```

setHeader 함수는 순수 함수가 아닙니다.

1. 함수나 값을 반환하지 않습니다.

2. 함수 외부의 DOM을 변경합니다.

**React에서 DOM을 변경하는 경우**

```
const setHeader = (props) => <h1>{props.title}</h1>
```

React에서 작성한 setHeader 함수는 순수 함수입니다.

JQuery를 사용하여 작성한 setHeader 함수는 함수나 값을 반환하지 않았지만, React에서 작성한 setHeader 함수는

<h1>{props.title}</h1>을 반환합니다.

JQuery를 사용하여 작성된 setHeader함수는 DOM에 직접 접근을 하였지만, React에서 DOM을 변경하는 일은 setHeader함수와는 무관하게 React 라이브러리가 책임지고 담당합니다.

---

### **순수 함수가 중요한 이유**

**1. 테스트하기 쉽습니다.**

→ 전달되는 인자에 대해서 결괏값을 예상하면 되므로 테스트가 쉽습니다.

**2. 코드 관리가 쉬워지며, 리팩토링 할 수 있습니다.**

→ 순수하지 않은 함수는 분석 및 수정이 어려우며 예기치 않은 동작을 유발합니다. 순수하지 않은 함수가 스파게티 코드로 작성되었을 경우 리팩토링이 불가능한 상황이 올 수도 있습니다.