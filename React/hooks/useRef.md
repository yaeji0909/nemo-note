

## useRef

**ref를 과도하게 사용하지 마세요.** ref는 props로 표현할 수 없는 필수적인 행동에만 사용해야 합니다. 예를 들어 특정 노드로 스크롤하기, 노드에 초점 맞추기, 애니메이션 촉발하기, 텍스트 선택하기 등이 있습니다.

prop으로 표현할 수 있는 것은 ref를 사용하지 마세요. 예를 들어 `Modal` 컴포넌트에서 `{open, close}`와 같은 imperative handle을 노출하는 대신 `<Modal isOpen={isOpen} />`과 같은 `isOpen` prop을 사용하는 것이 더 좋습니다. [Effect](https://react-ko.vercel.app/learn/synchronizing-with-effects)를 사용하면 prop을 통해 명령형 동작(imperative behavior)을 노출할 수 있습니다.






