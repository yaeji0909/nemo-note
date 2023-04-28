Nuxt에서 미들웨어(Middleware)는 페이지나 레이아웃이 렌더링 되기 전에 호출되는 커스텀 훅(Hook)입니다. 미들웨어의 대표적인 특징은 아래와 같습니다.
-> server middlware에 대한 설명임.


>Do not add serverMiddleware to the middleware/ directory.Middleware are bundled by webpack into your production bundle and run on beforeRouteEnter. If you add serverMiddleware to the middleware/ directory it will be wrongly picked up by Nuxt as middleware and will add wrong dependencies to your bundle or generate errors.


## nuxt 3

``` javascript
//server/middleware/log.js

export default defineEventHandler((event) => {

  event.context.auth = { user: 123 };

  console.log('server middleware');

});
```

## nuxt 2
```javascript

//server-middleware/logger.js
export default function (req, res, next) {

// req is the Node.js http request object
console.log(req.url);
console.log('server middleware');
  // res is the Node.js http response object
  // next is a function to call to invoke the next middleware

  // Don't forget to call next at the end if your middleware is not an endpoint!
next();
}
```


``` javascript
//nuxt.config.js
serverMiddleware: ['~/server-middleware/logger']
```