user agent는 HTTP 요청을 보내는 디바이스와 브라우저 등 사용자 소프트웨어의 식별 정보를 담고 있는 request header의 한 종류이다. 임의로 수정될 수 없는 값이고, 보통 HTTP 요청 에러가 발생했을 때 요청을 보낸 사용자 환경을 알아보기 위해 사용한다.

기본 형태는 아래와 같다.

```null
User-Agent: <product> / <product-version> <comment>
```

보통 `<comment>` 부분이 길어지는데, 실제 예시를 가져와 보면 이렇게 나온다.

```null
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36
```

user agent에서 comment를 구성하는 형식에 대해 MDN에서는 브라우저를 기준으로 형식을 소개하고 있다. 보통 **Mozilla 정보/버전 + 운영체제 정보 + 렌더링 엔진 정보 + 브라우저** 형태로 노출되는 것 같다.  
미리 알아두면 나중에 사용자 정보를 확인할 때 편하니까 참고해놓자.  
  

**1. Firefox**

```null
Mozilla/5.0 (platform; rv:geckoversion) Gecko/geckotrail Firefox/firefoxversion
```

Mozilla/5.0 : 접속한 브라우저가 Mozilla와 호환된다는 의미. 거의 모든 브라우저가 이렇게 표시된다.  
platform : 브라우저가 실행되는 운영체제 환경(window, mac, linux, android 등), 그리고 모바일인지 여부.  
rv: geckoversion : Gecko 버전 (파이어폭스의 렌더링 엔진이다)  
Gecko/geckotrail : 브라우저가 Gecko 기반인지 여부. 데스크탑일 경우 geckotrail은 20100101이라는 스트링값으로 고정된다.  
Firefox/firefoxversion : 브라우저가 파이어폭스라는 의미, 그리고 파이어폭스의 버전.  
  

**2. Chrome**

```null
Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36
```

크롬은 파이어폭스와 비슷한 형식이다. 위 UA는 리눅스 환경이라는 의미이고, 뒤에 Chrome이라는 이름이 붙고 버전이 명시된다.  
모바일에서는 조금 다른데, ios에서는 CriOS가 크롬을 뜻한다.

```null
Mozilla/5.0 (iPhone; CPU iPhone OS 12_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) CriOS/71.0.3578.89 Mobile/15E148 Safari/605.1
```

안드로이드 삼성 브라우저에서 접속한 경우에도 크로미움 기반이어서, 아래와 같이 나온다. Chrome과 SamsungBrowser가 같이 노출된다.

```null
Mozilla/5.0 (Linux; Android 8.0.0; SAMSUNG-SM-G950N/KSU3CRJ1 Build/R16NW) AppleWebKit/537.36 (KHTML, like Gecko) SamsungBrowser/8.2 Chrome/63.0.3239.111 Mobile Safari/537.36
```

Microsoft Edge도 크로미움 기반이라 chrome과 Edge가 같이 노출된다.

```null
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.140 Safari/537.36 Edge/17.17134
```

정리하면 chrome이 노출되는 경우는 크로미움 기반인 삼성브라우저와 Edge가 포함된다.  
  

**3. Safari**

```null
Mozilla/5.0 (iPhone; CPU iPhone OS 13_5_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.1.1 Mobile/15E148 Safari/604.1
```

사파리는 크롬과 아주 비슷하다. 다만 마지막 브라우저 정보에 Safari가 노출되고, 모바일 접속일 경우 Mobile이라고 같이 뜬다.  
  

**4. IE**

```null
IE11 : Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; rv:11.0) like Gecko
```

IE로 접속시에는 위처럼 trident 렌더링 엔진이 명시된다.  
  

개발을 하다보면 UA 정보를 기반으로 특정 브라우저에서만 다르게 동작하도록 하는 판별 로직을 넣거나 유틸 함수를 짜야 하는 경우가 종종 있다. 정확한 UA 구성 형식을 몰랐어서 매번 찾아봤는데, 뜯어보니까 어려운건 아니었다!