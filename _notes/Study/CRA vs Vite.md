### CRA
검색해보면 create-react-app(cra)에 대한 내용이 대다수이다.  
cra는 정말 다양한 개발환경을 만족하기 위해 포함되어있는 모듈들이 정말 많다.  
또한 쉽게 공부 또는 개발을 돕기 위한 기본 설정들이 잘 되어 있다.  
하지만, 그만큼 설정을 변경하기가 힘들고 무겁다는 것이 단점이다.

cra로 생성한 프로젝트를 수정하다보면 eject를 해야할 상황이 온다.  
(webpack 설정을 바꾼다던지..)  
eject를 하면 숨겨져있던 파일들이 많이 나오게 되는데 위에서 설명한 것과 같이  
다양한 환경을 대응하기 위해서 설정되어있는 코드들이 정말 많다.  
이해하기 힘든 부분도 있고,  
필요한 부분을 찾아서 수정하는것은 익숙하지 않으면 쉬운일은 아니다.

### create-react-app + react-app-rewired

eject의 단점은,  
다시 돌아갈 수 없고 앞으로의 설정들을 스스로 해결해나가야한다는 단점아닌 단점이 있다.  
이런 문제를 해결하기 위해 react-app-rewired라는 모듈이 있는데,  
eject하지 않고 설정파일들을 덮어씌워준다.  
변경하고싶은 부분만 rewired를 통해 변경할 수도 있다.

https://joonfluence.tistory.com/658
https://think0wise.tistory.com/54


______________________________________________________________________

https://velog.io/@wynter_j/Bundler-JavaScript-%EB%B2%88%EB%93%A4%EB%9F%AC-%EA%B7%B8%EB%A6%AC%EA%B3%A0-Webpack-Parcel-Rollup-Vite...-2












https://velog.io/@wynter_j/Bundler-JavaScript-%EB%B2%88%EB%93%A4%EB%9F%AC-%EA%B7%B8%EB%A6%AC%EA%B3%A0-Webpack-Parcel-Rollup-Vite...-2


### Bundler

### [**Rollup**](https://11001.tistory.com/213#Rollup)
-   ES6 모듈 형식으로 빌드 결과물을 출력할 수 있습니다.
-   Code Splitting 에 강점이 있습니다. 
-   트리 쉐이킹에 특화되어 있습니다. 특히 진입점이 여러 개 있을때 두드러집니다. 진입점이 다르기 때문에 중복해서 번들될 수 있는 공통된 부분을 알아내고 독립된 모듈로 분리할 수 있습니다.
-   라이브러리 개발에 적합한 번들러 입니다.

### [**Parcel**](https://11001.tistory.com/213#Parcel)
-   JS 엔트리포인트를 지정하는게 아니라 어플리케이션 진입을 위한 HTML 파일 자체를 읽기 때문에 별도 설정 없이도 동작합니다. (Zero Config 번들러 입니다.)
-   HTML 파일을 순서대로 읽어가면서 JS, CSS, Asset을 직접 참조합니다.
-   트렌스파일러 설정이 간편합니다. JS 파일만 읽을 수 있는 일반적인 번들러와 달리 CSS, Asset 을 직접 참조하기 때문에 트렌스파일이 필요한 파일 유형을 일일이 설정해줄 필요 없이 **.bablerc, .postcssrc, .posthtml** 같은 설정 파일들을 루트 디렉토리에 만들기만 하면 자동으로 파일을 읽어와서 세팅 해줍니다.
-   모든 모듈에서 Babel을 사용하여 최신 JS를 브라우저에 지원하는 형식으로 컴파일합니다.
-   트리 쉐이킹에 강점이 있습니다. ES6, CommonJS 모듈 모두에 대해 트리 쉐이킹 지원합니다.

문제점
-   웹팩, 롤업에 비해 좁은 생태계, 안정성이 떨어집니다.
-   일반적인 케이스만 다루고 커스텀한 설정이 필요하면 결국 설정 파일을 다시 작성해야 합니다.


### SWC
https://fe-developers.kakaoent.com/2022/220217-learn-babel-terser-swc/
https://engineering.ab180.co/stories/webpack-to-vite


### Vite
https://vitejs-kr.github.io/guide/why.html