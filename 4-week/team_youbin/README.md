# Team 유빈

# React

### Component
    • 컴포넌트를 통해 UI를 재사용 가능한 개별적인 여러 조각으로 나눈다.
    • 컴포넌트는 개념적으로 JavaScript 함수와 유사하다. 'props'라는 입력을 받은 후, 화면에 어떻게 표시되는지 기술하는 React 엘리먼트를 반환한다.
    • 엘리먼트는 일반 객체(plain object)로 React 앱의 가장 작은 단위다. 엘리먼트는 컴포넌트의 '구성 요소'다.
    • 컴포넌트를 선언하는 방식에는 함수형 컴포넌트와 클래스형 컴포넌트가 있다.

🌱 Function Component
```js
function Welcome(props) {
    return <h1>안녕, {props.name}</h1>;
}
```

### Hooks

fuction component는 class component와는 다르게 state사용이 불가하고 Lifecycle에 따른 기능 구현이 불가능하다

→ Hooks로 해결(class component의 기능 사용 가능)

훅은 원하는 시점에 state, Lifecycle 관련 함수를 실행하게 한다. 

모두 use를 앞에 붙혀 사용

**대표적인 Hook**

- `useState()` : state를 사용하기 위한 Hook

    ```jsx
    const[변수명, set함수명] = useState(초기값);
    ```

- `userEffect()` :

    Side effect를 사용하기 위한 Hook

     생명주기 함수와 동일한 효과 

    ```jsx
    //의존성 배열의 값 중 하나라도 변경될 시 함수 실행
    useEffect(이펙트 함수, 의존성 배열);
    
    //mount, unmount 시에 한 번씩만 실행
    useEffect(이펙트 함수,[]);
    
    //Component가 업데이트 될 때마다 실행
    useEffec(이펙트 함수);
    
    useEffect(() => {
    	return () => {
    		// 컴포넌트 마운트가 해제되기 전 실행
    	}
    },)
    ```

- `userMemo()` :

    Component 랜더링 시 연산량이 높은 작업의 반복을 방지

    ```jsx
    const memoizedValue = useMemo(
    	() => {
    		//연산량이 높은 작업을 수행하여 결과를 반환
    		return computeExpensiveValue(의존성 변수1, 의존성 변수2);
    	},
    	[의존성 변수1, 의존성 변수2]
    );
    ```

    userMemo의 함수는 랜더링이 일어나는 중에 실행되기 때문에 랜더링 중 실행되면 안되는 작업을 작성하면 안된다.

- `userCallback` : userMemo와 거의 똑같지만 값이 아닌 함수를반환
- `useRef()` :

    특정 컴포넌트에 접근할 수 있는 객체(Reference)를 사용하기 위한 Hook 

    ```jsx
    const refContainer = userRef(초기값);
    ```

    ### Hook의 규칙
- Hook은 무조건 최상위 레벨에서만 호출해야 한다
- Hook은 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 한다
- Hook은 리액트 함수 컴포넌트에서만 Hook을 호출해야 한다

### eslint-plugin-react-hooks
- Hook의 규칙을 따르도록 강제해주는 플러그인
- 리액트 컴포넌트가 hook의 규칙을 따르는지 안 따르는지 분석하고  
의존성 배열이 잘못되어 있는 경우 고칠 방법을 제안함

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
### HTML에서와의 차이점
    1) React 이벤트 이름은 소문자 대신 camelCase를 사용
    2) JSX에 문자열 대신 함수를 전달

    html에서는 아래와 같이 이벤트를 넣었다면,
```js
<button onClick="activateButton()">클릭하세요</button>
```
    React에서는 이벤트이름을 onClick, onSubmit 등과 같이 camelCase로 설정한다는 차이점이 있습니다. event handler는 JSX 표기인 { }를 사용하여 연결합니다.
```js
<button onClick={activateButton}>클릭하세요</button>
```

### Conditional Rendering

어떠한 조건에 따라 렌더링이 달라지는 것

Element Variable은 React Component를 변수처럼 사용하는 방법

**Inline Condtions**

- &&
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
- 삼항 연산자

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

컴포넌트를 랜더링 하고 싶지 않을 때는 null을 return

## map() - List렌더링

### List와 array
- List는 같은 아이템을 순서대로 모아놓은 것
- Array 배열은 List를 위해 사용하는 자료구조이고 Javascript의 변수나 객체들을 하나의 변수로 묶어놓은 것

### keys
- 각 객체나 아이템을 구분할 수 있는 고유한 값
- 아이템들을 구분하기 위한 고유한 문자열
- 리액트에서의 key값은 같은 List에 있는 Elements사이에서만 고유한 값이면 된다
- 즉, 속한 집단 내에서만 고유한 값이면 된다 (두 List 사이에 key가 같아도 상관없다)

```jsx
const numbers = [1,2,3,4,5];
const listItems = numbers.map((number) =>
	<li key={}>
			{number}
	</li>
);
```

**key값 지정**

1. 값으로 지정: `<li key={numbers.toString()}>` 

    → 배열 값이 중복이면 key값의 중복이 생길 수 있다.

2. id로 지정
3. index로 지정: id가 없을때만 사용, default

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

### Lifting state up
- 여러 개의 컴포넌트 사이에서 state를 공유하는 방법

### Shared state
- 하나의 데이터를 여러 개의 컴포넌트에서 표현해야 하는 경우 사용한다
- 즉, 하위 컴포넌트의 state를 공통된 부모 컴포넌트로 끌어올려서 state를 적용
- 공통적으로 사용하는 요소를 상위 컴포넌트로 올리기에 코딩을 효율적으로 가능
- 리액트는 컴포넌트를 잘게 쪼개서 재사용가능한 형태로 개발하는 것이 중요하다

# Composition vs Inheritance

**Composition:** 여러개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것

Composition 기법

- Containment: 하위 컴포넌트를 포함하는 형태의 합성 방법
    
    `props.children` 을 통해 하위 컴포넌트를 전달받음
    
    여러개의 children 집합이 필요할 때는 각각 새로운 props에 자식 컴포넌트를 정의
    
- Specialization: 범용 컴포넌트를 만들어 놓고 이를 특수화 시킨 컴포넌트를 만드는 합성 방법

**Inheritance**: 다른 컴포넌트로부터 상속받아 새로운 컴포넌트를 만드는 것

**복잡한 컴포넌트를 쪼개 여러 개의 컴포넌트로 만들고, 만든 컴포넌트들을 조합해 새로운 컴포넌트를 만든다.**

# Context

기존의 props를 통해 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달하는 방식은 여러 컴포넌트에 자주 사용되는 데이터를 전달할 때 반복적으로 코드를 작성해야한다.

→ Context사용

**Context 사용 고려**

여러 개의 Component들이 접근해야 하는 데이터 

ex)로그인 여부,정보, 현재 언어, UI 테마 등;

**Context 사용 방법**

```jsx
// Context 생성
const Context = React.createContext("기본값");

// Context 사용
fuction App(props){
	return(
		<Context.Provider value="a">
				<Component />
		</Context.Provide>
```