# Hook
![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3e9d2f79-9b70-467e-a365-867a2402258e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220730%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220730T103745Z&X-Amz-Expires=86400&X-Amz-Signature=ffee53c96445034904e5c2562c7788c4463f5cb55fd10488576ea140672d9c4d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### Hook이란

함수 component는 클래스 component와는 다르게 코드도 간단하고, state를 정의하거나 생명주기에 맞춰 코드를 실행되도록 할 수 없어서 이러한 기능을 지원하기 위해 만들어진 함수

## Hook의 종류

### 1. useState()

State를 사용하기 위한 Hook

```jsx
const [변수명, set함수명] = useState(초기값);
```

state함수에 초기값을 넣듯 useState(초기값) 설정 후, 리턴값으로 변수명과 set함수명을 적어준다.

```jsx
import React, { useState} from "react";

function Counter(props) {
	const [count, setCount] = useState(0); // 초기값 설정

	return (
			<div>
					<p>총 {count}번 클릭했습니다.</p>
					<button onClick={() => setCount(count + 1)}> //업데이트
						클릭
					</button>
			</div>
		);
}
```

- 클래스 component와 같은 점
    
    setState 함수를 호출해서 state가 업데이트 되고, 이후 component가 재 랜더링 되는 과정과 같다.
    
- 클래스 component와 다른 점
    
    setState 함수 하나를 사용해서 모든 State값을 업데이트 했지만, hook을 이용하면 변수 각각에 대해 set함수가 따로 존재한다.
    

### 2. useEffect

- side effect를 수행하기 위한 Hook
- side effect는 서버에서 데이터를 받아오거나, 수동으로 돔을 수정하는 것들을 뜻함
- 즉, useEffect는 클래스 component의 생명주기 함수를 하나로 통합하여 제공한다.

```jsx
useEffect(이펙트 함수, 의존성 배열);
```

- 의존성 배열이란, 이 이펙트가 의존하고 있는 배열. 그래서 배열안에 있는 변수가 하나라도 바뀔 때 이펙트 함수가 실행됨
- 기본적으로 이펙트 함수는 처음으로 component가 렌더링된 이후나, 업데이트로 인한 재렌더링 후에 실행 됨

```jsx
//이펙트 함수가 mount,unmount시에 단 한번씩만 실행되게 하는 경우

useEffect(이펙트 함수, []); // 의존성 배열에 빈 배열을 넣는다.
```

- 이렇게 되면 의존성 배열은 어떠한 값에도 의존하지 않으므로 여러번 실행되지 않음

```jsx
// 컴포넌트가 업데이트될 때마다 호출시키는 경우

useEffect(이펙트 함수);  // 의존성 배열을 생략한다.
```

```jsx
import React, { useState, useEffect } from "react";

function Counter(props) {
	const [count, setCount] = useState(0);

	// componentDidMount, componentDidUpdate와 비슷하게 작동함
	useEffect(() => {
			// 브라우저 API를 사용해서 document의 title을 업데이트 한다
			document.title = 'You clicked ${count} times';
	});
	
	return (
			<div>
					<p>총 {count}번 클릭했습니다.</p>
					<button onClick={() => setCount(count +1)}>
						클릭
					</button>
			</div>
	);
}
```

- useState를 사용하여, count가 state값에 계속 업데이트 되도록함
- 의존성 배열을 생략한 useEffect을 이용해서 count가 업데이트 될 때마다 호출시켜서 문서의 title 명을 매번 바꿔준다.
- 이러한 행위가 생명주기 중 mount와 update 과정과 비슷함

### 3. useMemo()

Memoized value를 리턴하는 함수

Memoization이란 최적화를 위해 사용되는 개념, 연산량이 많은 함수를 저장해놨다가 다시 사용할 때 연산하지 않고 저장된 결과값을 리턴함

```jsx
const memoizedValue = useMemo(
	() => {
			// 연산량이 높은 작업을 수행하여 결과를 반환
			return computeExpensiveValue(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

- 의존성 배열에 들어있는 변수가 변했을 경우에 새로 create함수를 호출하여 반환하고, 그렇지 않은 경우에는 기존함수의 결과값을 그대로 반환함
- component가 다시 렌더링될 때마다 연산량이 높은 작업을 반복하는 행동을 피할 수 있음.
- useMemo()로 전달된 함수는 렌더링 일어나는 동안 실행된다. 즉, 렌더링이 일어나는 동안 실행되서는 안될 작업을 useMemo에 넣으면 안된다. ex)side effect 작업

```jsx
// 의존성 배열을 넣지 않은 경우

const memoizedValue = useMemo(
 () => computeExpensiveValue(a, b) 
);
```

- 의존성 배열을 넣지않으면 매 함수가 렌더링될 때마다 create함수가 호출됨
- 그렇기에 미리 저장하는 의미가 없어지기에 useMemo 함수를 사용하는 의미가 없어짐

```jsx
// 의존성 배열이 빈 배열인 경우

const memoizedValue = useMemo(
	() => {
			return computeExpensiveValue(a,b);
	},
	[]
);
```

- 컴포넌트가 mount 될 때만 호출되어서, mount시점 이후에는 값이 변경되지 않는다.

## DOM vs React

```jsx
//DOM의 Event
<button onclick="activate()">
		Activate
</button>

//React의 Event
<button onClick={activate}>
		Activate
</button>
```

차이점

1. React는 onclink이 카멜 표기법으로 되어있다.
2. DOM에서는 이벤트 처리할 함수를 문자로 전달하지만 
    
    React에서는 함수 그대로 전달함.
    

### 다양한 표기법들

1. 카멜(Camel)케이스
    - 첫 번째 단어는 소문자, 그 다음 단어부터는 대문자를 사용
        
        ex) getMathScore
        
2. 파스칼(Pascal) 케이스
    - 카멜 표기법과 달리, 첫 번째 단어부터 대문자
        
        ex) CommonUtil
        
3. 스네이크(snake) 케이스
    - 단어 사이에 언더바를 넣음
        
        ex) total_number
        
    

## Event Handdler

사건이 발생하면 사건을 처리하는 역할

### 사용법

```jsx
function Toggle(props) {
	const [isToggleOn, setIsTogglenOn] = useState(true);

// 방법 1. 함수 안에 함수로 정의
	function handleClick() {
		setIsToggleOn((isToggleOn) => !(sToggleOn);
	}

	const handleClick = () => {
		setIsToggleOn((isToggleOn) => !(isToggleOn);
	}

	return(
		<button onClick={handleClick}
				{isToggleOn ? "켜짐" : "꺼짐"
		</button>
	);
}
```

### 삼항 연산자

```html
조건식 ? 반환값1 : 반환값2
```

조건식이 true면 반환값 1을 반환, 조건식이 false면 반환값 2을 반환