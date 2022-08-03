# 4week-2

## bind()

- 강의를 듣다보면 bind() 함수를 사용하는 것을 볼 수 있다. bind()를 사용하면 this를 활용하여 생기는 범위 오류를 해결할 수 있다.
- 사용 방법은 아래 예시와 같이 뒤에 .bind(this)를 붙여 주면 된다. bind()를 활용하여 handleClick() 함수가 호출되는 this 범위가 Toggle 컴포넌트에 묶인 것을 볼 수 있다.
    
    ```jsx
    ...
    class Toggle extends React.Component {
    	constructor(props) {
    		super(props);
    		this.state = {isToggleOn: true};
    		this.handleClick = this.handleClick.bind(this);
    	}
    	handleClick() {
    		this.setState(prevState => ({
    			isToggleOn: !prevState.isToggleOn
    	}));
    }
    render() {
    	return (
    		<button onClick = {this.handleClick}>
    			{this.state.isToggleOn ? '켜짐' : '꺼짐'}
    		</button>
    		);
    	}
    }
    ```
    
- bind 함수가 번거로워서 사용하기 싫다면, **class fields syntax**를 사용하는 방법도 있다.
    
    ```jsx
    class CountApp extends React.Component {
    	handleClick = () => {
    		console.log('this is', this);      //이 부분이 class fields syntax이다. 
    	}
    ...
    }
    ```
    
- 앞 선 방법 두가지 모두 사용하기 싫다면 , **Arrow function**을 사용하는 방법도 존재한다. (하지만, 렌더링 될 때마다 다른 콜백함수가 생성되기 때문에 성능에 문제가 생길 수도 있어서 권장하지 않는다. )
    
    ```jsx
    ...
    render() {
    	return (
    		<button onClick= {() => this.handleClick()}> 
    			클릭
    		</buttion>
    	);
    }
    ```
    

## Component에서 DOM 객체 함수 사용하기

- Coponent에서 addEventListener() 함수와 같은 Dom 객체 함수를 사용하려면 DOM 객체를 컴포넌트 변수에 할당해야 한다. 이때 특수 프로퍼티 ref를 사용한다 !
- ref 프로퍼티는 document.getElementById()가 반환하는 객체를 반환한다. 단 ref 프로퍼티는 DOM 객체 함수가 필요한 Element에 콜백 함수 형태로 전달된다.
    
    

## Event

- Component에서 Event 프로퍼티는 특수 프로퍼티로 콜백 함수 형태로 전달해서 처리한다.
- JSX 내에서는 ‘ on + 이벤트이름 ‘  형태의 프로퍼티가 제공된다. ( 이때 이벤트 이름은 카멜 표기법을 따른다. )
    - camel case 란 ? 첫 글자는 소문자로 시작하되 중간에 나오는 새로운 단어는 대문자로 사용하는 것을 낙타의 등 모양과 비슷하다해서 따온 이름이다.
- 아래는 자주 사용되는 이벤트 프로퍼티이다.
    - onClick , onSubmit, onMouseMove, onMouseOver, onMouseOut, onKeyDown, onKeyPass 등등이 있다.

```jsx
import React from 'react';

class Count extends React.Component {
	constructor(props) {        
		super(props);
		this.state = { 
		count : 0,     // 초기화 과정
		};
		this.increateCount = this.increateCount.bind(this);   // 범위 오류를 줄이기 위해 사용
		}
	increateCount() {
		this.setState(({count}) => ({count : count +1}));
		} 
	render() {
		return(
			<div>
				현재 카운트 : {this.state.count}	
				<button
					onClick={this.increateCount}  // 버튼을 누를 때 마다 카운트가 증가하는 함수 호출
				>
				카운트 증가 </div>
		);
	}
}
```

## Conditional Rendering

