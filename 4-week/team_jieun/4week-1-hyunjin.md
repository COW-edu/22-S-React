# Hooks

- 리액트 버전 16.8에서 새롭게 등장한 개념! 최근에 리액트로 개발할 때 대부분이 훅을 사용한다.
- 클래스 컴포넌트 → 생성자에서 state를 정의, setState() 함수를 통해 state 업데이트, Lifecycle methods 제공
- 함수 컴포넌트 → state 사용 불가, Lifecycle에 따른 기능 구현 불가

**클래스 컴포넌트의 기능을 함수 컴포넌트에서 동일하게 사용할 수 있게 하기 위한것이 바로 Hooks**

 - 프로그래밍에서 hook : 원래 존재하는 특정 기능에 갈고리를 거는 것 처럼 끼어들어가 같이 수행되는 것을 의미한다.

### useState()

- 가장 대표적이고 많이 사용됨
- state를 사용하기 위한 hook

예시)

```jsx
import React, {useState} from "React";

function Counter(props){
	var count =0;

	return (
		<div>
			<p>총 {count}번 클릭했습니다.</p>
			<button onClick={() => count++}>
				클릭
			</button>
		</div>
	);
}
```

→ count 값을 증가시킬 수는 있지만 재 렌더링이 되지 않아 화면에는 변한 count 값을 띄울 수 없게 된다.

- useState() 사용법

```jsx
const [변수명, set함수명] = useState(초기값);
```

```jsx
import React, {useState} from "React";

function Counter(props){
	const [count, setCount] = useState(0);

	return (
		<div>
			<p>총 {count}번 클릭했습니다.</p>
			<button onClick={() => setCount(count+1)}>
				클릭
			</button>
		</div>
	);
}
```

→ 재 렌더링 됨

### useEffect()

- Side effect를 실행할 수 있게 해주는 Hook
    - 리액트에서 Side effect : 효과, 영향(서버에서 데이터를 받아오거나, 수동으로 DOM을 변경하는 작업)
    - effect인 이유 : 이 작업이 다른 컴포넌트에 영향을 미칠 수 있으며, 렌더링 중에는 작업이 완료될 수 없기 때문에
- 생명주기 함수와 동일한 기능을 수행할 수 있다. → componentDidMount, componentDidUpdate와 비슷하게 작동

- useEffect() 사용법

```jsx
useEffect(이펙트 함수, 의존성 배열);
```

    - 의존성 배열: 해당 effect가 의존하고 있는 배열
    - 배열 안의 값 중 하나라도 바뀐다면 이펙트 함수가 실행됨

- mount, unmount 시에 단 한 번 씩만 실행되게 하는 방법 → 의존성 배열을 비워놓는다.

```jsx
useEffect(이펙트 함수, []);
```

- 의존성 배열을 생략하면 컴포넌트가 업데이트 될 때마다 호출됨

```jsx
useEffect(이펙트 함수);
```

### useMemo()

- Memoization : 연산량이 많이 드는 함수의 호출 결과를 저장해두었다가, 같은 입력값으로 함수를 호출하면 새로 함수를 호출하지 않고 이전에 저장해두었던 호출 결과를 바로 반환하는 것 → 함수 호출 결과를 받기까지의 시간이 짧아짐, 불필요한 중복 연산을 하지 않게됨 → **빠른 렌더링 속도를 얻을 수 있음**

- useMemo() 사용법

