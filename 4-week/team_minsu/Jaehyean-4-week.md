# React 4주차

## hooks

리액트의 컴포넌트에는 클래스형 컴포넌트와 함수형 컴포넌트 프로그램이 있다. 클래스 컴포넌트의 사용은 개발시 화면 상태 변화, 생명주기의 제어 처리와 같은 부분에서 사용된다. 

![훅.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a0a22cf2-756f-4f19-933c-fb36ae58fac6/%ED%9B%85.png)

함수형 컴포넌트에서 처리가 불가능한 부분을 처리하기 위해 state나 생명주기 메소드 제공 같은 기능이 추가된 클래스형 컴포넌트이지만 여러 단점이 있고 무엇보다 코드를 짜는데 있어 더 복잡하고 리액트에서의 this 키워드는 대부분의 다른 언어와는 다르게 작동하여 사용자와 기계 모두에게 혼란을 주었다. 이 외에도 생성자와 바인딩등 함수형에 비하여 클래스형은 따라야 할 규칙이 많았다. 이에 함수형 컴포넌트에서도 state의 사용과 lifecycle 기능 구현을 가능하게 해주는 hook이 등장한 것이다..!

리액트에서의 hooks은 마치 갈고리처럼 리액트의 state와 생명주기와 관련된 함수를 원하는 시점에 함수를 지정해 사용하게 하는 것이다. 그러니까 정확히 하자면 끌려온 그 함수를 hooks 이라 부르는 것이다. 

### hooks의 종류와 이용 예시

hooks는 use~의 형태이다. 수행하고자 하는 기능에 따라 이름지어진 것이다.

- useState()
    
    함수 컴포넌트에선 기본적으로 state를 제공하지 않기 때문에 state를 사용하고자 한다면 useState()의 사용이 필요하다. 
    
    ```jsx
    const [변수명, set함수명] = useState(state의 초기값);
    
    setState(넣고싶은 값:식 포함);
    ```
    

다음의 코드를 실행하면 useState는 배열을 return한다. 이 배열에는 두가지 항목이 들어있는데  각각state로 선언된 변수와 해당 state의 set함수를 담고 있다. 이때 이 변수들은 함수 동일성이 안전하여 리렌더링 시에도 변경되지 않는다. 

```jsx
import React, {useState} from "react";

function Counter(props) {
	const [count, setCount] = useState(0);

		return (
			<div>
				<p>총 {count}번 클릭했습니다</p>
				<button onClick={() => setCount(count+1)}>
						클릭
				</button>
				</div>
		);
}
```

다음의 코드를 함께 보면 useState의 사용을 시작으로 초기값을 0을 가지고 setCount라는 함수를 가진 state의 사용이 시작되고 버튼이 클릭되는 이벤트가 발생할 때마다 setCount함수를 통해 state로 선언된 변수인 count의 값이 늘어나는것을 볼 수 있다. count값이 변경될 때마다 화면은 다시 렌더링 되고 이는 결국 클래스 함수에서 setState를 호출하여 state값을 변경하여 컴포넌트를 업데이트하고 다시 렌더링 하는 과정과 같다. 

다만 클래스 컴포넌트에서는 setState()를 이용하면 state값을 모두 바꿀 수 있었지만 hooks의 사용시에는 반환된 배열 안에 있는 set함수만을 통해서 그 값을 변경할 수 있다는 차이가 있다.

