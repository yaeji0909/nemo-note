**```javascript
//해당 메서드 사용해서 미들웨어를 만듦.
//global routes middleware
export default defineNuxtRouteMiddleware(() => {
  console.log('a');
  // skip middleware on server
  if (process.server) return;
  // skip middleware on client side entirely
  if (process.client) return;
  // or only skip middleware on initial client load
  const nuxtApp = useNuxtApp();
  if (process.client && nuxtApp.isHydrating && nuxtApp.payload.serverRendered)
    return;
});
```

- Global route middleware, which are placed in the `middleware/` directory (with a `.global` suffix) and will be automatically run on every route change.


pages directory안에 있는 vue 파일에서 사용하는 미들웨어를 부르는게 예제로 되어있음. 
-> layout에서가 아니라 pages안에 있는 vue파일에서 미들웨어를 불러와서 사용해야
layout middlware보다 먼저 미들웨어를 실행할 수 있음.

```javascript
<script setup>
definePageMeta({
  middleware: process.client ? 'auth' : undefined,

</script>
```

![[Pasted image 20230420105551.png]]
위와 같이 global suffix를 넣으면 전역에서 사용 가능.
01,02 혹은 a,b처럼 네이밍 하면 순서대로 사용 가능.



![[Pasted image 20230420131924.png]]



![[Pasted image 20230420132055.png]]


Nuxt에서 미들웨어(Middleware)는 페이지나 레이아웃이 렌더링 되기 전에 호출되는 커스텀 훅(Hook)입니다. 미들웨어의 대표적인 특징은 아래와 같습니다.

**front2.0**
layout -> middleware를 불러오는 default mixin이 각 layout에 들어가 있어서
순서가 layout-middleware순이 되는 것.**

일부 페이지나 레이아웃에 지역적인 미들웨어를 설정할 수는 있지만
위의 경우 전역적으로 사용되는 부분이라서 전역 설정을 해주어야 함.