- Conditional rendering이란, 어떠한 조건에 따라서 렌더링이 달라지는 것을 의미한다. (조건문이라고 생각하면 쉽다.) 결과에 따라서 렌더링을 다르게 하는 것이다.
- 자바 스크립트에서는 truthy와 falsey가 존재한다. 개발자를 혼동 시키지만, 잘 활용하면 유용하게 사용할 수 있다고 한다. (나도 혼동중)
    - truthy는 true는 아니지만 true로 여겨지는 값이다. (ex. true, {} , [], “0” (string), “false”(not empty))
    - falsy는 false는 아니지만 false로 여겨지는 값이다. (ex. false, 0, 0n, ‘’ , null, undefined, NaN)
- 조건부 렌더링을 사용하다보면, 렌더링해야 하는 컴포넌트를 객체처럼 다루고 싶을 때가 존재하는데  그럴 때는 **Element Variable**를 활용한다. 리액트의 element를 변수처럼 다루는 것이다.
    
    ```jsx
    //로그인 , 로그아웃 컴포넌트이다.
    function LoginButton(props) {
    	return (
    		<button onClick = {props.onClick}>
    			로그인
    		</button>
    	);
    }
    function LogoutButton(props) {
    	return (
    		<button onClick = {props.onClick}>
    			로그아웃
    		</button>
    	);
    }  
    
    // 아래 코드는 로그인, 로그아웃 컴포넌트를 변수처럼 사용한 예시이다. 
    
    function LoginControl(props) {
    	const [isLoggedIn,setIsLoggedIn]= useState(false);
    	
    	const handleLoginClick = () => {
    		setIsLoggedIn(true);
    	}
    	const handleLogoutClick = () => {
    		setIsLoggedIn(false);
    	}
    
    	let button;
    	if(isLoggedIn) {
    		button = <**LogoutButton** onClick = {handleLogoutClick}/>;
    	} else {
    		button = <LoginButton onClick = {handleLoginClick}/>;
    	}
    
    	return (
    		<div>
    			<Greeting isLoggedIn={isLoggedIn}/>
    			{button}
    		</div>
    	)
    }
    ```
    
- Component를 렌더링 하고 싶지 않을 때는 .. ? → null을 리턴 값으로 받으면 된다.

## Inline Conditions

- : in  + line 이라는 말에 맞게 코드를 라인 안에 작성하는 것을 의미한다. 조건문을 코드 안에 집어 넣는 것인데 Inline if, Inline if-else가 존재한다.
- **Inline if**는 if문을 필요한 곳에 작성하는 형태인데, 실제 if문을 사용하는 것이 아니라 비슷한 효과를 주기 위해 && 연산자를 사용한다.
    - 조건문 && expression → 앞에 조건문이 true일 경우에는 뒤에 expression이 실행된다. 반면에 조건문이 false 경우에는 전체 결과가 false가 됨으로 뒤에 표현식을 볼 필요가 없다. **하지만 ! 결과값은 리턴 받는다 !!**
    
    ```jsx
    function Mailbox(props) {
    	const unreadMessages = props.unreadMessages;
    
    	return (
    		<div>
    			<h1>hi</h1>
    			{unreadMessages.length > 0 &&    
    				<h2>
    					현재 {unreadMessages.length}개의 읽지 않은 message가 존재합니다.
    				</h2>
    			}
    		</div>
    	);
    }   //unreadMessages의 길이가 1 이상이면 뒤에 표현식이 렌더링되고 
    		// unreadMessages 0이라면 표현식이 렌더링이 되지 않는다.  
        // 결과적으로 현재 0개의 읽지 않는 message가 존재한다고 알려줄 것이다.
    ```
    
- **Inline if-else**는 조건문의 값에 따라서 다른 element를 보여줄 때 사용한다.’ ? ‘ 연산자를 사용하여 나타낸다.
    - 삼항 연산자를 사용한다. condition ? true : false 형태이다.
    
    ```jsx
    function UserStatus(props) {
    	return (
    		<div>
    			이 사용자는 현재 <b>{props.isLoggedIn ? :'로그인' :'로그인 하지 않은'}</b>상태임.
    		</div>
    	)
    }
    ```
    

## List의 Key

