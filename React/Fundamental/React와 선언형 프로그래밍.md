#2023/04
React에서는 직접 UI를 조작하지 않습니다. 즉, 컴포넌트를 직접 활성화하거나 비활성화 하지도, 보여주거나 숨기지도 않습니다. 대신 **표시할 내용을 선언하면** React가 UI를 업데이트할 방법을 알아냅니다. 

1.  컴포넌트의 다양한 시각적 상태를 **식별**합니다.
2.  상태 변화를 trigger하는 요소를 **파악**합니다.
3.  `useState`를 사용하여 메모리의 상태를 **표현**합니다.
4.  비필수적인 state 변수를 **제거**합니다.
5.  이벤트 핸들러를 **연결**하여 state를 설정합니다.


#### 명령형 프로그래밍
무엇을 **어떻게** 할 것이다.
코드로 원하는 결과물을 얻기 위한 과정에 집중한다.

```javascript
let string = 'THis is the midday show with Cheryl Waters'; 
let urlFriendly = ""; for(let i = 0 ; i < string.length ; i++)
{ 
	if(string[i] === " ")
	{ urlFriendly += "-"; }
	else{ urlFriendly += string[i]; }
	} 
console.log(urlFriendly);
```

#### 선언형 프로그래밍
**무엇**을 할 것이다.
원하는 결과물을 얻기 위한 과정 하나하나보다는 필요한 것들을 기술하는 데 중점을 둔다.

```javascript
const string = 'This is the midday show with Cheryl Waters'; 
const urlFriendly = string.replace(/ /g, '-'); 
console.log(urlFriendly);
```


명령형 프로그래밍은 string이 사이사이 공백에 하이픈을 넣기 위한 방법을 하나하나 나열하고 있다.

반면, 선언형 프로그래밍은 replace()라는 내장 메서드를 통해 공백 자리에 하이픈을 넣겠다고 한 번에 기술한다. 공백 자리에 하이픈을 넣는 방법은 replace() 라는 메소드 안에 서술되어 있기 때문에, 우리는 치환하는 구체적인 절차보다는 추상화된 메서드만을 사용하여 원하는 결과를 얻을 수 있다.

선언형 프로그래밍은, 어떤 일이 발생해야 하는지에 대해서만 알려주고 어떻게 해당 일이 발생하는지 방법에 대한 기술은 추상화 아랫단으로 감춘다.

선언형 프로그래밍에 대해 다음과 같이 정리할 수 있다.

1.  선언형 프로그래밍은 원하는 결과물에만 집중하고, 그 과정에 대해서는 추상화를 통해 깊게 알려하지 않는다.
2. 명령형의 경우 한 줄씩 읽어 나가면서 다음에 어떤 일이 발생할지 추측해야 한다. 반면에 선언형의 경우 자세한 방법을 모르더라도 코드만 보고도 어떤 일이 발생할지 예측이 빠르게 가능하다.
3. 즉, 선언적 프로그래밍이 더 읽기 쉽고 예측이 쉽다.

https://blog.mathpresso.com/declarative-react-and-inversion-of-control-7b95f3fbddf5
