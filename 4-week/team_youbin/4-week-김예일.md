# Hooks
### component
1. function component
- state 사용 불가 
- Lifecycle에 따른 기능 구현 불가

2. class component 
- 생성자에서 state 정의 
- setState() 함수를 통해 state 업데이트
- Lifecycle methods 제공

function component를 보완하기 위해 생성된 것이 Hooks
원래 존재하는 어떤 기능에 갈고리를 끼우는 것처럼 연결시키는 것을 의미한다

### useState()
- state를 사용하기 위한 Hook
- const [변수명, set함수명] =useState(초깃값)을 입력하여 사용한다.

예시
```jsx
import React, {useState} from "React";
function Counter(props){
	const [count, setCount] = useState(0);
	return (
	<div>
		<p> 총 {count}번 클릭했습니다.</p>
		<button onClick={ ( ) => setCount (count + 1)}>
			클릭
		</button>
	</div>
	);
}
```

### useEffect()
- Side Effect를 사용하기 위한 Hook
- 리액트에서 side Effect는 효과, 영향을 의미
- 사용하는 이유는 작업들이 다른 component에 영향을 미칠 수 있어, 렌더링 중에는 작업이 완료될 수 없기 때문

useEffect() 사용법
```jsx
useEffect(이펙트 함수, 의존성 배열)
```
- Effcet function이 mount, unmount 시에 단 한 번씩만 실행된다

```jsx
useEffect(이펙트 함수, [ ]);
```
- 의존성 배열을 생략하면 컴포넌트가 업데이트될 때마다 호출된다

```jsx
useEffect(이펙트 함수);
```
- useEffect( )의 return은 컴포넌트가 Unmount 될 때 호출된다

### useMemo()
- Memoized value를 반환하는 Hook

Memoization
- 연산량이 많이 드는 함수의 호출결과를 저장해 두었다가 같은 입력 값으로 함수를 호출하면 새로 함수를 호출하지 않고 저장해놨던 호출결과를 반환시킴
- 걸리는 시간 단축, 불필요한 중복연산 X, 빠른 렌더링 속도

useMemo() 사용법
```jsx
const memoizedValue = useMemo(
	() => {
		//연산량이 높은 작업을 수행해 결과를 반환
		return computeExpensiveValue(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```
- useMemo()로 전달된 함수는 렌더링이 일어나는 동안 실행됨
- 즉, 렌더링이 일어나는 동안에 실행되어서는 안 될 작업을 useMemo() 함수에 넣으면 안됨 => useEffect() Hook을 사용
- 의존성 배열을 넣지 않을 경우, 매 렌더링마다 함수가 실행됨
- 의존성 배열이 빈 배열일 경우, 컴포넌트 마운트 시에만 호출됨

### useCallback()
- useMemo() Hook과 유사하지만 값이 아닌 함수를 반환
- 컴포넌트가 렌더링 될 때마다 매번 함수를 새로 정의하는 것이 아니라 의존성 배열의 값이 바뀐 경우에만 함수를 새로 정의해서 return

useCallback() 사용법
```jsx
const memoizedCallback = useCallback (
	( ) => {
		dosomething(의존성 변수1, 의존성 변수2);
	},
	[의존성 변수1, 의존성 변수2]
);
```
- 동일한 역할을 하는 두 줄의 코드  
useCallback(함수, 의존성 배열);  
useMemo( () => 함수, 의존성 배열);
- useCallback을 사용하지 않고 컴포넌트 내에서 정의하는 함수를 자식 컴포넌트의 props로 넘겨 사용하는 경우,  
 부모의 컴포넌트가 렌더링될 때마다 매번 자식 컴포넌트도 렌더링 됨
- useCallback인 경우 특정 변수의 값이 변한 경우에만 함수 정의됨

### useRef()
- Reference를 사용하기 위한 Hook
- Reference 객체를 반환
- Refence = 특정 컴포넌트에 접근할 수 있는 객체

