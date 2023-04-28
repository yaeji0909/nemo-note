***watchEffect()의 경우에는 Composition API에서만 사용이 가능하다.**

## watch, watchEffect의 차이점

-   watch : 특정 데이터가 변경되었을 때 실행, 새로운 데이터와 이전의 데이터를 가져옴 (lazy)
-   watchEffect : 의존성이 있는 데이터에 대해서 즉각적으로 실행 (immediately)

## watch

-   특정 반응 속성을 보고 싶거나, old value를 보고 싶을 떄 활용
-   함수 또는 하나 이상의 반응 속성을 감시할 수 있음
-   reactive 종속성이 변경될 때만 실행됨

```
// Triggers whenever `user` is updated.
watch(user, () => doSomething({ user: user.value, profile: profile.value }))
```

## watchEffect

-   watch의 단순화 버전
-   함수만을 값으로 가진다.
-   정의되거나 reactived의 종속성이 변경될 때 즉시 동작한다.
-   여러 반응 속성을 감시하고 싶고, 이전 값에 신경을 쓰지 않을 때 사용됨
-   computed() hook 처럼 동작한다.
-   함수 내부에 있는 여러 반응값을 관찰해야 할 때마다 사용하고 그 중 하나가 업데이트 될 때 마다 반응

```
// Triggers whenever `user` *or* `profile` is updated.
watchEffect(() => doSomething({ user: user.value, profile: profile.value }))
```

우선 가장 큰 차이점은 **watchEffect는 대상을 지정하는 부분이 존재하지 않는다.**

그 이유는 effect를 지정하는 함수에서 선언된 대상들을 감시하기 때문이다.

쉽게 말하면 computed()처럼 **안에 사용된 대상들의 변경을 감지하여 실행**된다는 이야기다.

그리고 watch와 다르게 **watchEffect는 "즉시" 실행**된다는 점이다.

### 메모