- useEffect()
    
    useEffect는 서버에서 데이터를 받아오거나 수동으로 dom을 변경하는 등의 다른 컴포넌트에 영향을 줄 수 있는 effect들을 실행할 수 있게 해주는 hooks이다. 
    
    ```jsx
    useEffect(이펙트 함수, 의존성 배열);
    ```
    
    이펙트 함수는 (의존성) 배열 안의 요소중 하나라도 값이 변한다면 호출된다. 이 말인 즉 클래스 컴포넌트에서로 따지자면 마운팅과 업데이트에 해당하는 상황에서 발동하는 함수라는 뜻이다. 이를 통해서 life cycle 메서드가 없는 함수형 컴포넌트에서도 생명 주기 함수를 다룰 수 있는 것이다.
    
    ```jsx
    useEffect(이벤트 함수,[]);
    ```
    
    참고로 다음과 같이 사용하면 배열 안에는 어떠한 값도 들어있지 않기 때문에 받을 영향이 없다. 따라서 마운팅과 언마운팅시에 한번씩만 이벤트 함수를 호출하는 결과가 나온다. 아예 배열을 생략할 수도 있는데 이 경우에는 컴포넌트 내부에 변화가 일어날 때마다 이벤트 함수가 호출된다.
    
    - effect 타이밍
    
    ~~클래스 컴포넌트의 componentDidMount, componentDidUpdate의 기능을 이용하고자 사용한 useEffect였지만 둘은 서로 다른점이 있는데 바로 타이밍이다. useEffect는 화면의 레이아웃의 구성과 그리기를 마치고 난 후에서야 함수를 전달한다. 이말은 만약 useEffect에서 전달해주는 함수가 실행될 때 이에 따라서 화면에 새로운 요소를 추가하는 기능의 코드가 있다면 한번의 렌더링이 아니라 ???~~ 
    
    **잠깐!**
    
    <aside>
    💡 Memoization, memoized value란?
    
    </aside>
    
    연산량이 큰 함수의 결과값을 저장해 두었다가 동일한 입력값으로 함수를 호출받으면 저장해두었던 값을 사용하는 방법으로, 이를 통해서 중복적인 계산을 줄이며 계산시간을 단축하고 컴퓨터의 자원을 적게 사용하게 된다. 이때 저장된 이 연산값을 바로 memoized value라고 한다.
    
    - useMemo()
    
    ```jsx
    const memoizedValue = useMumo(
    	() => {
    		return computeExpensiveValue(의존성 변수1, 의존성 변수2);
    	},
    	[의존성 변수1, 의존성 변수2]// <- useMemo에서의 의존성 배열
    );
    //???: 예시에서 굳이 의존성 변수가 두개인 이유는 예시에서 
    //     저장된 값이 받는 인자가 2개라서인가?
    //???: 아마도..?
    ```
    
    의존성 변수의 값이 변하였을 때는 당연히 저장되어 있는 값을 그대로 반환하는 것이 의미가 없기 때문에 해당 인자에 맞는 값에 대한 값을 새로 받아서 저장한다.
    
    useMemo의 사용시 **주의사항**이 있는데 useMemo에서 서버에서 데이터를 받아오거나 수동으로 dom을 변경하는 등의 다른 컴포넌트에 영향을 줄 수 있는 effect들을 다루는 것은 다룰수 없다. 이는 useMemo로 전달되는 함수가 렌더링 중에 실행되기 때문인데, 렌더링 중에 실행되는 과정에서 화면의 구성을 나타내야 하는 DOM에 영향을 준다면 당연히 문제가 생기기 때문이다.
    
    - useCallback()
    
    useCallback은 useMemo와 마찬가지로 연산량이 많은 값을 저장해두었다가 재사용, 그리고 의존성 배열이 변경된다면 그에 따라 저장하고 있는 값이 다시 정의되어 저장되는 점 역시 같다. 차이점은 useMemo에서는 저장하고 있었던 것이 결과 값에 해당하는 어떤 ‘값’이었지만 useCallback에서는 ‘함수식’이라는 차이가 있다.
    
    ```jsx
    const memoizedCallback = useCallback(//함수 이름:doSomething
    	() => {
    		doSomething(의존성 변수, 의존성 변수2);//의존성 배열에서의 값을 인자로 
    	},//받아 처리하는 함수
    	[의존성 변수1, 의존성 변수2]//의존성 배열
    );
    ```
    
    당연하게도 의존성 배열의 값이 변경된다면 저장되어 있는 함수 역시 변경된다.
    
    - useRef()
        
        useRef는 current라는 속성을 가진 객체를 반환한다. “아니, 클래스로 작성하기 싫어서 함수형 컴포넌트에서 hooks이라는 걸 사용하는 건데 왜 또 객체야” 라는 생각이 들지만 useRef를 이용해야 컴포넌트가 다시 렌더링되어도 값을 잃어버리지 않는 일종의 박스를 만들 수 있다고 한다. 대신, useRef의 값에 따라서 컴포넌트가 렌더링 되는것 역시 아니다. 인과관계가 어떻게 될 지는 모르지만 값이 변경되었다고 해서 useState나 useCallback처럼 변화를 알리거나 하지 않는다.
        
        state의 변화→ 렌더링→ 컴포넌트 내부변수들의 초기화& Ref 값 유지됨
        
        Ref의 변화→ 렌더링 없음 → 변수들의 값 유지
        
        ```jsx
        함수 컴포넌트 내부에 다음과 같이 작성
        const countRef = useRef(current값)
        
        이후 countRef.current 를 통해 값의 변경과 호출을 진행
        ```
        
    
    [Hooks API Reference - React](https://ko.reactjs.org/docs/hooks-reference.html)
    

### 모든 hooks의 규칙

 hook은 오직 리액트 내에서 함수의 최상위에서 호출되어야 한다. 반복, 조건, 중첩된 함수 내에서 훅의 사용이 불가능하다는 말이다. 이는 하나의 컴포넌트에서 같은 훅들이 여러번 사용될 때 리액트가 이 훅들을 호출되는 순서대로 저장해놓고, 렌더링 시에 순서대로 훅을 호출하기 때문이다. 만약 조건에 따라 훅의 순서, 사용 횟수가 달라진다면  리액트가 저장했던 것과 다르게 실행될 것이고 문제가 생긴다. 

[Hooks 사용 규칙](https://kwangsunny.tistory.com/10)

### 커스텀 hooks

여느 큰 프로그램이 그렇듯이 리액트로 아주 긴 코드의 컴포넌트를 작성하게 된다면 여러개의 훅을 사용하게 될 것이다. 당연하게도 그중에서는 중복되는 코드가 여러 컴포넌트에 걸쳐서 존재할 것이다. 커스텀 훅은 중복된는 코드를 하나의 로직으로 묶어 필요할 때 import하여 사용할 수 있게 해주는 기능이다. 커스텀 훅 역시 함수와 같이 작성하는데 이름을 짓는 방식은 자유롭되 **이름앞에 use+대문자의 규칙은 여전히 같다.**

```jsx
import React, {usestate} from 'react';

const useFetch = (initialUrl: srtring) =>{
	const [url, seturl] = useState(initialUrl);

	return[url, seturl];
};

export default useFetch;
```

위의 예시에서는 하나의 훅만을 사용했지만 여러개의 묶음으로도 응용할 수 있다.

## Event

컴퓨터 프로그램에서 사용자에 의해 일어나는 사건이며 이를 처리하는 것이 바로 event handling이다.  자바 스크립트 dom에서의 이벤트 처리 방식과 리액트에서의 이벤트 처리는 사실 크게 다르지는 않다. 어쨋거나 두 방식 모두 js문법이 이용되긴 하기 때문이다. 두 코드를 보며 직접 비교해보자.

```jsx
DOM의 event
<button onclick="activate()">
	Activate
</button>
```

```jsx
리액트의 event
<button onClick={activate}
	Active
</button>
```

두 코드 모두  버튼에 클릭 이벤트가 발생하였을 때 activate라는 이름의 함수가 발동되게 짜여진 코드이다.  이 둘의 차이는 onClick과 onclick처럼 첫글자의 시작은 소문자, 이후는 대문자로 시작하는 카멜 표기법을 사용해 이벤트를 정의하는지, 파스칼 표기법에 의해서 작성되는지와 함수를 문자열로만 나타내는지, 그 기능을 다 하도록 ()까지 붙이는지정도의 차이가 있다.

 

### 함수형 컴포넌트를 이용한 이벤트 헨들링

```jsx
function Toggle(props) {
	const [isToggleOn, setIsToggleOn] = useState(true);

// 방법 1. 함수안에 함수로 정의
function handleClick() {
	setIsToggleOn((isToggleOn) => !(isToggleOn);
}
//방법 2. arrow function을 사용하여 정의 
const handleClick = () => {
	setIsToggleOn((isToggleOn) => !isToggleOn);
}
return (
	<button onClick={handClick}.
		{isToggleOn ? "켜짐" : "꺼짐"}
	</button>
);
}
```

(+)

 !( 함수식 A)은 함수식 A의 논리적 의미를 반전시킨 함수이다. 

### Argument

함수에 전달할 내용, 이벤트 핸들러에 전달할 데이터를 부르는 용어로 간단하게 하면 이벤트에 대한 상호작용으로 데이터가 반환되는 과정에서 전달받는 데이터가 예시로 존재한다.

함수형 컴포넌트에서는 다음과 같이 Argument를 전달받는다.

```jsx
function MyButton(props) {
	const handleDelete = (id, event) => {//파라미터로써 id와 이벤트의 
		console.log(id, event..target);//종류를 받는다
	};
	
	return(//이에 대하여 handleDelete에다가 event와 1을 매개변수로 삼는
		<button onClick={(event) => handleDelete(1, event)}>
			삭제하기//함수를 호출하는 html요소를 반환한다.
		</button>
	);
};
```

.