- key는 고유한 형태가 들어가야 한다. (ex. 주민등록번호, id, 이메일 등등)
- key로 값을 사용할 경우
    
    ```jsx
    const numbers =[1,2,3,4,5]
    const listItems = numbers.map((number) => 
    	<li key ={number.toString()}>  // numbers 배열에 중복된 값이 있으면 오류가 생긴다.
    		{number}                     // key 값은 고유해야 하기 때문이다.
    	</li>
    );
    ```
    
- key로 id를 사용하는 경우
    
    ```jsx
    const todoItmes = todo.map((todo)=>
    	<li key={todo.id}>     //id의 의미 자체가 고유한 값을 나타내기 때문에 매우 적합하다.
    		{todo.text}
    	</li>
    );
    ```
    
- **map() 함수 안에 있는 Element는 꼭 key가 필요하다 !**

## Form

- 사용자로부터 입력을 받기 위해 사용하는 양식이다. (ex. 회원가입을 하거나, 로그인을 할 때 사용하는 입력창을 생각하면 쉽다.)
- HTML의 폼보다 리액트를 활용하여 form을 작성하면 사용자가 입력한 값에 접근하고 제어하는 것이 편리하다.
    - HTML은 각각의 form들이 자체적으로 state를 관리한다는 불편함이 있다. 자바스크립트를 통해 각각의 form 값들에 접근하기가 쉽지 않다.
    - 리액트는 controlled component를 활용하여 모든 데이터를 state에서 관리한다.
- Controlled Component : 이름 그대로 값이 누군가(리액트)에게 통제를 받는 Input Form Element이다. 그렇기 때문에 사용자의 입력을 직접적으로 제어가 가능하다.
    
    ```jsx
    function NameForm(props) {
    	const [value,setValue] = useState('');
    
    	const handleChange = (event)=> {    
    	setValue(event.target.value);    //제어 컴포넌트를 통해 직접적으로 제어한다. 
    	}                                //setValue를 사용하여 값을 업데이트 하는 것이다.
    ...
    	return (
    		<form>
    		<label>
    		이름 : 
    			<input type="text" value={value} onChange={handleChange}> 
    		</label>   //콜백함수형태로 이벤트를 처리한다. 
    		</form>
    	)
    }
    ```
    
- Form 의 종류
    - textarea : 여러 줄에 걸쳐 긴 텍스트를 입력받기 위해 사용하는 html 태그이다. value 속성을 사용한다.
    
    ```jsx
    //위의 예시에서 input만 textarea로 바꾼 형태이다. 
    <textarea value={value} onChange={handleChange}> 
    ```
    
    - select : drop-down 목록을 보여주기 위해 사용하는 html 태그이다.
    
    ```jsx
    <select>
    	<option value ="apple">사과</option>
    	<option selected value="grape">포도</option>  // 포도가 선택된 상태로 출력된다.
    </select>
    
    //다른 예시이다.
    const [value, setValue] = setState('grape'); //초기값으로 grape를 넣어준다.
    ...
    <select value={value}>
    	<option value ="apple">사과</option>
    	<option value="grape">포도</option>
    </select>
    ```
    
    - file input : 서버로 파일을 업로드 하거나 파일을 다룰 때 사용한다. (Uncontrolled Component이다. 읽기 전용이기 때문에 값이 리액트의 통제를 받지 않는다. )
    
    ```jsx
    <input type="file"/>;     
    ```
    
- 만약 하나의 component에서 여러개의 form을 다루고 싶다면 ? → **여러개의 state를 선언하여 각각의 입력에 대해 사용하면 된다.**
- value props을 정해진 값으로 넣으면 코드를 수정하지 않는한 입력값을 바꿀 수 없다. value props은 넣되, **자유롭게 값을 변경하고 싶다면 값에 undefined 혹은 null을 넣어주면 된다.**
    
    ```jsx
    ReactDOM.render(<input value="hi" />, rootNode); 
    // input의 값이 hi로 정해져있다. 
    
    setTimeout(function() {
    	ReactDOM.render(<input value={null}/>,rootNode);
    },1000);  // 1 초뒤에 value가 null로 바뀌면서 입력이 자유로워진 형태가 된다. 
    ```