```jsx
const memoizedValue = useMemo(
	() => {
		//연산량이 높은 작업을 수행해 결과를 반환
		return computeExpensiveValue(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

  - useMemo()로 전달된 함수는 렌더링이 일어나는 동안만 실행된다 → 일반적으로 렌더링이 일어나는 동안 실행되서는 안될 작업을 useMemo()에 넣으면 안됨

  - 의존성 배열을 넣지 않을 경우, 매 렌더링마다 함수가 실행됨 → 사용하는 의미 x
  - 의존성 배열이 빈 배열일 경우, 컴포넌트 마운트 시에만 호출됨

### useCallback()

- useMemo() Hook과 유사하지만 **값이 아닌 함수를 반환한다**. → 컴포넌트가 렌더링 될 때마다 매번 함수를 정의하는 것이 아니라 의존성 변수값이 바뀐 경우에만 함수를 새로 정의해 return 해준다.
- 컴포넌트가 마운트 될 때만 함수가 정의된다.

- useCallback() 사용법

```jsx
const memoizedCallback = useCallback(
	() => {
		doSomething(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```

### useRef()

- Reference를 사용하기 위한 Hook
- 리액트에서 Reference : 특정 컴포넌트에 접근할 수 있는 객체
- reference 객체를 반환한다.
- refObject.current 에서 current → 현재 참조하고 있는 element

- useRef() 사용법

```jsx
const refContainer = useRef(초기값);
```

 - 파라미터로 초기값을 넣으면 해당 초기값으로 초기화된 레퍼런스 객체를 반환된다.
  - 만약 초기값이 null이라면, current의 값이 null인 레퍼런스 객체가 반환된다.
 - 반환된 레퍼런스 객체는 컴포넌트의 lifetime 전체에 걸쳐 유지 → **즉, 컴포넌트가 마운트 해제 전까지는 계속해서 유지된다.**

- useRef() Hook은 내부의 데이터가 변경되었을 때 별도로 알리지 않는다. 따라서 current 속성을 바꾼다고 해서 재 렌더링이 일어나지 않는다.
→ 따라서 Ref에 DOM node가 연결되거나 분리되었을 경우에 어떤 코드를 사용하고 싶다면 **Callback ref**를 사용한다.

- Callback ref

 자식 컴포넌트가 변경되었을 때 알림을 받을 수 있고, 이를 통해 다른 정보들을 업데이트 할 수 있게 된다.

## Hook의 규칙

- Hook은 무조건 **최상위 레벨**에서만 호출해야 한다.
    - 최상위 레벨: 리액트 함수 컴포넌트의 최상위 레벨을 의미 → 반복문, 조건문, 중첩된 함수 안에서 Hook을 호출하면 안된다.
    - Hook은 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 한다. ~> 다수의 useEffect, useState hook에서 컴포넌트의 state를 올바르게 관리할 수 있게 된다.
    - 잘못된 Hook 사용법
    
    ```jsx
    function MyComponent(props){
    	const [name, setName] = useState('hyunjin');
    
    	if (name !== ''){
    		useEffect(() => {
    			...
    		});
    	}
    	...
    }
    ```
    
    → 조건문의 결과에 따라 호출되는 Hook이 달라지기 때문
    

- **리액트 함수 컴포넌트에서만** Hook을 호출해야 한다.

### Custom Hook 만들기

- 여러 컴포넌트에서 반복적으로 사용되는 로직을 hook으로 만들어 재사용하기 위함!
- 이름이 use로 시작하고 내부에서 다른 hook을 호출하는 하나의 **자바스크립트 함수**
- 특별한 규칙이 없음 → 단순한 함수와 같은데 이름 앞에 use를 붙임으로써 단순한 함수가 아닌, 리액트의 hook이라는 것을 나타내 준다.

<aside>
💡 Custom Hook의 이름은 꼭 use로 시작해야 한다!

</aside>

- 여러 개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있다.
    - 어떻게 state를 분리할까?
        
        리액트 컴포넌트는 각각의 Custom Hook 호출에 대해서 분리된 state를 얻게 되기 때문에
        
- 각 Custom Hook의 호출은 완전히 독립적이다.

# Handling Event

- Event : 특정 사건을 의미
    - ex) 버튼 클릭 이벤트
- 이벤트를 처리하는 방식 → Handling Event

- DOM의 Event

```jsx
<button onclick="activate()">
	Activate
</button
```

- 리액트의 Event

```jsx
<button onClick={activate}> //카멜표기법 사용
	Activate
</button>
```

→ 이벤트 이름의 표기법, 함수를 전달하는 방식에서 차이가 있음

- 카멜표기법: 첫글자는 소문자로 시작하되, 중간에 나오는 새로운 단어의 첫글자를 대문자로 사용

### Event Handler(Event Listener)

- 어떤 이벤트가 발생했을 때 해당 이벤트를 처리하는 역할을 하는 함수

- 함수 컴포넌트에서 이벤트 핸들러를 정의하는 방법

```jsx
function Toggle(props){
	const [isToggleOn, setIsToggleOn] = useState(true);

//방법1. 함수 안에 함수로 정의
	function handleClick(){
	setIsToggleOn((isToggleOn) => !isToggleOn);
	}

//방법2. ARROW FUNCTION을 사용해 정의
	const handleClick = () => {
	setIsToggleOn((isToggleOn) => !isToggleOn);
	}

	return (
		<button onClick={handleClick}>
			{isToggleOn ? "켜짐" : "꺼짐"}
		</button>
	);
}
```

- Argument : 함수에 주장할 내용 ~> **함수에 전달할 데이터**
- Parameter : 매개변수