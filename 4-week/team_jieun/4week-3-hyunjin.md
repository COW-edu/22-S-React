# Lifting State Up

- 개발을 하다 보면 하나의 데이터를 여러 개의 컴포넌트에서 표현해야 하는 경우가 생김 → 가장 가까운 공통된 부모 컴포넌트의 state를 공유해서 사용하는 것이 효율적
- Shared State : 공유된 state
→ 하위 컴포넌트가 공통된 부모 컴포넌트의 state를 공유해 사용하는 것

# Composition and Inheritance

## Composition(구성)

: 여러 개의 컴포넌트를 합쳐 새로운 컴포넌트를 만드는 것 → ‘합성’ 이라는 뜻에 더 가까움

- 컴포넌트를 무작정 붙이는 것이 아니라 여러 개의 컴포넌트들을 어떻게 조합할 것인가에 대해 생각해야 한다.

### composition 방법

- Containment : 하위 컴포넌트를 포함하는 형태의 합성 방법

- sidebar나 dialog같은 box 형태의 컴포넌트는 자신의 하위 컴포넌트를 미리 알 수 없다. (해당 컴포넌트를 사용하는 개발자가 어떤 것을 넣느냐에 따라서 하위 컴포넌트가 달라지기 때문)

- children prop을 사용한다.

```jsx
function FancyBorder(props){
	return(
		<div className={'FancyBorder FancyBorder-' + props.color}>
			{props.children}
		</div>
	);
}

function WelcomeDiaglog(props){
	return(
		<FancyBorder color="blue">
			<h1 className="Dialog-title">
				어서오세요
			</h1>
			<p className="Dialog-message">
				우리 사이트에 방문하신 것을 환영합니다!
			</p>
		</FancyBorder>
	);
}
```

 - h1태그 , p태그는 모두 FancyBorder 함수의 props로 들어가게 된다. → 파란색으로 감싸지는 형태가 만들어지게 됨

- 여러 개의 children 집합이 필요한 경우?
   - 별도로 props를 정리해서 각각 원하는 컴포넌트를 넣어준다.

```jsx
function SplitPane(props){
	return(
		<div className="SplitPanme">
			<div className="SplitPane-left">
				*{props.left}*
			</div>
			<div className="SplitPane-right">
				*{props.right}*
			</div>
		</div>
	);
}

function App(props){
	return(
		<SplitPane left={<Contacts />} right={<Chat />} />
	);
}
```

- Specialization : 범용적인 개념을 구별이 되도록 구체화 하는 것

```jsx
function Dialog(props){
	return(
		<FancyBorder color="blue">
			<h1 className="Dialog-title">
				{props.title}
			</h1>
			<p className="Dialog-message">
				{props.message}
			</p>
		</FancyBorder>
	);
}

function WelcomeDialog(props){
	return{
		<Dialog title="어서 오세요" 
			message="우리 사이트에 방문하신 것을 환영합니다!" />
	);
}
```

- Containment와 Specialization을 같이 사용하기

```jsx
function Dialog(props){
	return(
		<FancyBorder color="blue">
			<h1 className="Dialog-title">
				{props.title}
			</h1>
			<p className="Dialog-message">
				{props.message}
			</p>
			*{props.children}*
		</FancyBorder>
	);
}
```

```jsx
function SignUpDialog(props){
	const [nickname, setNickname] = useState('');
	
	const handleChange = (event) => {
		setNickname(event.target.value);

	const handleSignUp = () => {
		alert(`어서 오세요, ${nickname}님!`);
	}

	return(
		<Dialog
			*title="화성 탐사 프로그램"
			message="닉네임을 입력해 주세요."*>
			**<input value={nickname} onChange={handleChange} />
			<button onClick={handleSignUp}>
				가입하기
			</button>**
	);
}
```

 - 이탤릭체: Specialization

 - 볼드체: containment

## Inheritance

: 다른 컴포넌트로부터 상속을 받아서 새로운 컴포넌트를 만드는 것

- 추천하지 않는 방법임!

## 결론

<aside>
💡 복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 만들고, 만든 컴포넌트들을 조합해서 새로운 컴포넌트를 만들자!

</aside>

# Context

- 기존의 리액트에서는 데이터가 컴포넌트의 props를 통해 부모→ 자식의 단방향으로 전달이 되었는데, 이에 따라 코드가 복잡해지고 사용하기에 불편함이 많았다.
- 그래서 나오게 된 것이 context!
- 컴포넌트 트리를 통해 곧바로 데이터를 필요로 하는 컴포넌트로 데이터를 전달하는 방식을 사용!

- 언제 context를 사용하면 좋을까?
- 여러 개의 컴포넌트들이 접근해야 하는 데이터 → 로그인 여부, 로그인 정보, UI 테마, 현재 언어 etc.
- context를 사용하기 전에 고려할 점
- 컴포넌트와 context가 연동된다면 재사용성이 떨어지게 된다. 따라서 다른 레벨의 많은 컴포넌트가 데이터를 필요로 하는 것이 아니라면 기존의 방법을 사용하는 것이 좋다.

## Context 사용하기

- Context 생성

```jsx
const MyContext = React.createContext(기본값);
```

 - 리액트에서 렌더링이 일어날 때, context객체를 구독하는 하위 컴포넌트가 나오면 현재 context의 값을 가장 가까이에 있는 상위 레벨의 Provider로부터 받아온다.

  - 만약 상위 레벨에 매칭되는 Provider가 없다면 기본값이 사용된다!
  - 기본값으로 undefined를 넣으면 기본값이 사용되지 않는다.

- Context.Provider : 하위 context들이 해당 컴포넌트의 값을 받을 수 있도록 설정할 때 사용! → **데이터를 제공해주는 컴포넌트**라는 의미로 이해하면 된다. 

- 모든 context 객체는 Provider라는 리액트 컴포넌트를 가지고 있다.

```jsx
<MyContext.Provider value={/* some value */}>
```

 - value → provider 컴포넌트의 하위에 있는 컴포넌트들에게 전달됨
 - 하위 컴포넌트들이 데이터를 소비한다는 뜻에서 consuming component라고 부름
 - consuming component는 context 값의 변화를 지켜보다가 만약 값이 변하게 되면 재 렌더링 됨

 - 하나의 provider component은 여러 개의 consuming component과 연결될 수 있으며, 여러 개의 provider component은 중첩되어 사용할 수 있다.

- 주의 사항
- provider 컴포넌트가 재렌더링 될 때마다 모든 하위 consumer 컴포넌트가 재렌더링 됨 ( value props을 위한 새로운 객체가 매번 새롭게 생성되기 때문!) 
  → **state를 사용해 불필요한 재렌더링을 막는다.**