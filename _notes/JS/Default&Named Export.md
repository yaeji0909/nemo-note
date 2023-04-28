보통 JavaScript에서는 default export와 named export라는 두 가지 방법으로 값을 export 합니다. 지금까지의 예제에서는 default export만 사용했지만 두 방법 다 한 파일에서 사용할 수도 있습니다. **다만 한 파일에서는 하나의 _default_ export만 존재할 수 있고 _named_ export는 여러 개가 존재할 수 있습니다.**

Export 하는 방식에 따라 import 하는 방식이 정해집니다. Default export로 한 값을 named import로 가져오려고 하려면 에러가 발생합니다. 아래 표에는 각각의 경우의 문법이 정리되어 있습니다.

![[Pasted image 20230421092328.png]]

![[Pasted image 20230421092403.png]]

_default_ import를 사용하는 경우 원한다면 `import` 단어 후에 다른 이름으로 값을 가져올 수 있습니다. 예를 들어 `import Banana from './button.js'` 라고 쓰더라도 같은 default export 값을 가져오게 됩니다. 반대로 named import를 사용할 때는 양쪽 파일에서 사용하고자 하는 값의 이름이 같아야 해서 _named_ import라고 불립니다.

**보편적으로 한 파일에서 하나의 컴포넌트만 export 할 때 default export 방식을 사용하고 여러 컴포넌트를 export 할 경우엔 named export 방식을 사용합니다.** 어떤 방식을 사용하든 컴포넌트와 파일의 이름을 의미 있게 명명하는 것은 중요합니다. `export default () => {}` 처럼 이름 없는 컴포넌트는 나중에 디버깅하기 어렵기 때문에 권장하지 않습니다.

default export와 named export 사이의 잠재적인 혼동을 줄이기 위해 일부 팀에서는 한 가지 스타일(default 또는 named)만 고수하거나, 단일 파일 내에서 혼합하지 않도록 선택합니다. 자신에게 가장 적합한 방식을 선택하세요!