useRef() 사용법
```jsx
const refContainer = useRef(초깃값);
```
- useRef( ) Hook은 내부의 데이터가 변경되었을 때 별도로 알리지 않는다
- 즉, current 속성을 변경한다 해서 재 렌더링이 되는 건 아니다
- Ref의 DOM 노드가 연결되거나 분리되었을 때 어떤 코드를 실행하고 싶다면 callBack ref를 사용한다

### Hook의 규칙
- Hook은 무조건 최상위 레벨에서만 호출해야 한다
- Hook은 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 한다
- Hook은 리액트 함수 컴포넌트에서만 Hook을 호출해야 한다

### eslint-plugin-react-hooks
- Hook의 규칙을 따르도록 강제해주는 플러그인
- 리액트 컴포넌트가 hook의 규칙을 따르는지 안 따르는지 분석하고  
의존성 배열이 잘못되어 있는 경우 고칠 방법을 제안함

### Custom Hook 
- 반복되는 로직을 하나로 묶어 재사용하기 위해 Custom Hook 생성
- 이름은 꼭 use로 시작하고 내부에서 다른 Hook을 호출하는 하나의 자바스크립트 함수
- 여러 개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있다.
- 각 Custom Hook 호출에 대해서 분리된 state를 얻게되고 Custom Hook 호출 또한 완전히 독립적이다

# Handling Events
### Event
- 특정 사건을 의미
- Ex) 사용자가 버튼을 클릭하는 사건 = 버튼 클릭 이벤트
- DOM과 React의 이벤트를 사용하는 방법이 조금 다르다  
Ex) Event 이름의 표기법과 함수를 전달하는 방식

### Event Handler 
- 어떤 이벤트가 발생하면 해당 이벤트를 처리하는 역할
- Event Listener 라고도 함

함수 컴포넌트에서 이벤트 핸들러 정의
```jsx
function Toggle(props) {
	const [isToggleOn, setIsToogleOn] = useState(true);
	// 방법 1. 함수 안에 함수로 정의
	function handleClick( ) {
		setIsToggleOn( (isToggleOn) => !isToggleOn);
	}
	
	// 방법 2. Arrow function을 사용하여 정의
	const handleClick = ( ) => {
		setIsToggleOn( (isToggleOn) => !isToggleOn);
	}
	return (
		<button onClick = { handleClick }>
			{isToggleOn ? “켜짐” : “꺼짐”}
		</button>
	);
}
```
### argument 전달
- argument는 함수에 전달할 데이터를 의미
- 즉, Event Handler에 전달할 데이터
- parameter 매개변수라고도 함
- 함수 컴포넌트에서 사용하며 매개변수 순서는 상관 X

### Quiz
1. Hook은 클래스 컴포넌트에서 호출 가능하다 (O/X)
2. useCallback() Hook이 useMemo() Hook과 유사하지만 다른 점은?

# Conditional Rendering
- 조건에 따른 렌더링 (조건부 렌더링)
- 어떠한 조건에 따라서 렌더링이 달라지는 것

### Javascript의 Truthy와 Falsy
- Truthy = true는 아니지만 true로 여겨지는 값  
예시) [ ] (empty array), 42 (number, not zero)
- Falsy =false는 아니지만 false로 여겨지는 값  
예시) 0, -0 (zero, minus zero), Null, Undefined

### inline component
- 조건문을 코드 안에 집어넣는 것

Inline If
- if문의 경우 && 연산자를 사용
- True && expression => expression  
False && expression => false (쇼트서킷)
- 예시
```jsx
function Counter(props) {
	const count = 0;
	return (
		<div>
			{count && <h1> 현재 카운트: {count} </h1>}
		</div>
	);
}
```
Inline If-Else
- 삼항연산자 ? 연산자를 사용
- 조건문 ? true인 경우 : false인 경우
- 예시
```jsx
function UserStatus(props) {
	return (
		<div>
			이 사용자는 현재 <b> {props.isLoggedIn ? ‘로그인’ : ‘로그인하지 않은’}
			</b> 상태입니다.
		</div>
	);
}
```
Component 렌더링을 막는 방법
- Null을 반환하면 component가 렌더링 되지 않음

