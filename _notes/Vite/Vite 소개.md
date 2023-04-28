### Vite

https://vitejs-kr.github.io/

### pre-bundling(only dev-server)
https://bundlers.tooling.report/
Vite는 **ESM 기반의 번들링**과, **사전 번들링**을 통해 개발자들에게 향상된 개발 경험을 제공합니다. Webpack은 개발 서버 실행시 전체 파일을 처음부터 번들링하는 반면, Vite는 설치와 동시에 수정이 잘 일어나지 않는 의존성 패키지들을 ESBuild를 사용하여 사전 번들링해두고, 수정이 잦은 내부 소스 코드는 브라우저에서 esm을 이용하여 번들러 역할을 수행하게하여 최적화합니다.

#### 지루할 정도로 길었던 서버 구동[​](https://vitejs-kr.github.io/guide/why.html#slow-server-start)

콜드-스타트* 방식으로 개발 서버를 구동할 때, 번들러 기반의 도구의 경우 애플리케이션 내 모든 소스 코드에 대해 크롤링 및 빌드 작업을 마쳐야지만이 실제 페이지를 제공할 수 있습니다. (* 콜드-스타트: 최초로 실행되어 이전에 캐싱한 데이터가 없는 경우를 의미)

vite는 이 문제를 **dependencies** 그리고 **source code** 두 가지 카테고리로 나누어 개발 서버를 시작하도록 함으로써 해결했죠.

-   **Dependencies**: 개발 시 그 내용이 바뀌지 않을 일반적인(Plain) JavaScript 소스 코드입니다. 기존 번들러로는 컴포넌트 라이브러리와 같이 몇 백 개의 JavaScript 모듈을 갖고 있는 매우 큰 디펜던시에 대한 번들링 과정이 매우 비효율적이었고 많은 시간을 필요로 했습니다.
    
    Vite의 [사전 번들링](https://vitejs-kr.github.io/guide/dep-pre-bundling.html) 기능은 [Esbuild](https://esbuild.github.io/)를 사용하고 있습니다. Go로 작성된 Esbuild는 Webpack, Parcel과 같은 기존의 번들러 대비 10-100배 빠른 번들링 속도를 보였죠.
    
-   **Source code**: JSX, CSS 또는 Vue/Svelte 컴포넌트와 같이 컴파일링이 필요하고, 수정 또한 매우 잦은 Non-plain JavaScript 소스 코드는 어떻게 할까요? (물론 이들 역시 특정 시점에서 모두 불러올 필요는 없습니다.)
    
    vite는 [Native ESM](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)을 이용해 소스 코드를 제공하도록 하고 있습니다. 다시말해, 브라우저가 곧 번들러라는 말이죠. vite는 그저 브라우저의 판단 아래 특정 모듈에 대한 소스 코드를 요청하면 이를 전달할 뿐입니다. 따라서 조건부 동적 import 이후의 코드는 현재 화면에서 실제로 사용이 되어야만 처리가 됩니다.


### 느렸던 소스 코드 갱신[​](https://vitejs-kr.github.io/guide/why.html#slow-updates)

기존의 번들러 기반으로 개발을 진행할 때, 소스 코드를 업데이트 하게 되면 번들링 과정을 다시 거쳐야 했었습니다. 따라서 서비스가 커질수록 소스 코드 갱신 시간 또한 선형적으로 증가할 수 밖에 없었죠.

일부 번들러의 경우 메모리 상에서 이를 진행하여 실제로 갱신에 영향을 받는 파일들만을 새로이 번들링하게끔 하였으나, 결국 이 역시 처음에는 모든 파일에 대한 번들링을 진행해야 했었죠. "모든 파일"을 번들링 하고, 이를 다시 웹 페이지에서 불러오는 것이 얼마나 비효율적인 것인지 느껴지시나요? 이러한 이슈를 우회하고자 HMR(Hot Module Replacement)* 이라는 대안이 나왔으나, 이 역시 명확한 해답은 아니었습니다. (* HMR: 앱을 종료하지 않고 갱신된 파일만을 교체하는(Replacement) 방식. 다만 마찬가지로 앱 사이즈가 커질수록 선형적으로 갱신에 필요한 시간이 증가한다.)

물론, vite는 HMR을 지원합니다. 번들러가 아닌 ESM을 이용해서 말이죠. 어떤 모듈이 수정되면 vite는 그저 수정된 모듈과 관련된 부분만을 교체할 뿐이고, 브라우저에서 해당 모듈을 요청하면 교체된 모듈을 전달할 뿐입니다. 전 과정에서 완벽하게 ESM을 이용하기에, 앱 사이즈가 커져도 HMR을 포함한 갱신 시간에는 영향을 끼치지 않습니다.

또한 vite는 HTTP 헤더를 이용해 퍼포먼스를 한 단계 높였습니다. 필요에 따라 소스 코드는 `304 Not Modified`로, 디펜던시는 `Cache-Control: max-age=31536000,immutable`을 이용해 캐시되도록 함으로써, 한 번의 요청이라도 덜 하도록 말이죠.

이렇게나 빠른 Vite를 사용하지 않을 이유가 있나요?




