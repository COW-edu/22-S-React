# Conditional Rendering

: 조건에 따른 렌더링 → 조건부 렌더링
 → 어떠한 조건에 따라서 렌더링이 달라지는 것( 조건? 프로그래밍에서 사용하는 조건문이라고 이해하면 됨! ⇒ 결과 true, false에 따라서 렌더링을 다르게 하는 것)

ex) true이면 버튼을 보여주고, false이면 버튼을 숨기는 것

- 예제코드

```jsx
function UserGreeting(props){
	return <h1>다시 오셨군요!</h1>;
}

function GuestGreeting(props){
	return <h1>회원가입을 해주세요.</h1>;
}

function Greeting(props){
	const isLoggedIn = props.isLoggedIn;

	if(isLoggedIn){
		return <UserGreeting />;
	}
	return <GuestGreeting />;
}
```

- JavaScript의 Truthy와 Falsy
- Truthy : true는 아니지만 true로 여겨지는 값
- Falsy : false는 아니지만 false로 여겨지는 값

```jsx
//Truthy
true
{}
[]
42 //0이 아닌 숫자
"0", "false" //비어있지 않은 문자열

//Falsy
false
0, -0
0n //(BigInt zero) 범위: 무한대
'',"",`` //비어있는 문자열
null
undefined
NaN //숫자가 아닌
```

## Element Value

: element를 변수처럼 다룰 수 있는 것

 - 예제코드

```jsx
function LoginButton(props){
	return (
		<button onClick={props.onClick}>
			로그인
		</button>
	);
}

function LogoutButton(props){
	return (
		<button onClick={props.onClick}>
		로그아웃
		</button>
	);
}

function LoginControl(props){
	const [isLoggedIn, setIsLoggedIn] = useState(false);
	
	const handleLoginClick = () =>{
		setIsLoggedIn(true);
	}
	
	const handleLogoutClicke = () =>{
		setIsLoggedIn(false);
	}

	*let button;
	if (isLoggedIn){
		button =
		<LogoutButton onClick={handleLogoutClick} />;
	} else {
		button =
		<LoginButton onClick={handleLoginClick} />;
	}*
	
	return (
		<div>
			<Greeging isLoggedIn={isLoggedIn} />
			*{button}*
		</div>
	)
}
```

 - isLoggedIn의 값에 따라서 button에 엘리먼트를 대입
 - 엘리먼트가 대입된 변수를 return에 넣음 → 실제로 엘리먼트가 렌더링되도록 하고 있음

## InLine Condition

: 코드를 별도로 분리된 곳에 작성하지 않고, 해당 코드의 안에 작성하는 것 → 조건문을 코드 안에 집어넣는 것

- InLine if
- if문을 필요한 곳에 직접 넣어 사용하는 것 (&& 연산자를 사용)

- 예제코드

```jsx
function Mailbox(props){
	const unreadMessages = props.unreadMessages;

	return (
		<div>
			<h1>안녕하세요.</h1>
			{unreadMessages.length > 0 && 
				<h2>
					현재 {unrdadMessages.length}개의 읽지 않은 메시지가 있습니다.
				</h2>
			}
		</div>
	);
}
```

→ unreadMessages.length가 0보다 크면 h2 태그가 실행, 그렇지 않으면 실행되지 않음

- InLine If-Else
- if-else문을 필요한 곳에 직접 넣어 사용하는 것
- 조건문의 값에 따라 다른 엘리먼트를 보여줄 때 사용 (? 연산자를 사용)

- 예제코드

```jsx
function UserStatus(props){
	return(
		<div>
			이 사용자는 현재 
			<b>{props.isLoggedIn ? '로그인' : '로그인하지 않은'}
			</b> 상태입니다.
		</div>
	);
}
```

 - 문자열이 아닌 엘리먼트를 넣어서 사용할 수도 있음!

- 컴포넌트를 렌더링하고 싶지 않을 때
→ null을 리턴!

-예제코드

```jsx
function WarningBanner(props){
	if(!props.warning){
		return null;
	}
	
	return(
		<div>경고!</div>
	);
}

function MainPage(props){
	const [showWarning, setShowWarning] = useState(False);
	
	const handleToggleClick = () => {
		setShowWarning(prevShowWarning => !prevShowWarning);
	}

	return (
		<div>
			<WarningBanner warning={showWarning}/>
			<button onClick={handleToggleClick}>
				{showWarning ? '감추기' : '보이기'}
			</button>
		</div>
	);
}
```

 - 클래스 컴포넌트의 렌더함수에서 null을 리턴하는 것은 컴포넌트의 생명주기 함수에 전혀 영향을 끼치지 않는다.