# List and keys
### List와 array
- List는 같은 아이템을 순서대로 모아놓은 것
- Array 배열은 List를 위해 사용하는 자료구조이고 Javascript의 변수나 객체들을 하나의 변수로 묶어놓은 것

### keys
- 각 객체나 아이템을 구분할 수 있는 고유한 값
- 아이템들을 구분하기 위한 고유한 문자열
- 리액트에서의 key값은 같은 List에 있는 Elements사이에서만 고유한 값이면 된다
- 즉, 속한 집단 내에서만 고유한 값이면 된다 (두 List 사이에 key가 같아도 상관없다)

key값 지정
1. Key로 값을 사용하는 경우  
배열에 중복된 값이 포함된다면 key가 중복된다는 경고
2. Key로 id를 사용하는 경우   
3. key로 index를 사용하는 경우  
default로 id가 없는 경우에만 사용 

### map()
- 여러 개의 component를 렌더링 하기 위해 사용
- 배열에 들어있는 각 변수에 어떤 처리를 한 뒤 return하는 것
- map() 안에는 key가 꼭 들어가야함 key가 없는 경우에는 경고가 뜬다
- 예시
```jsx
const numbers = [1,2,3,4,5];
const listItems = numbers.map((number) =>
	<li> {number} </li>
);
ReactDOM.render(
	<ul>{listItems}</ul>,
	document.getElementById(‘root’)
);
```

### Quiz-2
1. 고유한 값을 나타내는 key값은 서로 다른 List끼리도 중복된 값이 있으면 안 된다. (O/X)
2. inline if문의 False && expression에서 expression이 실행되지 않는 이유는?

# Form
- Form은 사용자로부터 입력을 받기 위해 사용
- 리액트는 컴포넌트 내부에서 state를 통해 내부관리
- HTML은 Element 내부에 각각의 state 존재

### Controlled component
- 리액트를 통해 사용자가 입력한 값을 접근하고 제어할 수 있음
- 값이 리액트의 통제를 받는 Input Form Element
- 모든 데이터를 setState()를 통해 state에서 관리
- 사용자의 입력을 직접적으로 제어할 수 있음

### 다양한 Forms
Textarea 태그
- 여러 줄에 걸쳐 긴 텍스트를 입력받기 위한 HTML 태그

Select 태그
- Drop-down 목록을 보여주기 위한 HTML 태그
- 여러 개의 옵션 선택을 위해서는 multiple 사용  
```jsx
<select multiple={true} value = { [‘B’, ‘C’] }>
```

File input 태그
- 디바이스의 저장 장치로부터 하나 또는 여러 개의 파일을 선택할 수 있게 해주는 HTML 태그
- File input 태그는 read-only이기 때문에 읽기 전용  
즉, uncontrolled component로 값이 리액트의 통제를 받지 않음

### Multiple inputs
- 하나의 컴포넌트에서 여러 개의 입력을 다루기 위해서 사용
- 사용법은 여러 개의 state를 선언하여 각각의 입력에 대해 사용한다

### Input null value
- input에 value를 입력하면 고정값으로 되어있는데  
이를 바꾸기 위해서는 value를 null로 반환한 뒤에  
원하는 value 값을 입력할 수 있다

# Lifting state up
- 여러 개의 컴포넌트 사이에서 state를 공유하는 방법

### Shared state
- 하나의 데이터를 여러 개의 컴포넌트에서 표현해야 하는 경우 사용한다
- 즉, 하위 컴포넌트의 state를 공통된 부모 컴포넌트로 끌어올려서 state를 적용
- 공통적으로 사용하는 요소를 상위 컴포넌트로 올리기에 코딩을 효율적으로 가능
- 리액트는 컴포넌트를 잘게 쪼개서 재사용가능한 형태로 개발하는 것이 중요하다

### Quiz-3
1. HTML과 리액트에서의 state 관리방법 차이는?
2. Form 태그 중 uncontrolled component인 것은?